---
publish: true
---

# CG-2 从世界坐标到屏幕
## $\mathrm I$ 齐次坐标与基本变换
用 $(x, y, z, t)^T$ 表示三维坐标系中的 $\left( \dfrac xt, \dfrac yt, \dfrac zt \right)^T$。其好处是可以将所有变换（尤其是平移）都写成 $4\times4$ 的矩阵乘法。
### $\it 1.1$ 基本矩阵
举几个典型例子：平移矩阵、缩放矩阵、按 $z$ 轴旋转：
$$
\begin{aligned} \begin{pmatrix}
1&0&0&t_x\\
0&1&0&t_y\\
0&0&1&t_z\\
0&0&0&1
\end{pmatrix} \ \begin{pmatrix}
s_x&0&0&0\\
0&s_y&0&0\\
0&0&s_z&0\\
0&0&0&1
\end{pmatrix} \ \begin{pmatrix}
1&0&0&0\\
0&\cos\theta&-\sin\theta&0\\
0&\sin\theta&\cos\theta&0\\
0&0&0&1
\end{pmatrix} \end{aligned}
$$
这些都可以表示为线性变换，可以视作基本的变换。复合变换即为这些矩阵按左乘顺序乘在一起的结果。

在 `glm` 中，定义了 `glm::rotate`、`glm::scale` 等函数封装：
```cpp
glm::fmat4 model = glm::rotate(glm::fmat4(1.0f), angle, glm::fvec3(0,1,0));
model = glm::scale(model, glm::fvec3(scale));
```
法向量的变换矩阵：若对 $\mathbf x$ 施加线性变换 $A$，假设曲线此处的法向量为 $\mathbf n$，那么 $A\mathbf n$ 一般不与 $A\mathbf x$ 垂直，正确的做法是对法向量施加变换 $(A^{-1})^T$。这样 $((A^{-1})^T\mathbf n)^T(A\mathbf x)=\mathbf n^T\mathbf x=0$。
### $\it 1.2$ 相机旋转
设 $\mathbf v_i, \mathbf v_j, \mathbf v_k$ 为相机坐标系（在自然坐标系下）的基向量（单位向量），$\mathbf e_i, \mathbf e_j, \mathbf e_k$ 是自然坐标系的基向量，那么矩阵 $A=\begin{pmatrix} \mathbf v_i & \mathbf v_j & \mathbf v_k \end{pmatrix}$ 即为相机坐标系到自然坐标系的变换（左乘），且 $A$ 是正交矩阵。其逆（即转置）$A^T$ 表示自然坐标系到相机坐标系的变换。
- 假设相机的位置在自然坐标下是 $\mathbf u$，物品在自然坐标系的坐标是 $\mathbf x$，那么这个物品在相机坐标系的表示应当是 $\mathbf x'=A^T(\mathbf x-\mathbf u)$。

相机在自然世界可以抽象为一个位置向量和朝向参数。其中位置向量即一个三维向量 $\mathbf u$ 表示相机在自然坐标系下的位置。而旋转就有欧拉表示和四元数表示。
- 相机的朝向可以表示为一个朝向向量 $\mathbf t$ 和表示“头顶方向”的向量 $\mathbf h$，这两个向量垂直，因此总共有三个自由度。
- 欧拉表示的思路是，先用三维极坐标 $\varphi, \theta$ 表示 $\mathbf t$，然后引入一个角度 $\psi$ 表示 $\mathbf h$，即“歪头”的倾角。这样的问题是，当 $\varphi=\dfrac\pi2$ 时（即抬头看天），$\theta$ 失去了作用，此时根据工程实现，代表 $\theta$ 变化的向量仍然是水平，即 yaw 轴与 roll 轴重合，左右转头与歪头一样，会导致一系列渲染 bug（万向节锁，Gimbal Lock）。

术语：
- VRP：View Reference Point，即相机在自然系下的位置。
- VPN：View Plane Normal，即视线所指的方向。
- VUP：View Up Vector，即头顶正方向。
### $\it 1.3$ 四元数表示旋转
四元数是一个四元组，用来描述一个相机的朝向（更准确地说，是描述其相对标准姿态的旋转）。其核心的思想是，在三维世界中，任何相机的朝向，等价于从“标准方向”绕某一个轴 $\mathbf n$ 旋转一个角度 $\theta$（总共四个参数）。其中标准方向即相机面向 $-z$ 方向、水平直视前方的姿态（相机的“头顶”是 $+y$）。那么针对这样的一组旋转，四元数存储为 $q=\left( \cos\dfrac\theta2, n_x\sin\dfrac\theta2, n_y\sin\dfrac\theta2, n_z\sin\dfrac\theta2 \right)$。
- 为什么三维坐标下可以这样化简：考虑相机可以描述为两个垂直的向量 $\mathbf t$ 和 $\mathbf h$，其中 $\mathbf t$ 是相机朝向的方向，$\mathbf h$ 与 $\mathbf t$ 垂直，是相机“头顶”的方向。那么一个相机的任意一个姿态，其都可以等效为一次 $(\mathbf t_0, \mathbf h_0)$ 到 $(\mathbf t, \mathbf h)$ 的旋转，根据几何知识（欧拉旋转定理）一定存在这样的轴 $\mathbf n$ 和 $\theta$ 来等效描述这次旋转。
- 有关为什么四元数可以描述旋转：四元数本质上是一种复数的扩展（三个不同的虚根 ${\text i}, \text j, \text k$）。鉴于复数 $a+b{\text i}$ 可以等效于 $2\times2$ 矩阵 $\begin{pmatrix} a & -b \\ b & a \end{pmatrix}$，四元数来描述旋转以及按照上述代码变为旋转矩阵也并不奇怪。
### $\it 1.4$ 透视投影、裁剪坐标系
透视投影可以描述为 `(FoV, ratio, near, far)` 的四个参数。假定我们已经转变到相机参考系，此时考虑真实相机的摄影，我们截取视角（偏角）为 `FoV`、水平截面的长方形长宽比为 `ratio`、最近距离为 `near`、最远距离为 `far` 的一个**四棱台**，并将其中每一个点（在透视除法之前**可逆**）映射到 $[-1, 1]^3$ 中。其中，$x, y$ 坐标的映射规则是其射向相机的光线与“距离相机距离为 $1$”的平面的交点的坐标（并经过 FoV 区域线性归一化到 $[-1, 1]$），而 $z$ 坐标被重新定义为一个 $[-1, 1]$ 的深度（非线性映射），用来表示点距离的远近，用于深度测试（注：距离相机更近的点被分配到的 $z$ 值更有区分度，是为了更清晰辨别近处物品的“深度”）。

其矩阵形如：
$$
P=\begin{pmatrix}
\frac{1}{\text{aspect}\cdot\tan(\text{fovy}/2)}&0&0&0\\
0&\frac{1}{\tan(\text{fovy}/2)}&0&0\\
0&0&-\frac{far+near}{far-near}&-\frac{2\cdot far\cdot near}{far-near}\\
0&0&-1&0
\end{pmatrix}
$$

**平行投影**。矩阵形如
$$
M=\begin{pmatrix} 1&0&0&0\\0&1&0&0\\0&0&0&0\\0&0&0&1 \end{pmatrix}
$$
### $\it 1.5$ 模型坐标系
在自然世界引入某些几何模型的时候，其预先可能处于“模型坐标系”（也就是自己的坐标系）中。此种情况我们只需要设置一个能够在模型坐标系和自然坐标系中转换的矩阵即可。

在“骨骼”模型中，不同指关节的依赖形成了树状结构，而每一个关节的变换起初作用于自己的局部坐标系中。因此子节点（的自然坐标）是父节点的自然坐标乘上其父到子的变换。
## $\mathrm{II}$ 图形渲染管线与 GLSL
### $\it 2.1$ 管线流程
1. CPU 准备顶点数据，对每一个点准备 MVP 矩阵，绑定 VAO/VBO，调用 `glDrawElements`。
2. GPU 顶点着色器：根据 `vertex shader` 的 GLSL 代码计算 `gl_Position = u_mvp * transform * vec4(in_position, 1.0)`。
3. 图元装配、光栅化、片段着色器（`fragment shader`，处理每个像素的颜色）、深度测试。
4. 帧缓冲、投射到屏幕。
### $\it 2.2$ Vertex Shader 顶点着色器
这是一个 GLSL 小程序（给 GPU 处理），其作用是计算每一个点在裁剪空间中的坐标。其中从自然坐标系到屏幕，所有顶点共享同一个 MVP 矩阵（变换、投影，即 `u_mvp = projection * view * model`），而内容涉及复杂的动画时，其中将存储更多的用于播放动画的矩阵，具体的数据量取决于动画有多复杂（有多少个独立参考系）。根据动画播放的逻辑，每部分的点乘上其所需要的矩阵。
### $\it 2.3$ Fragment Shader 片段着色器与插值
Vertex Shader 对每个顶点（关键点）执行一次变换，而之后 GPU 将对顶点所得的图形进行光栅化，将顶点描述的几何图形变为片元（候选像素），并给出每个片元在当前三角形中的重心坐标以及一系列几何参数。Fragment Shader 则拿到这些数据（纹理、光源、材质），对每一个片元执行计算，按照你自己编写的 GLSL 代码决定其为什么颜色。

最后经过深度测试（处理遮挡关系）与混合（半透明处理）写入帧缓冲，由 GPU 逐帧处理到屏幕。
