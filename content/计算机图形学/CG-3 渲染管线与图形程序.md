---
publish: true
---

# CG-3 渲染管线与图形程序
### $\it 0.1$ Intro
回顾图形渲染的整个流程：
- CPU 的任务：设置模型的样子（顶点长什么样），然后计算变换 MVP 以及相机和 perspective 的矩阵，并一并将这些数据传给 GPU。
- GPU：进行很多处理，接收到数据之后，根据模型的每一个点计算其经过所有变换矩阵之后在屏幕上的坐标（Vertex Shander），然后进行图元装配、光栅化，以及 Fragment Shader（计算颜色还有纹理什么的），最后深度测试以及投射到屏幕。

我们现在所处的情景是，CPU 将所渲染所需要的数据都处理好了，需要将其交给 GPU 进行运算。所需要传递的数据基本有三类：
1. 图形的顶点信息：每个顶点是什么，按照怎样的模式和结构组织在一起。
2. 变换信息（uniform）：相机的位置和视角、模型的变换矩阵（MVP）以及模型内部的变换。
3. 纹理信息：本质上是图片。

其中顶点信息和纹理信息数据量很大（尤其针对复杂模型），因此在程序的预处理环节就已经传到了 GPU 中。而变换信息是频繁变化的（每帧都不一样），但数据量小（只涉及较少矩阵）。对应 GPU 也有相应的结构，VBO（Vertex Buffer Object）等属于显存（静态大量数据的储存），而常量缓存（Constant Cache）则存储 uniform 信息等经常变化的信息。

CG Lecture 3 的内容聚焦 GPU 拿到数据之后，如何进行进一步的处理。
## $\mathrm I$ 基础光栅管线部分

### $\it 1.1$ Vertex Assembly 顶点装配
顶点装配是管线的第一个阶段。它的任务是从 CPU 准备的缓冲区里（VBO），把顶点数据拉取（pull）出来，整理成顶点着色器能处理的形式。对于每一个顶点，其有三个属性：$(\mathbf p, \mathbf n, \mathbf t)$。
- $\mathbf p\in\mathbb R^3$：描述这个顶点的位置。
- $\mathbf n\in\mathbb R^3$：描述这个顶点处于原模型切平面的法向向量。
- $\mathbf t\in\mathbb R^2$：描述其纹理对应纹理图片的哪个坐标（图片是二维的）。

那么抽象地来说，Vertex Assembly 的任务就是从 VBO（不一定直接的）压缩信息中重新提取出每个点的 $(\mathbf p, \mathbf n, \mathbf t)$，供 Vertex Shader 处理（总之是提取出每个顶点所需要的所有信息）。
- 其中在 VBO 中，可以将每个顶点的信息理解成一个 `struct Node {...};`，一个顶点的数据量就相当于 `sizeof Node`，偏移量就是每个成员在 `Node` 内部的位置。

更多细节暂时不追究。
### $\it 1.2$ Vertex Shader 顶点着色器
Vertex Shader 的职责是，接收到每个顶点的坐标等数据之后，将其进行矩阵变换，使顶点的位置由**模型坐标系**（一开始的坐标）变为**裁剪坐标系**（左乘 `perspective` 之后），并对顶点的额外数据作处理（其他附加信息，但坐标、法向以及纹理坐标是基础）。是这条管线上的第一个可编程部分（GLSL 语言，在 C++ source code 中以 `const char*` 的形式传递给 GPU）。

其可以理解为一个函数，输入是每个顶点的位置、法向、纹理坐标等等信息。
- 其中 `in ...` 表示逐个顶点输入。
- `uniform` 表示全局信息（比如相机参数）。这部分信息在一次绘制中（一次 Draw Call）全局共享。

其输出内容为 `gl_Position` 表示每个顶点在裁剪坐标系中的坐标，以及 `pass_texcoord` 表示（原样传递）纹理坐标（注：给到 Fragment Shader 的坐标已经经过了这两者之间的自动处理，即光栅化等）。

`uniform u_mvp`：表示 Uniform MVP 矩阵，其中 model $M$ 表示由模型坐标系到世界坐标系，view $V$ 表示世界坐标系到相机坐标系，projection $P$ 表示相机坐标系到裁剪坐标系，矩阵按顺序做成即得到复合矩阵 `u_mvp = P * V * M`。

注意：Vertex Shader 只负责对每个点进行运算处理，不知晓三角形的存在。
### $\it 1.3$ Primitive Assembly 原始装配
这是一个 GPU 固定、不可编程的阶段。其任务是，将 Vertex Shader 计算得到的顶点信息，按照索引（EBO，Element Buffer Object）组装为**图元**。
- **图元**。图元是 GPU 能够处理的基本形状，其包括点、线段、三角形、三角形带、三角形扇等，最常用的是三角形。

以三角形图元为例，EBO 中指出了“哪几个顶点组成一个三角形”，那么这一步就是将这些点组织为三角形（比如 `struct Triangle { Vertex A, B, C; };`）其中这些顶点是经过 Vertex Shader 处理之后的信息（裁剪坐标系内），每个基本包含 `position = (px, py, pz, pw), texcoord = (u, v), normal = (nx, ny, nz)` 等。

Primitive Assembly 是自动进行的，只需要在代码中调用 `glDrawElements(GL_TRIANGLES, indexCount, GL_UNSIGNED_INT, 0);` 的 Draw Call。
### $\it 1.4$ Rasterization 光栅化
光栅化是 GPU 中固定但核心的功能，在这一步中几何图形由顶点信息走向像素信息，其将连续的三角形投射为一个个像素。
#### $\it 1.4.1$ 覆盖测试
经过 Vertex Shader 处理，每一个顶点的坐标在裁剪坐标系中形如 $(x_{\mathrm{clip}}, y_{\mathrm{clip}}, z_{\mathrm{clip}}, w_{\mathrm{clip}})$。之后将自动除以 $w_{\mathrm{clip}}$（透视除法）得到 NDC 坐标 $(x_{\mathrm{ndc}}, y_{\mathrm{ndc}}, z_{\mathrm{ndc}})$。这里 NDC 即为 $[-1, 1]^3$ 中归一化坐标。其还会经过视口变换，映射到屏幕的像素坐标 $(x, y)_{\mathrm{screen}}$（使用 $x_{\mathrm{ndc}}, y_{\mathrm{ndc}}$ 进行映射，这个坐标的单位是像素）。接下来的覆盖测试即采用像素坐标进行运算。
- 即：此时所有的顶点都被映射到了像素坐标系下。
- 在透视除法（除以 $w_{\mathrm{clip}}$）之前，有裁剪过程。如果三角形完全在 $[-w, w]$ 之外，那么就会被丢掉。若三角形跨越了相应 $[-w, w]$ 的边界，则会产生新的顶点。

覆盖测试的做法是，对屏幕上的每个像素，其中心（像素）坐标是 $P$，则对每个三角形计算，$P$ 是否在该三角形内部。
- 常用的做法是，假设 $\Delta ABC$ 是待判断的像素三角形，那么 $P$ 在 $\Delta ABC$ 内部，当且仅当 $\mathrm{sgn}(\overrightarrow{AB}\times\overrightarrow{AP})=\mathrm{sgn}(\overrightarrow{BC}\times\overrightarrow{BP})=\mathrm{sgn}(\overrightarrow{CA}\times\overrightarrow{CP})$。
- 如果 $P$ 落在三角形的边界上，那么则需要某种统一规则来判定（例如：上边和左侧算内部）。
#### $\it 1.4.2$ Fragment 片元与插值
接下来需要生成片元。所谓片元就是候选像素，其包含屏幕坐标（像素的坐标）、深度值、以及各种插值之后的属性。

**插值**。在原点 $O$ 存在时（像素坐标），三角形 $\Delta ABC$ 内部的任意一点 $P$ 可以用**重心坐标**表示，即存在 $\lambda_1, \lambda_2, \lambda_3$ 使得 $\overrightarrow{OP}=\lambda_1\overrightarrow{OA}+\lambda_2\overrightarrow{OB}+\lambda_3\overrightarrow{OC}$。
- 参数计算方法：$\lambda_1=\dfrac{S_{\Delta PBC}}{S_{\Delta ABC}}$，其余同理。$\lambda_1>0\land\lambda_2>0\land\lambda_3>0\iff P$ 在 $\Delta ABC$ 内部。
- 得到重心坐标之后，即可利用该比例来插值内部像素坐标的属性，实现三角形内部每个点到其三个顶点的平滑变换。以颜色为例，内部每个点 $P$ 的颜色即为三个顶点的 RGB 三元组按照 $\lambda_i$ 加权（插值）的结果。
- 常见的插值对象有：颜色、法向量 $\mathbf n$、纹理坐标 UV、深度（NDC 中的 $z$ 坐标）。
- **注意**。三角形的插值需要按照自然坐标系的坐标来，此处屏幕坐标系经过了非线性处理，因此直接按照屏幕坐标计算 $\lambda_i$ 是错误的（会扭曲）。正确的做法是（数学推导的结果），$x_P=\dfrac{\sum\limits_{i=1}^nx_i\cdot\dfrac{\lambda_i}{w_i}}{\sum\limits_{i=1}^n\dfrac{\lambda_i}{w_i}}$。其中 $\lambda_i$ 是按照像素坐标得出的重心坐标，而 $w_i$ 是 Vertex Shader 处理的结果的齐次项 $w_{\mathrm{clip}}$。

总之，这一步得到了每个片元（备选像素）的像素坐标、颜色插值、法向量插值、纹理插值、深度插值等等。
### $\it 1.4.3$ 有关插值坐标的数学推导
为什么按照 $\dfrac1w$ 来插值？我们需要建立一个几何模型（相机坐标到裁剪坐标）：原点为 $O$，设想空间中有任意三个点 $A, B, C$，其组成了一个三角形。现在我们将这个三角形按照透视投影的方法将其投射到 $z=1$ 的平面，得到了 $A'B'C'$。那么 $\overrightarrow{OA}=z_A\overrightarrow{OA'}$（对 $B, C$ 同理）。

假设原三角形平面上的 $P$ 的投影结果是 $P'$，则可以写出两组插值权重的关系：
$$
\begin{cases} \overrightarrow{OP}=\alpha_A\overrightarrow{OA}+\alpha_B\overrightarrow{OB}+\alpha_C\overrightarrow{OC} \\ \overrightarrow{OP'}=\lambda_A\overrightarrow{OA'}+\lambda_B\overrightarrow{OB'}+\lambda_C\overrightarrow{OC'} \end{cases}
$$
同理有 $\overrightarrow{OP}=z_P\overrightarrow{OP'}$，所以 $\overrightarrow{OP}=\dfrac{z_P\lambda_A}{z_A}\overrightarrow{OA}+\dfrac{z_P\lambda_B}{z_B}\overrightarrow{OB}+\dfrac{z_P\lambda_C}{z_C}\overrightarrow{OC}$，对应得到 $\alpha_A=\dfrac{z_P\lambda_A}{z_A}$（对 $B, C$ 同理）。

在实际管线中，投影矩阵计算的结果的第四维 $w_{\mathrm{clip}}=-z$ 正好保存了原来相机坐标中点的 $z$ 分量。因此对于**屏幕**坐标得出的重心插值权重 $\lambda_i$，应当按照实际（世界坐标系）比例 $\alpha_i$ 进行插值（计算法向、纹理等），否则渲染会扭曲。而有一些参数可以直接按照 $\lambda_i$ 插值（比如片元的深度信息）。
- 注：在相机坐标系中，使用的四维向量可以保证第四维 $w=1$，因为其初始默认值就是 $1$，且 $M$ 与 $V$ 矩阵不会改变第四维 $w$ 的值。因此其对应真实的 $z$。

实际做法正是将每一个 $\lambda_i$ 除以 $w_i$ 然后归一化，即得到 $\alpha_i$。
### $\it 1.5$ Fragment Shader 片元着色器
Fragment Shader 是管线中第二个可编程部分。与 Vertex Shader 一样，通过一个源代码中的 `const char*` 来传递给 GPU。其作用为对每一个片元处理，考虑其光照、纹理与材质之间的交互，输出最终的颜色。

其输入为 `pass_texcoord`（光栅化插值的 UV 纹理坐标）以及 `pass_normal`（光栅化插值之后的法向量，在世界坐标系下或者相机坐标系），这些来自 Vertex Shader 的输出。以及一些通用的相关参数：
```glsl
uniform sampler2D u_diffuse;   // 纹理
uniform vec3 u_light_dir;      // 光源方向
uniform vec3 u_light_color;    // 光源颜色
uniform vec3 u_view_pos;       // 相机位置
```
其输出即为每一个片元通过其程序计算之后的最终 RGBA 四元组。其功能在于纹理混合、阴影计算、后处理、程序化纹理等。
### $\it 1.6$ Per-Fragment Test 逐片元测试
通过 Fragment Shader 得到每个片元的颜色之后，需要进行逐个片元的筛选测试。
- 裁剪测试。检测每一个片元是否落在裁剪框架的范围内，这一步丢弃了处在裁剪框之外的片元，不予显示。
- 模板测试。可以丢弃窗口中的任意区域，并对每个片元计数（暂时没有遇到）。
- **深度测试**。为了解决视觉的遮挡问题，利用每个片元的 $z$ 坐标，判定同一个屏幕位置的不同片元的深度，选择显示最上层的。代码上表示为 `glEnable(GL_DEPTH_TEST);`。
### $\it 1.7$ 混合
指的是，将新片元的颜色与已有的帧缓冲中的颜色进行混合，实现半透明效果。
## $\mathrm{II}$ GLSL Coding
GLSL 是一种专门用于 GPU 处理的类 C++ 语言，其主要用于 Vertex Shader 和 Fragment Shader 的自定义程序，由 C++ 主程序通过 `const char*` 来传递给 GPU。
### $\it 2.1$ Vertex Shader 部分
```glsl
#version 330 core
layout(location = 0) in vec3 aPos;
layout(location = 1) in vec2 aUV;

uniform mat4 uMVP;

out vec2 vUV;

void main() {
    gl_Position = uMVP * vec4(aPos, 1.0);
    vUV = aUV;
}
```
GLSL 程序分为输入、输出、计算三个部分。

输入部分。
- 逐顶点输入。其格式形如 `layout(location = x) in vec3 X;`：其中 `location = x` 表示顶点属性的接口编号（顶点属性就是一个顶点的 `pos, normal, uv` 等），该编号不直接对应 VBO 中的内存偏移量，这些由 C++ 端的 OpenGL 在 `main.cpp` 中规定好接口，Vertex Shader 程序直接取该接口对应的位置。
- 通用输入。`uniform mat4 uMVP` 是在一次 Draw Call 共享的数据（比如这里即全局 MVP 矩阵）。

输出部分。
- 该程序内置了通用输出，即默认会输出 `gl_Position`，该项固定含义为顶点坐标处理得到的**裁剪坐标系**的坐标。例如此处 `gl_Position = uMVP * vec4(aPos, 1.0);` 就是把 `aPos` 的输入扩展为齐次的四元坐标，然后左乘 MVP 矩阵直接作为 `gl_Position` 的输出。
- 自定义输出。格式为 `out vec2 vUV`，这里即为纹理坐标（二元）`(u, v)`，没有经过计算，直接传递。

可以等效地按照 C++ 代码理解其含义：
```cpp
VertexOutput vertexShader(VertexInput vertex, mat4 sharedMVP) {
    VertexOutput result;
    result.clipPosition = sharedMVP * vec4(vertex.position, 1.0);
    result.uv = vertex.uv;
    return result;
}
```
### $\it 2.2$ Fragment Shader
与 Vertex Shader 类似，Fragment Shader 也分为输入、计算、输出三个方面。
```glsl
#version 330 core

in vec2 vUV;

uniform sampler2D uTex;

out vec4 outColor;

void main() {
    outColor = texture(uTex, vUV);
}
```
输入部分。
- `in vec2 vUV` 从数据流中输入纹理坐标 `vUV`。
	- 注意这里的 `vUV` 已经不等同于 Vertex Shader 的输出了，已经经过了中间步骤的插值，是每一个片元的纹理坐标，而非顶点的纹理坐标（毕竟 Fragment Shader 是针对片元执行运算的）。
- 通用输入：`uniform sampler2D uTex` 这是用来实现纹理坐标与 C++ 端纹理图片的接口的，通过 `uTex` 可以通过 `vUV` 的纹理坐标采取相应坐标的 RGB 是什么。

输出部分。
- `out vec4 outColor` 即输出 $(R, G, B, A)$ 四元组。$A$ 表示该片元的透明度，用于混合时候的参数。

整体而言相当于：
```cpp
Color fragmentShader(FragmentInput fragment, Texture texture) {
    return sample(texture, fragment.uv);
}
```
## $\mathrm{III}$ 现代渲染组织
### $\it 3.1$ FrameBuffer 帧缓冲与 FBO
从顶点装配到深度测试、混合的这一整套流程（渲染管线）称为**一次绘制**（由一次 Draw Call 触发）。在一帧的渲染中，可能存在多次 Draw Call，期间所产生的数据就存储在 FrameBuffer 里，包含目前所有像素的颜色、深度等信息。后续的 Draw Call 可以参考前面的 Draw Call 已经计算好的数据，反复往 FrameBuffer 中“作画”。

**FBO**。在基础图形学程序中，会使用默认的 FrameBuffer 存储作画结果，我们也可以自定义属于自己的“作画缓存”，也就是自己申请一片储存作画结果的区域，我们使用 Frame Buffer Object（FBO）来管理这些内容的分配。
- 也就是：FBO 是用来管理额外帧缓冲存储信息的一套配置，可以向 FBO 添加“另外的一张画纸”（申请一张颜色纹理）等，其一个作用是存储作画的中间信息。
### $\it 3.2$ MultiPass 多遍绘制
在一帧的绘制中，通常可能跑多次完整管线，而这其中的中间变量可以存在自定义的 FBO 关联的纹理中（就是 `int temp;`）。多 Pass 流程可以通过 FBO 来切换，每次的作画保存在哪里。
- 例如，可以第一次画一张清晰的手的图片，存到自定义的 FBO（所关联的颜色纹理）中；然后第二次绘制取出其结果，进行高斯模糊，然后写入默认 FrameBuffer 中，输出的结果就是一张模糊的手图片。
- 例如，如果存在光源，需要绘制阴影，那么就需要多遍 Pass 来绘制，尤其第二次绘制需要参考第一次绘制的结果，辅助判断阴影。
### $\it 3.3$ Forward / Deferred Rendering
前向 / 延迟渲染的区别是：计算光照影响的颜色发生在绘制物体时直接完成，还是先保存表面信息，然后进一步计算得到。

具体来说，每个像素颜色的计算涉及表面位置、法向、材质、光源信息等，而前向渲染则直接在绘制物体时输出最终颜色，也就是跑完一遍管线就直接向 FrameBuffer 输出颜色信息。
- 例如直接在 Fragment Shader 中计算出每个片元的颜色，然后经过深度测试、混合等立即写入 FrameBuffer。

而延迟渲染则是先计算片元的底色（材质原本的颜色）、法向、深度等信息，将其写入不同的纹理（二维数组）中。这些统称 G-Buffer，关联到同一个 FBO。而后面一次 Pass 才处理光源信息，计算每个位置的最终颜色。
- 延迟渲染的好处是，可以集中处理表面可见处的光照（如果前向渲染中，直接输出光照信息但最终被遮挡了，那么该计算就浪费了），节省计算资源。但其需要开多个纹理储存中间信息，占用的缓存较多，以及其处理透明效果（需要前后多个片元贡献）不如前向渲染。
### $\it 3.4$ Compute Shader
与 Vertex Shader 与 Fragment Shader 不同，Compute Shader 专门用于对 CPU 指定的大量重复计算任务进行处理，结果直接写到显存的缓冲区。


---

- **已经建立的核心**：基础渲染管线、坐标空间、覆盖与插值、两个基本 shader。
- **保留基本认识即可**：Framebuffer / FBO、多 Pass、Forward / Deferred、Compute Shader。
- **暂缓**：细分的具体阶段、Mesh Shader、GPU-Driven、Bindless，以及整套硬件演化叙事。先标记“待回看”，不必继续展开。
