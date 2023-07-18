<script src="https://cdn.jsdelivr.net/npm/katex@0.13.11/dist/katex.min.js" integrity="sha384-YJ3Ooi3T6B0r0icrOP0k scrapbooking mymn lllllll" crossorigin="anonymous"></script>
<link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/katex@0.13.11/dist/katex.min.css" integrity="sha384-Um5gpz1odJg5Z4HAmzPtgZKdSm1XVgenQsK4ZODlTGWlZqHwqID7yyV7KCF0f0V" crossorigin="anonymous">

# 前言
这个项目是我和两位同学在*大三*的工程光学课程结课大作业，时隔多日，总算想起来有`Github`这个存放代码的绝佳仓库，遂上传到此。

# 背景介绍
通俗上讲，光的衍射可定义为光能绕开物体，在边缘处发生不沿直线传播的现象，实际上这一定义反映了衍射的最直观印象，即衍射发生在光的传播路径之外，1690年惠更斯提出假设“波面上的每一点都可以看作为一个发出球面子波的次级扰动中心，在后一个时刻这些子波的包络面就是该时刻的新的波面”，初步建立起点光源的衍射理论基础。菲涅耳用干涉原理完善了该定义，即“惠更斯子波来自于同一光源，它们是相干光束，波面外任意一点的光振动为该波面上全部子波在该处振动的相干叠加”。后人（基尔霍夫）把这两位的论述建模为严格的计算公式，即：
$$
\bm{E}(P)=\frac{A}{i\lambda}\iint_\Sigma\frac{\exp(ikl)}{l}\frac{\exp(ikr)}{r}\Big[\frac{\cos<\vec{n},\vec{r}>-\cos<\vec{n},\vec{l}>}{2}\Big]d\Sigma
$$
式中，$k$是波数，即$k=\frac{2\pi}{\lambda}$，$\Sigma$是平面衍射孔，$l$是光源点到某点P的距离，$r$是光源点到衍射孔某一点Q的距离，$<,>$表示两个矢量的夹角，矢量从光源出发，到达对应的目标点。
## 夫琅禾费衍射
直接使用基尔霍夫公式计算涉及到复杂的矢量运算，通常只有理论分析意义，为了便于工程应用，人们引入了两种简化模型，一种是菲涅耳衍射模型，即光源点的近场衍射。另一种应用更为广泛，即在距离光源点较远的情况下，光源发出的波束认为是平行光，若孔径的线度与观察者到孔径的距离相比足够小，则此时所得衍射结果称为夫琅禾费衍射，也称远场衍射模型。
由于观察者距离孔径和光源都足够遥远，光源发光、衍射光都可以认为是平行光，远场衍射模型的原理式可由下式给出：
$$
\widetilde{\bm E}(x,y)=\frac{\exp(ikz_1)}{z_1}\exp\Big[\frac{ik}{2z_1}(x^2+y^2)\Big]\iint_{-\infty}^{+\infty}\widetilde{\bm E}(x_1,y_1)\exp\Big[-i2\pi\big(x_1\frac{x}{\lambda z_1}+y_1\frac{y}{\lambda z_1}\big)\Big]\mathrm{d}x_1\mathrm{d}y_1
$$
式中,脚标为1的坐标指的是以观察者平面为原点建立坐标系，从观察者向衍射屏观测得到的坐标值。
## 从衍射到傅立叶光学
取定$\displaystyle u=\frac{x_1}{\lambda z_1}$，$\displaystyle v=\frac{y_1}{\lambda z_1}$，上式改写为：
$$
\widetilde{\bm E}(x,y)=\frac{\exp(ikz_1)}{z_1}\exp\Big[\frac{ik}{2z_1}(x^2+y^2)\Big]\iint_{-\infty}^{+\infty}\widetilde{\bm E}(x_1,y_1)\exp\Big[-i2\pi\big(x_1u+y_1v\big)\Big]\mathrm{d}x_1\mathrm{d}y_1
$$
其中，积分符号以外的指数项均带有单位虚数$i$，说明该项只影响光振动分布的相位，对计算光强度分布没有影响，从光振动计算光强度分布的公式为：
$$
I(x,y)=\widetilde{\bm E}(x,y)\cdot\widetilde{\bm E}^*(x,y)=\int_{0}^{T}\bm{E}(x,y)\cdot\bm{E}^*(x,y){\rm d}t
$$
可见是在时间上进行积分，因此该相位项在积分过程中会略去，孔径面上的复振幅分布为$\widetilde{\bm E}(x_1,y_1)$，因此上式简写为：
$$
\widetilde{\bm E}(x,y)=C_{xy}\iint_{-\infty}^{+\infty}\widetilde{\bm E}(x_1,y_1)\exp\Big[-i2\pi\big(x_1u+y_1v\big)\Big]\mathrm{d}x_1\mathrm{d}y_1
$$
此式也就是二维傅立叶变换的形式，说明夫琅禾费衍射就是对衍射孔处光束复振幅的二维傅立叶变换结果。这为我们的项目提供了理论支持。
# 代码架构
由于matlab闭源，代码文件不是脚本，我这里把代码复制出来，放置在[这里](https://github.com/BugBubbles/2D_FraunhoferDiffraction/blob/main/version_4_5_source_code)供大家参考。
## 