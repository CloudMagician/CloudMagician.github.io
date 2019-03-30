---
layout:	post
title:	"Computer Graphics"
image:	''
date:	2019-01-06 08:30:49
tags:	
- Exam
description: 'My life for pass!'
categories:
---

<script type="text/javascript" src="../MathJax/MathJax.js?config=default"></script>

## 一、简介

* 计算机图形学：指用计算机产生对象图形的输出的技术。
* 模型：能够正确地表达出一个对象性质、结构和行为的描述信息。
* 图象处理是指用计算机来改善图象质量的数字技术。
* 模式识别是指用计算机对输入图形进行识别的技术。
* 计算几何学是研究几何模型和数据处理的学科。

## 二、图形基元的显示

### 区域填充

* 内定义区域，定义方法是指出区域内部所具有的象素值,此时区域内部所有象素有某个原值oldvalue。
* 边界定义区域，定义方法是指出区域边界所具有的象素值。此时区域边界上所有象素具有某个边界。

#### 种子填充算法

利用区域的连通性进行区域填充，除了需要区域应该明确定义外，还需要事先给定一个区域内部象素，这个象素称为种子。做区域填充时，要进行对光栅网格的遍历，找出由种子出发能达到而又不穿过边界的所有象素。这种利用连通性的填充，其主要优点是不受区域不规则性的影响，主要缺点是需要事先知道一个内部象素。

#### 多边形的扫描转换算法

1. 找出扫描线与多边形边界线的所有交点；
2. 按x坐标增加顺序对交点排序；
3. 在交点对之间进行填充。

## 三、图形变换

### 1. 齐次坐标

齐次坐标表示法就是用n+1维向量表示一个n维向量。

### 2. 二维图形变换

|                             平移                             |                             比例                             |                             旋转                             |                             对称                             |                             错切                             |
| :----------------------------------------------------------: | :----------------------------------------------------------: | :----------------------------------------------------------: | :----------------------------------------------------------: | :----------------------------------------------------------: |
| $$\left[\begin{matrix}1&0&0\\0&1&0\\T_x&T_y&1\end{matrix}\right]$$ | $$\left[\begin{matrix}S_x&0&0\\0&S_y&0\\0&0&1\end{matrix}\right]$$ | $$\left[\begin{matrix}\cos\theta&\sin\theta&0\\-\sin\theta&\cos\theta&0\\0&0&1\end{matrix}\right]$$ | $$\left[\begin{matrix}a&d&0\\b&e&0\\0&0&1\end{matrix}\right]$$ | $$\left[\begin{matrix}1&d&0\\b&1&0\\0&0&1\end{matrix}\right]$$ |

### 3. 二维视见变换

* 窗口就是在用户坐标系中指出的那个要显示出来的区域，这一区域通常为矩形区域。
* 视见区是屏幕域中的一个子区域，通常为矩形区域，它最大与屏幕域等同。视见区用于显示窗口中的图形。
* 窗口与视见区的差别在于，窗口是在用户坐标系中确定的，它指出了要显示的图形；而视见区在设备坐标系中确定，它指出了实际显示的图形处于显示屏幕的哪一部分。
* 视见变换就是将用户坐标系窗口内的图形变换到显示屏幕设备坐标系的视见区中以产生显示。 

### 4. 三维图形变换

### 5. 投影

#### 平行投影

* 当投影中心与投影平面的距离为无穷远时，投射直线成为一组平行线。

* 斜交投影投影矩阵：$$\left[\begin{matrix}1&0&0&0\\0&1&0&0\\l\cos\alpha&l\sin\alpha&0&0\\0&0&0&1\end{matrix}\right]$$

#### 透视投影

* 当投影中心与投影平面的距离是有限数值时，投射直线交于一点。
* 可以分为一点透视、二点透视、三点透视。

### 6. 裁剪

#### **Cohen-Sutherland**算法

* 算法的基本思想是：首先判断直线段是否全部在窗口内，是，则保留；不是，则再判断是否完全在窗口之外，如是，则舍弃。如果这两种情况都不属于，则将此直线段分割，对分割后的子线段再进行如前判断。直至所有直线段和由直线段分割出来的子线段都已经确定了是保留还是舍弃为止。
* 区域编码：
  1. 如果该区域在窗口的上方，则代码的第一位为1，否则为0；
  2. 如果该区域在窗口的下方，则代码的第二位为1，否则为0；
  3. 如果该区域在窗口的右侧，则代码的第三位为1，否则为0；
  4. 如果该区域在窗口的左侧，则代码的第四位为1，否则为0。

```c
double xl, xr, yt, yb;//窗口位置

int makecode(double x, double y) {
    int c = 0;
    
    if (x < xl) {
        c = 1;
    } else if (x>xr) {
        c = 2;
    }
    if (y < yb) {
        c = c + 4;
    } else if (y > yt) {
        c = c + 8;
    }
    
    return c;
}

void Cohen_Sutherland(double x0, double y0, double x2, double y2) {
    int c = 0;
    int c1 = makecode(x0, y0);
    int c2 = makecode(x2, y2);
    double x = 0;
    double y = 0;

    while (c1 != 0 || c2 != 0) {
        if (c1 & c2) return;    //全在外面则返回
        
        c = c1;
        if (c == 0) c = c2;
        
        if (c & 1) {
            y=y0+(y2-y0)*(xl-x0)/(x2-x0);
            x=xl;
        } else if (c & 2) {
            y=y0+(y2-y0)*(xr-x0)/(x2-x0);
            x=xr;
        } else if (c & 4) {
            x=x0+(x2-x0)*(yb-y0)/(y2-y0);
            y=yb;
        } else if (c & 8) {
            x=x0+(x2-x0)*(yt-y0)/(y2-y0);
            y=yt;
        }
        
        if (c == c1) {
            x0=x;
            y0=y;
            c1 = makecode(x, y);
        } else {
            x2=x;
            y2=y;
            c2 = makecode(x, y);
        }
    }
    
    showline(x0, y0, x2, y2);   //显示可见线段
}
```

## 四、曲线和曲面

### 曲线和曲面表示的基础知识

- 插值：要求构造一条曲线顺序通过型值点，称为对这些型值点进行插值（interpolation）。
- 逼近：构造一条曲线，使它在某种意义上最佳逼近这些型值点，称之为对这些型值点进行逼近（approximation）。

### Hermite多项式

#### 分段三次Hermite差值

$$
P(u)=\left(\begin{matrix}u^3&u^2&u&1\end{matrix}\right)\left[\begin{matrix}2&-2&1&1\\-3&3&-2&-1\\0&0&1&0\\1&0&0&0\end{matrix}\right]\left[\begin{matrix}P_i\\P_{i+1}\\P'_i\\P'_{i+1}\end{matrix}\right]
$$

### Bezier曲线

$$
P(u)=\frac{1}{6}\left(\begin{matrix}t^3&t^2&t&1\end{matrix}\right)\left[\begin{matrix}-1&3&-3&1\\3&-6&3&0\\-3&3&0&0\\1&0&0&0\end{matrix}\right]\left[\begin{matrix}P_i\\P_{i+1}\\P_{i+2}\\P_{i+3}\end{matrix}\right]
$$



#### Bezier曲线的拼接

* $$C^1$$连续：$$Q_1-P_0=a(P_3-P_2)$$
* $$C^2$$连续：$$Q_2-P_1=2(Q_1-P_2)$$

#### Bezier曲线的绘制

* 分裂法

### B样条曲线

* 给定n+1个控制点$$P_0，P_1，…，P_n$$，它们所确定的*k*阶B样条曲线是：
  $$
  P(u)=\sum_{i=0}^{n}N_{i,k}(u)\times P_i,u\in[u_{k-1},u_{n+1})
  $$


#### 均匀B样条曲线(4阶3次)

$$
P(u)=\frac{1}{6}\left(\begin{matrix}u^3&u^2&u&1\end{matrix}\right)\left[\begin{matrix}-1&3&-3&1\\3&-6&3&0\\-3&0&3&0\\1&4&1&0\end{matrix}\right]\left[\begin{matrix}P_i\\P_{i+1}\\P_{i+2}\\P_{i+3}\end{matrix}\right]
$$

#### 均匀B样条曲线的拼接问题

$$
Q_i(0)=\frac{1}{6}(P_i+4P_{i+1}+P_{i+2})\\
Q'_i(0)=\frac{1}{2}(P_{i+2}-P_i)\\
Q''_i(0)=P_i-2P_{i+1}+P_{i+2}
$$

## 五、图形运算

### 1. 线段的交点计算

### 2. 多边形表面的交线计算

### 3. 平面中的凸壳算法

* 凸壳：包含一个平面点集的最小凸区域。凸区域要求区域内任意两点的连线仍在该区域内。

#### Graham扫描算法

* 处理的思路是设想有一内点**O**并且不妨设想**O**就是坐标原点**,**这时点集**S**中所有各点相对轴**OX**有一个倾角。所有各点按倾角递增排序后**,**如果某一点不是壳上顶点**,**则它必然在两个壳顶点与点**O**形成的三角形内部。

* **Graham**扫描的实质是围绕已经按**"**倾角**"**排序的各顶点进行一次扫描**,**在扫描过程中消去在凸壳内部的点**,**留下以希望次序排列的壳顶点。

* 由于是按倾角递增排序,可知若连续三个顶点P1,P2,P3是一个“右转”，则P2是一个应去掉的内点。

* 对给出的三点P1,P2, P3 ,设它们的坐标是(x1,y1),(x2,y2),(x3,y3),这时要判断三点在P2处形成一个右转还是左转,可以计算下面的行列式：$$\Delta=\left|\begin{matrix}x_1&y_1&1\\x_2&y_2&1\\x_3&y_3&1\end{matrix}\right|$$

  其中△给出的是带有正负号的三角形P1P2P3面积的2倍,因此若△>0,则P1,P2,P3是左转; △ <0,则是右转; △ =0,则三点共线。

```c
void Graham(POINT S[], int n) {	//S为点集数组，n为点的个数
    sortangle(S,n,Q);   //依据边界点O将S中点按倾角排成序放置在Q中,Q中起始点为O
    v = first(Q);       //取出Q中起始点
    while (next(v,Q) != first(Q)) {
        if (left(v,next(v,Q),next(next(v,Q),Q)) {	//若连续三点左转
            v=next(v,Q);	//v前进至下一个点
        } else {
            delete(next(v),Q);  //删除v的下一个点
            v=pred(v,Q);        //v退回至前一个点
        }
    }
}
```

### 4. 包含与重叠

### 5. 简单多边形的三角剖分

## 六、形体的表示及其数据结构

### 1. 二维形体的表示

#### 二维图形的边界表示

* 折线法：就是用多段线段形成的折线去逼近曲线。
* 带树法：带树是一棵二叉树，树的每个结点对应一个矩形带段，这样每个结点可由八个字段组成，前六个字段描述矩形带段，后二个是指向两个子结点的指针，即矩形带段的起点是(xb,yb)，终点是(xe,ye)。相对从起点到终点的连线，矩形有两边与之平行，两边与之垂直，平行两边与之距离分别为wl和wr。

### 2. 三维几何模型

### 3. 分形

## 七、消除隐藏线和隐藏面

* 消除隐藏面算法：图象空间算法、客体空间算法。
* 图象空间算法对显示设备上每一个可分辨象素进行判断，看组成物体的多个多边形表面中哪一个在该象素上可见，即要对每一象素检查所有的表面。
* 客体空间算法把注意力集中在分析要显示形体各部分之间的关系上，这种算法对每一个组成形体的表面，都要与其它各表面进行比较，以便消去不可见的面或面的不可见部分。

### 1. 线面比较法消除隐藏线

* 多面体的面可见性：凸多面体的可见面就是朝向观察位置的面。设观察方向由指向观察位置的一个方向向量k给出，所考查的面的外法向量是n，则这两个向量的夹角 α 满足0≤ α < π/2时，所考查面是可见的，否则就是不可见的。
* 利用外法线就可以判断凸多面体上各表面的可见性，由此就能解决对单个凸多面体的隐藏线和隐藏面的消除问题。
* 消除隐藏线的线面比较法的最先一步就是利用外法线判断出所有可能的可见面，可能可见面上的线段是可能可见线。要依次用每一条可能可见线，与每一个可能可见面比较，从而确定出可见线、隐藏线及可见线上的隐藏部分。

### 2. 深度排序算法

* 主要步骤：
  1. 把所有的多边形按顶点最大**z**坐标值进行排序。
  2. 解决当多边形**z**范围发生交迭时出现不明确问题。
  3. 按最大**z**坐标值逐渐减小的次序，对每个多边形进行扫描转换。

### 3. z−缓冲算法

* z−缓冲算法(深度缓冲算法)是一种最简单的图象空间算法。对每一个点，这个算法不仅需要有一个更新缓冲器存储各点的象素值，而且还需要有一个z−缓冲存储器存储相应的z值。更新缓冲存储器初始化为背景值，z缓冲存储器初始化为可以表示的最大z值。对每一个多边形，不必进行深度排序算法要求的初始排序，立即就可以逐个进行扫描转换。
* 扫描转换时,对每个多边形内部的任意点(x,y),实施如下步骤：
  1. 计算在点(x,y)处多边形的深度值Z(x,y)。
  2. 如果计算所得的Z(x,y)值，小于在z−缓冲存储器中点(x,y)处记录的深度值，那么就做：
     1. 把值Z(x,y)送入z−缓冲存储器的点(x,y)处。
     2. 把多边形在深度Z(x,y)处应有的象素值，送入更新缓冲存储器的点(x,y)处。

### 4. 扫描线算法

* 扫描线算法是图象空间算法，它建立图象是通过每次处理一条扫描线来完成的。这个算法是第二章讨论的多边形填充的扫描线算法的推广。在多边形填充的扫描线算法中，只是对一个多边形做扫描转换，而这里是同时对多个多边形做扫描转换。
* 要建立一个边表ET。ET中各登记项按边的较小的y坐标递增排列；每一登记项下的“吊桶”，按所记x坐标递增排列。“吊桶”中各项的内容依次是：
  1. 与较小的y坐标对应的端点的x坐标xmin。
  2. 边的另一端点的较大的y坐标ymax。
  3. x的增量Δx，它实际上是边的斜率的倒数，是从一条扫描线走到下一条扫描线时，按x方向递增的步长。
  4. 边所属多边形的标记。

### 5. 区域分割算法

* 区域分割算法将投影平面分割成区域，考察区域内的图象。如果容易决定在这个区域内某些多边形是可见的，那么就可以显示那些可见的多边形，完成对这一区域的显示任务。否则，就将区域再分割成小的区域，对小的区域递归地进行判断。由于区域逐渐变小，在每个区域内的多边形逐渐变少，最终总可以判定哪些多边形是可见的。这个算法利用的区域的相关性，这种相关性是指位于适当大小的区域内的所有象素，表示的其实是同一个表面。

## 八、真实感图形的绘制

* 用计算机在图形设备上生成连续色调的真实感图形必须完成四个基本的任务。
  1. 用数学方法建立所构造三维场景的几何描述，并将它们输入至计算机。（立体或曲面造型系统）
  2. 将三维几何描述转换为二维透视图。（透视变换） 
  3. 确定场景中的所有可见面。（消除隐藏面算法）
  4. 计算场景中可见面的颜色。（基于光学物理的光照模型计算）

### 1. 漫反射及具体光源的照明

#### 环境光 ambient light

* 在多数实际环境中，存在由于许多物体表面多次反射而产生的均匀的照明光线，这就是环境光线。环境光线的存在使物体得到漫射照明。
* 亮度计算如下：$$I=I_a· k_a$$
* 其中I是可见表面的亮度，Ia是环境光线的总亮度， ka是物体表面对环境光线的反射系数，它在0到1之间。

#### 漫反射 diffuse reflection

* 具体光源在物体表面可以引起漫反射和镜面反射(specular reflection)。漫反射是指来自具体光源的能量到达表面上的某一点后，就均匀地向各个方向散射出去，使得观察者从不同角度观察时，这一点呈现的亮度是相同的。通常不光滑的粗糙表面总是呈现出漫反射的效果。

* Lambert定律指出，漫反射的效果与表面相对于光源的取向有关，即：
  $$
  I_d=I_p · k_d ·\cos{θ}
  $$
  其中Id是漫反射引起的可见表面上一点的亮度。Ip是点光源发出的入射光线引起的亮度。kd是漫反射系数，它的取值在0到1之间，随物体材料不同而不同。 θ是可见表面法向N和点光源方向L之间的夹角，即入射角，它应该在0°到90°之间。

* 为了简化公式中余弦值的实际计算，可以假定向量N和L都已经正规化，即已经是长度为1的单位向量，这样就可以使用向量的数量积或内积。因为这时$$\cos{\theta}=L\cdot N$$，于是得：$$I_d=I_p · k_d ·(L · N)$$

* 将环境光线和漫反射的效果结合起来，计算亮度的公式应该写成：
  $$
  I=I_a· k_a + I_p · k_d ·(L\cdot N)
  $$

* 通常认为具体光源对可见表面产生的照明作用，是随着光源与表面之间距离的增加而下降的。设**R**是光线从光源发出到达表面再返回的距离，则：
  $$
  I=I_a· k_a + I_p · k_d ·(L\cdot N) /R^2
  $$

* 对于平行投影，光源在无穷远处，故距离R成为无穷大。对于透视投影，1/R2也常常有很大的数值范围而使效果不好。一种比较逼真的效果，可通过用r+k代替R2来获得：
  $$
  I=I_a· k_a + I_p · k_d ·(L\cdot N) /(r+k)
  $$
  其中r是光源到表面的距离，k是根据经验选取的一个常数。

#### 镜面反射与**Phong**模型

$$
I=I_a· k_a + I_p /(r+k) · (k_d ·(L\cdot N)+k_s(R\cdot V)^n)
$$

#### 光的衰减

* 衰减比例为光的传输距离平方的倒数，若以衰减函数f(d)来表示衰减的比例，则$$f(d)=1/d^2$$其中，d为光的传播距离。

* 这种变化规律对点光源来说是正确的，但真实的世界中物体并不是以点光源照射的。为了弥补点光源的不足，产生真实感更强的图形，一个有效的衰减函数如下所示：$$f(d)=\min(1/(C_0+C_1d+C_2d^2),1)$$

* 光照明计算式：
  $$
  I=I_a· k_a +f(d)·I_p·(k_d ·(L\cdot N)+k_s(R\cdot V)^n)
  $$


### 2. 多边形网的明暗处理

多边形网方法是指用若干多边形表面去拟合任意形状复杂形体的方法。

#### 常数明暗法（**Flat**均匀着色法）

#### 亮度插值明暗法（**Gouraud**着色方法）

* 步骤：
  1. 计算各多边形表面的法向量。
  2. 计算各顶点的法向量。这里顶点的法向，指共享该顶点的所有多边形表面法向的平均值。
  3. 计算各顶点的亮度。因为各顶点的法向已经求得，所以已经可以利用上节讨论的计算亮度的公式进行计算。
  4. 计算各多边形表面上任意点处的亮度值，实行对多边形表面的明暗处理。做法是先利用顶点的亮度值，在边上做线性插值，求得边上的亮度值。再用之在扫描线上做线性插值，从而求得多边形面内任意点处的亮度值。

#### 法向量插值明暗法（**Phong**着色方法）