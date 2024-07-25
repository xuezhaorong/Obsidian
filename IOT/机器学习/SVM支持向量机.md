假设超平面线性方程：

![](https://www.yuque.com/api/services/graph/generate_redirect/latex?w%5ETx%2Bb%20%3D%200#card=math&code=w%5ETx%2Bb%20%3D%200&id=GJdx0)

其中![](https://www.yuque.com/api/services/graph/generate_redirect/latex?w%3D(w_1%2Cw_2%2C...%2Cw_d)#card=math&code=w%3D%28w_1%2Cw_2%2C...%2Cw_d%29&id=P8JAk)为**法向量**，决定超平面的方向，`b`为**位移项**，决定了超平面与原点之间的距离。

样本空间中任意点`x`到超平面`(w,b)`的距离可写为：

![](https://www.yuque.com/api/services/graph/generate_redirect/latex?r%20%3D%20%20%5Cfrac%7B%7Cw%5ET%2Bb%7C%7D%7B%7C%7Cw%7C%7C%7D#card=math&code=r%20%3D%20%20%5Cfrac%7B%7Cw%5ET%2Bb%7C%7D%7B%7C%7Cw%7C%7C%7D&id=DOvCl)

对于![](https://www.yuque.com/api/services/graph/generate_redirect/latex?(x_i%2Cy_i)%20%E2%88%88%20D#card=math&code=%28x_i%2Cy_i%29%20%E2%88%88%20D&id=D8ZWg),若![](https://www.yuque.com/api/services/graph/generate_redirect/latex?y_i%20%3Dn%20%20%2B1#card=math&code=y_i%20%3Dn%20%20%2B1&id=djZbt),则有![](https://www.yuque.com/api/services/graph/generate_redirect/latex?w%5ETx_i%2Bb%3E0#card=math&code=w%5ETx_i%2Bb%3E0&id=YHuCR),若![](https://www.yuque.com/api/services/graph/generate_redirect/latex?y_i%20%3D%20-1#card=math&code=y_i%20%3D%20-1&id=bMl3w),则有![](https://www.yuque.com/api/services/graph/generate_redirect/latex?w%5ETx_i%2Bb%3C0#card=math&code=w%5ETx_i%2Bb%3C0&id=ugYzv)，所以有![](https://www.yuque.com/api/services/graph/generate_redirect/latex?y_iy(x_i)%E2%89%A70#card=math&code=y_iy%28x_i%29%E2%89%A70&id=xIdOe)恒成立，则：

![](https://www.yuque.com/api/services/graph/generate_redirect/latex?r%3D%5Cfrac%7By_i%7Cw%5ETx%2Bb%7C%7D%7B%7C%7Cw%7C%7C%7D%20%3D%20%5Cfrac%7By_i(w%5ETx%2Bb)%7D%7B%7C%7Cw%7C%7C%7D#card=math&code=r%3D%5Cfrac%7By_i%7Cw%5ETx%2Bb%7C%7D%7B%7C%7Cw%7C%7C%7D%20%3D%20%5Cfrac%7By_i%28w%5ETx%2Bb%29%7D%7B%7C%7Cw%7C%7C%7D&id=K2Up3)

对于**支持向量(support vector)**,将其进行放缩，使其条件更加严格，为![](https://www.yuque.com/api/services/graph/generate_redirect/latex?y_iy(x_i)%E2%89%A71#card=math&code=y_iy%28x_i%29%E2%89%A71&id=eDvEv)恒成立，则两个异类支持向量到超平面的距离之和为：

![](https://www.yuque.com/api/services/graph/generate_redirect/latex?r%20%3D%20%5Cfrac%7B2%7D%7B%7C%7Cw%7C%7C%7D#card=math&code=r%20%3D%20%5Cfrac%7B2%7D%7B%7C%7Cw%7C%7C%7D&id=awarA)

所以欲找到具有**最大间隔(maximum margin)**的划分超平面，需要找到能满足上式约束的参数`w`和`b`使得`r`最大，即：

![](https://www.yuque.com/api/services/graph/generate_redirect/latex?%5Cunderset%7Bw%2Cb%7D%7Bmax%7D%5Cfrac%7B2%7D%7B%7C%7Cw%7C%7C%7D#card=math&code=%5Cunderset%7Bw%2Cb%7D%7Bmax%7D%5Cfrac%7B2%7D%7B%7C%7Cw%7C%7C%7D&id=qULoX)

![](https://www.yuque.com/api/services/graph/generate_redirect/latex?s.t%20y_i(w%5ETx_i%2Bb)%E2%89%A71%2Ci%20%3D%201%2C2%2C...%2Cm#card=math&code=s.t%20y_i%28w%5ETx_i%2Bb%29%E2%89%A71%2Ci%20%3D%201%2C2%2C...%2Cm&id=nlr9Y)

化为求最小值**(基本型)**：

![](https://www.yuque.com/api/services/graph/generate_redirect/latex?%5Cunderset%7Bw%2Cb%7D%7Bmin%7D%5Cfrac%7B1%7D%7B2%7D%7C%7Cw%7C%7C%5E2#card=math&code=%5Cunderset%7Bw%2Cb%7D%7Bmin%7D%5Cfrac%7B1%7D%7B2%7D%7C%7Cw%7C%7C%5E2&id=jffPL)

![](https://www.yuque.com/api/services/graph/generate_redirect/latex?s.t.%20y_i(w%5ETx_i%2Bb)%E2%89%A71%2Ci%20%3D%201%2C2%2C...%2Cm#card=math&code=s.t.%20y_i%28w%5ETx_i%2Bb%29%E2%89%A71%2Ci%20%3D%201%2C2%2C...%2Cm&id=Z6zLy)

加入核函数![](https://www.yuque.com/api/services/graph/generate_redirect/latex?%CF%86(x_i)#card=math&code=%CF%86%28x_i%29&id=SYdrM)和软间隔![](https://www.yuque.com/api/services/graph/generate_redirect/latex?C%5Csum_%7Bi%3D1%7D%5E%7Bn%7Dl_%7B0%2F1%7D(y_i(w%5ETx_i%2Bb)-1)#card=math&code=C%5Csum_%7Bi%3D1%7D%5E%7Bn%7Dl_%7B0%2F1%7D%28y_i%28w%5ETx_i%2Bb%29-1%29&id=XeOfq)后为：

![](https://www.yuque.com/api/services/graph/generate_redirect/latex?%5Cunderset%7Bw%2Cb%7D%7Bmin%7D%5Cfrac%7B1%7D%7B2%7D%7C%7Cw%7C%7C%5E2%2BC%5Csum_%7Bi%3D1%7D%5E%7Bm%7Dmax(0%2C1-y_i(w%5ET%CF%86(x_i)%2Bb))%20%3D%20%5Cunderset%7Bw%2Cb%2C%CE%BE%7D%7Bmin%7D%5Cfrac%7B1%7D%7B2%7D%7C%7Cw%7C%7C%5E2%2BC%5Csum_%7Bi%3D1%7D%5E%7Bm%7D%CE%BE_i#card=math&code=%5Cunderset%7Bw%2Cb%7D%7Bmin%7D%5Cfrac%7B1%7D%7B2%7D%7C%7Cw%7C%7C%5E2%2BC%5Csum_%7Bi%3D1%7D%5E%7Bm%7Dmax%280%2C1-y_i%28w%5ET%CF%86%28x_i%29%2Bb%29%29%20%3D%20%5Cunderset%7Bw%2Cb%2C%CE%BE%7D%7Bmin%7D%5Cfrac%7B1%7D%7B2%7D%7C%7Cw%7C%7C%5E2%2BC%5Csum_%7Bi%3D1%7D%5E%7Bm%7D%CE%BE_i&id=YjZFV)

![](https://www.yuque.com/api/services/graph/generate_redirect/latex?s.t.%20y_i(w%5ET%CF%86(x_i)%2Bb)%E2%89%A71%2Ci%20%3D%201%2C2%2C...%2Cm#card=math&code=s.t.%20y_i%28w%5ET%CF%86%28x_i%29%2Bb%29%E2%89%A71%2Ci%20%3D%201%2C2%2C...%2Cm&id=kAVVV)

![](https://www.yuque.com/api/services/graph/generate_redirect/latex?%CE%BE%E2%89%A70%2Ci%20%3D%201%2C2%2C...%2Cm#card=math&code=%CE%BE%E2%89%A70%2Ci%20%3D%201%2C2%2C...%2Cm&id=NN9OO)

其中**ξ**为**松弛变量**

通过**拉格朗日乘子法**可得到：

![](https://www.yuque.com/api/services/graph/generate_redirect/latex?L(w%2Cb%2C%CE%B1%2C%CE%BE%2C%CE%BC)%3D%5Cfrac%7B1%7D%7B2%7D%7C%7Cw%7C%7C%5E2%2BC%5Csum_%7Bi%3D1%7D%5E%7Bm%7D%CE%BE_i%2B%5Csum_%7Bi%3D1%7D%5E%7Bm%7D%CE%B1(1-%CE%BE-y_i(w%5ET%CF%86(x_i)%2Bb))-%5Csum_%7Bi%3D1%7D%5E%7Bm%7D%CE%BC_i%CE%BE_i#card=math&code=L%28w%2Cb%2C%CE%B1%2C%CE%BE%2C%CE%BC%29%3D%5Cfrac%7B1%7D%7B2%7D%7C%7Cw%7C%7C%5E2%2BC%5Csum_%7Bi%3D1%7D%5E%7Bm%7D%CE%BE_i%2B%5Csum_%7Bi%3D1%7D%5E%7Bm%7D%CE%B1%281-%CE%BE-y_i%28w%5ET%CF%86%28x_i%29%2Bb%29%29-%5Csum_%7Bi%3D1%7D%5E%7Bm%7D%CE%BC_i%CE%BE_i&id=YGpR8)

其中![](https://www.yuque.com/api/services/graph/generate_redirect/latex?%CE%B1_i%E2%89%A70%2C%CE%BC_i%E2%89%A70#card=math&code=%CE%B1_i%E2%89%A70%2C%CE%BC_i%E2%89%A70&id=Dt4NW)是拉格朗日乘子

分别对![](https://www.yuque.com/api/services/graph/generate_redirect/latex?w%2Cb%2C%CE%BE_i#card=math&code=w%2Cb%2C%CE%BE_i&id=mTkH0)求偏导为0可得：

![](https://www.yuque.com/api/services/graph/generate_redirect/latex?w%20%3D%20%5Csum_%7Bi%3D1%7D%5E%7Bm%7D%CE%B1_iy_ix_i#card=math&code=w%20%3D%20%5Csum_%7Bi%3D1%7D%5E%7Bm%7D%CE%B1_iy_ix_i&id=M30ml)

![](https://www.yuque.com/api/services/graph/generate_redirect/latex?0%3D%5Csum_%7Bi%3D1%7D%5E%7Bm%7D%CE%B1_iy_i#card=math&code=0%3D%5Csum_%7Bi%3D1%7D%5E%7Bm%7D%CE%B1_iy_i&id=fJUaR)

![](https://www.yuque.com/api/services/graph/generate_redirect/latex?C%3D%CE%B1_i%2B%CE%BC_i#card=math&code=C%3D%CE%B1_i%2B%CE%BC_i&id=EEe18)

代入上式中可得：

![](https://www.yuque.com/api/services/graph/generate_redirect/latex?%5Cunderset%7B%CE%B1%7D%7Bmax%7D%5Csum_%7Bi%3D1%7D%5E%7Bm%7D%CE%B1_i-%5Cfrac%7B1%7D%7B2%7D%5Csum_%7Bi%3D1%7D%5E%7Bm%7D%5Csum_%7Bj%3D1%7D%5E%7Bm%7D%CE%B1_i%CE%B1_jy_iy_jK_%7Bxi%2Cxj%7D#card=math&code=%5Cunderset%7B%CE%B1%7D%7Bmax%7D%5Csum_%7Bi%3D1%7D%5E%7Bm%7D%CE%B1_i-%5Cfrac%7B1%7D%7B2%7D%5Csum_%7Bi%3D1%7D%5E%7Bm%7D%5Csum_%7Bj%3D1%7D%5E%7Bm%7D%CE%B1_i%CE%B1_jy_iy_jK_%7Bxi%2Cxj%7D&id=qBux3)

![](https://www.yuque.com/api/services/graph/generate_redirect/latex?s.t.%5Csum_%7Bi%3D1%7D%5E%7Bm%7D%CE%B1_iy%20_i%3D0#card=math&code=s.t.%5Csum_%7Bi%3D1%7D%5E%7Bm%7D%CE%B1_iy%20_i%3D0&id=w31L1)

![](https://www.yuque.com/api/services/graph/generate_redirect/latex?0%E2%89%A6%CE%B1_i%E2%89%A6C%2Ci%3D1%2C2%2C...%2Cm#card=math&code=0%E2%89%A6%CE%B1_i%E2%89%A6C%2Ci%3D1%2C2%2C...%2Cm&id=VMI8Z)

忽略约束条件,使用**SMO算法**固定![](https://www.yuque.com/api/services/graph/generate_redirect/latex?%CE%B1_1#card=math&code=%CE%B1_1&id=VBm9w)和![](https://www.yuque.com/api/services/graph/generate_redirect/latex?%CE%B1_2#card=math&code=%CE%B1_2&id=fPA5b),剩下的 ![](https://www.yuque.com/api/services/graph/generate_redirect/latex?%F0%9D%9B%BC_3%2C%F0%9D%9B%BC_4%2C%E2%80%A6%2C%F0%9D%9B%BC_%F0%9D%91%81#card=math&code=%F0%9D%9B%BC_3%2C%F0%9D%9B%BC_4%2C%E2%80%A6%2C%F0%9D%9B%BC_%F0%9D%91%81&id=jvSdG) 则固定，作为常数处理，把与 ![](https://www.yuque.com/api/services/graph/generate_redirect/latex?%F0%9D%9B%BC_1#card=math&code=%F0%9D%9B%BC_1&id=e1akv),![](https://www.yuque.com/api/services/graph/generate_redirect/latex?%F0%9D%9B%BC_2#card=math&code=%F0%9D%9B%BC_2&id=SVXkC) 无关的项合并成常数项 𝐶,核函数![](https://www.yuque.com/api/services/graph/generate_redirect/latex?%CF%86(x_i)#card=math&code=%CF%86%28x_i%29&id=rTIpf)表示为![](https://www.yuque.com/api/services/graph/generate_redirect/latex?K_i#card=math&code=K_i&id=IctT8) :

![](https://www.yuque.com/api/services/graph/generate_redirect/latex?W(%CE%B1_1%2C%CE%B1_2)%20%3D%20%CE%B1_1%20%2B%20%CE%B1_2%20-%20%5Cfrac%7B1%7D%7B2%7DK_%7B1%2C1%7Dy_1%5E%7B2%7D%CE%B1_i%5E%7B2%7D-%5Cfrac%7B1%7D%7B2%7DK_%7B2%2C2%7Dy_2%5E%7B2%7D%CE%B1_2%5E%7B2%7D-K_%7B1%2C2%7Dy_1y_2%CE%B1_1%CE%B1_2-y_1%CE%B1_1%5Csum_%7Bi%3D3%7D%5E%7BN%7D%CE%B1_iy_iK_%7Bi%2C1%7D-y2%CE%B1_2%5Csum_%7Bi%3D3%7D%5E%7BN%7D%CE%B1_iy_iK_%7Bi%2C2%7D%2BC#card=math&code=W%28%CE%B1_1%2C%CE%B1_2%29%20%3D%20%CE%B1_1%20%2B%20%CE%B1_2%20-%20%5Cfrac%7B1%7D%7B2%7DK_%7B1%2C1%7Dy_1%5E%7B2%7D%CE%B1_i%5E%7B2%7D-%5Cfrac%7B1%7D%7B2%7DK_%7B2%2C2%7Dy_2%5E%7B2%7D%CE%B1_2%5E%7B2%7D-K_%7B1%2C2%7Dy_1y_2%CE%B1_1%CE%B1_2-y_1%CE%B1_1%5Csum_%7Bi%3D3%7D%5E%7BN%7D%CE%B1_iy_iK_%7Bi%2C1%7D-y2%CE%B1_2%5Csum_%7Bi%3D3%7D%5E%7BN%7D%CE%B1_iy_iK_%7Bi%2C2%7D%2BC&id=H3mho)

即二次规划优化：

![](https://www.yuque.com/api/services/graph/generate_redirect/latex?arg%5Cunderset%7B%CE%B1_1%2C%CE%B1_2%7D%7Bmax%7DW(%CE%B1_1%2C%CE%B1_2)#card=math&code=arg%5Cunderset%7B%CE%B1_1%2C%CE%B1_2%7D%7Bmax%7DW%28%CE%B1_1%2C%CE%B1_2%29&id=VAY7k)

根据约束条件![](https://www.yuque.com/api/services/graph/generate_redirect/latex?%5Csum_%7Bi%3D1%7D%5E%7Bm%7D%CE%B1_iy%20_i%3D0#card=math&code=%5Csum_%7Bi%3D1%7D%5E%7Bm%7D%CE%B1_iy%20_i%3D0&id=s1x4I)可得到![](https://www.yuque.com/api/services/graph/generate_redirect/latex?%CE%B1_1#card=math&code=%CE%B1_1&id=Juj0Z)与![](https://www.yuque.com/api/services/graph/generate_redirect/latex?%CE%B1_2#card=math&code=%CE%B1_2&id=FulRr)的关系：

![](https://www.yuque.com/api/services/graph/generate_redirect/latex?%CE%B1_1y_1%2B%CE%B1_2y_2%3D-%5Csum_%7Bi%3D3%7D%5E%7BN%7Da_iy_i%3D%CE%BE#card=math&code=%CE%B1_1y_1%2B%CE%B1_2y_2%3D-%5Csum_%7Bi%3D3%7D%5E%7BN%7Da_iy_i%3D%CE%BE&id=HKeCa)

两边同时乘上![](https://www.yuque.com/api/services/graph/generate_redirect/latex?y_1#card=math&code=y_1&id=UCLSE)，由于![](https://www.yuque.com/api/services/graph/generate_redirect/latex?y_iy_i%3D1#card=math&code=y_iy_i%3D1&id=lCEwB)得到：

![](https://www.yuque.com/api/services/graph/generate_redirect/latex?%CE%B1_1%3D%CE%BEy_1-%CE%B1_2y_1y_2#card=math&code=%CE%B1_1%3D%CE%BEy_1-%CE%B1_2y_1y_2&id=qnCDZ)

令![](https://www.yuque.com/api/services/graph/generate_redirect/latex?v1%3D%5Csum_%7Bi%3D3%7D%5E%7BN%7D%CE%B1_iy_iK_%7Bi%2C1%7D#card=math&code=v1%3D%5Csum_%7Bi%3D3%7D%5E%7BN%7D%CE%B1_iy_iK_%7Bi%2C1%7D&id=L3Vwb)，![](https://www.yuque.com/api/services/graph/generate_redirect/latex?v2%3D%5Csum_%7Bi%3D3%7D%5E%7BN%7D%CE%B1_iy_iK_%7Bi%2C2%7D#card=math&code=v2%3D%5Csum_%7Bi%3D3%7D%5E%7BN%7D%CE%B1_iy_iK_%7Bi%2C2%7D&id=SC0Ef)，带入上式得：

![](https://www.yuque.com/api/services/graph/generate_redirect/latex?W(%CE%B1_2)%3D-%5Cfrac%7B1%7D%7B2%7DK_%7B1%2C1%7D(%CE%BE-%CE%B1_2y_2)%5E2-%5Cfrac%7B1%7D%7B2%7DK_%7B2%2C2%7D%CE%B1_2%5E2-y_2(%CE%BE-%CE%B1_2y_2)%CE%B1_2K_%7B1%2C2%7D-v1(%CE%BE-%CE%B1_2y_2)-v_2y_2%CE%B1_2%2B%CE%B1_1%2B%CE%B1_2%2BC#card=math&code=W%28%CE%B1_2%29%3D-%5Cfrac%7B1%7D%7B2%7DK_%7B1%2C1%7D%28%CE%BE-%CE%B1_2y_2%29%5E2-%5Cfrac%7B1%7D%7B2%7DK_%7B2%2C2%7D%CE%B1_2%5E2-y_2%28%CE%BE-%CE%B1_2y_2%29%CE%B1_2K_%7B1%2C2%7D-v1%28%CE%BE-%CE%B1_2y_2%29-v_2y_2%CE%B1_2%2B%CE%B1_1%2B%CE%B1_2%2BC&id=V7v2T)

对![](https://www.yuque.com/api/services/graph/generate_redirect/latex?%CE%B1_2#card=math&code=%CE%B1_2&id=GOhzK)进行一次导为0得到：

![](https://www.yuque.com/api/services/graph/generate_redirect/latex?%5Cfrac%7B%D0%B1W(%CE%B1_2)%7D%7B%D0%B1%CE%B1_2%7D%3D-(K_%7B1%2C1%7D%2BK_%7B2%2C2%7D-2K_%7B1%2C2%7D)%CE%B1_2%2BK_%7B1%2C1%7D%CE%BEy_2-K_(1%2C2)%CE%BEy_2%2Bv1y_2-v2y2-y_1y_2%2By_2%5E2%3D0#card=math&code=%5Cfrac%7B%D0%B1W%28%CE%B1_2%29%7D%7B%D0%B1%CE%B1_2%7D%3D-%28K_%7B1%2C1%7D%2BK_%7B2%2C2%7D-2K_%7B1%2C2%7D%29%CE%B1_2%2BK_%7B1%2C1%7D%CE%BEy_2-K_%281%2C2%29%CE%BEy_2%2Bv1y_2-v2y2-y_1y_2%2By_2%5E2%3D0&id=yFxTL)

因为![](https://www.yuque.com/api/services/graph/generate_redirect/latex?w%20%3D%20%5Csum_%7Bi%3D1%7D%5E%7Bm%7D%CE%B1_iy_ix_i#card=math&code=w%20%3D%20%5Csum_%7Bi%3D1%7D%5E%7Bm%7D%CE%B1_iy_ix_i&id=j40I2)，所以SVM对数据点的预测值为：![](https://www.yuque.com/api/services/graph/generate_redirect/latex?f(x)%3D%5Csum_%7Bi%3D1%7D%5E%7BN%7D%CE%B1_iy_iK_%7Bx_i%2Cx%7D%2Bb#card=math&code=f%28x%29%3D%5Csum_%7Bi%3D1%7D%5E%7BN%7D%CE%B1_iy_iK_%7Bx_i%2Cx%7D%2Bb&id=QM8dm)

则![](https://www.yuque.com/api/services/graph/generate_redirect/latex?v_1#card=math&code=v_1&id=VnNkA)以及![](https://www.yuque.com/api/services/graph/generate_redirect/latex?v_2#card=math&code=v_2&id=dn97T)的值可以表示成：

![](https://www.yuque.com/api/services/graph/generate_redirect/latex?v1%3D%5Csum_%7Bi%3D3%7D%5E%7BN%7D%CE%B1_iy_iK_%7Bi%2C1%7D%3Df(x_1)-%CE%B1_1y_1K_%7B1%2C1%7D-%CE%B1_2y_2K_%7B1%2C2%7D-b#card=math&code=v1%3D%5Csum_%7Bi%3D3%7D%5E%7BN%7D%CE%B1_iy_iK_%7Bi%2C1%7D%3Df%28x_1%29-%CE%B1_1y_1K_%7B1%2C1%7D-%CE%B1_2y_2K_%7B1%2C2%7D-b&id=eg9WE)

![](https://www.yuque.com/api/services/graph/generate_redirect/latex?v2%3D%5Csum_%7Bi%3D3%7D%5E%7BN%7D%CE%B1_iy_iK_%7B2%2C1%7D%3Df(x_2)-%CE%B1_1y_1K_%7B1%2C2%7D-%CE%B1_2y_2K_%7B2%2C2%7D-b#card=math&code=v2%3D%5Csum_%7Bi%3D3%7D%5E%7BN%7D%CE%B1_iy_iK_%7B2%2C1%7D%3Df%28x_2%29-%CE%B1_1y_1K_%7B1%2C2%7D-%CE%B1_2y_2K_%7B2%2C2%7D-b&id=f77uT)

已知![](https://www.yuque.com/api/services/graph/generate_redirect/latex?%CE%B1_1%3Dy_1(%CE%BE-%CE%B1_2y_2)#card=math&code=%CE%B1_1%3Dy_1%28%CE%BE-%CE%B1_2y_2%29&id=d1WfP)，可得到：

![](https://www.yuque.com/api/services/graph/generate_redirect/latex?v_1%20-%20v_2%20%3D%20f(x_1)%20-%20f(x_2)%20-%20K_%7B1%2C1%7D%CE%BE%2BK_%7B1%2C2%7D%CE%BE%2B(K_%7B1%2C1%7D%2BK_%7B2%2C2%7D-2K_%7B1%2C2%7D)%CE%B1_2y_2#card=math&code=v_1%20-%20v_2%20%3D%20f%28x_1%29%20-%20f%28x_2%29%20-%20K_%7B1%2C1%7D%CE%BE%2BK_%7B1%2C2%7D%CE%BE%2B%28K_%7B1%2C1%7D%2BK_%7B2%2C2%7D-2K_%7B1%2C2%7D%29%CE%B1_2y_2&id=dhmyF)

将![](https://www.yuque.com/api/services/graph/generate_redirect/latex?v1-v2#card=math&code=v1-v2&id=H2ZM0)的表达式代入![](https://www.yuque.com/api/services/graph/generate_redirect/latex?%5Cfrac%7B%D0%B1W(%CE%B1_2)%7D%7B%D0%B1%CE%B1_2%7D#card=math&code=%5Cfrac%7B%D0%B1W%28%CE%B1_2%29%7D%7B%D0%B1%CE%B1_2%7D&id=CDpoN)中可以得到：

![](https://www.yuque.com/api/services/graph/generate_redirect/latex?%5Cfrac%7B%D0%B1W(%CE%B1_2)%7D%7B%D0%B1%CE%B1_2%7D%3D-(K_%7B1%2C1%7D%2BK_%7B2%2C2%7D-2K_%7B1%2C2%7D)%CE%B1_2%5E%7Bnew%7D%2B(K_%7B1%2C1%7D%2BK_%7B2%2C2%7D-2K_%7B1%2C2%7D)%CE%B1_2%5E%7Bold%7D%2By_2%5By_2-y_1%2Bf(x_1)-f(x_2)%5D#card=math&code=%5Cfrac%7B%D0%B1W%28%CE%B1_2%29%7D%7B%D0%B1%CE%B1_2%7D%3D-%28K_%7B1%2C1%7D%2BK_%7B2%2C2%7D-2K_%7B1%2C2%7D%29%CE%B1_2%5E%7Bnew%7D%2B%28K_%7B1%2C1%7D%2BK_%7B2%2C2%7D-2K_%7B1%2C2%7D%29%CE%B1_2%5E%7Bold%7D%2By_2%5By_2-y_1%2Bf%28x_1%29-f%28x_2%29%5D&id=EUGLs)

我们记![](https://www.yuque.com/api/services/graph/generate_redirect/latex?E_i#card=math&code=E_i&id=lQJgI)为**SVM预测值与真实值**的**误差**：![](https://www.yuque.com/api/services/graph/generate_redirect/latex?E_i%3Df(x_i)-y_i#card=math&code=E_i%3Df%28x_i%29-y_i&id=a6es3)

令![](https://www.yuque.com/api/services/graph/generate_redirect/latex?%CE%B7%3DK_%7B1%2C1%7D%2BK_%7B2%2C2%7D-2K_%7B1%2C2%7D#card=math&code=%CE%B7%3DK_%7B1%2C1%7D%2BK_%7B2%2C2%7D-2K_%7B1%2C2%7D&id=viehk)得到最终的一阶导数表达式：

![](https://www.yuque.com/api/services/graph/generate_redirect/latex?%5Cfrac%7B%D0%B1W(%CE%B1_2)%7D%7B%D0%B1%CE%B1_2%7D%3D-%CE%B7%CE%B1_2%5E%7Bnew%7D%2B%CE%B7%CE%B1_2%5E%7Bold%7D%2By_2(E_1-E_2)%20%3D%200#card=math&code=%5Cfrac%7B%D0%B1W%28%CE%B1_2%29%7D%7B%D0%B1%CE%B1_2%7D%3D-%CE%B7%CE%B1_2%5E%7Bnew%7D%2B%CE%B7%CE%B1_2%5E%7Bold%7D%2By_2%28E_1-E_2%29%20%3D%200&id=cReel)

得到更新公式：

![](https://www.yuque.com/api/services/graph/generate_redirect/latex?%CE%B1_2%5E%7Bnew%7D%3D%CE%B1_2%5E%7Bold%7D%2B%5Cfrac%7By_2(E_1-E_2)%7D%7B%CE%B7%7D#card=math&code=%CE%B1_2%5E%7Bnew%7D%3D%CE%B1_2%5E%7Bold%7D%2B%5Cfrac%7By_2%28E_1-E_2%29%7D%7B%CE%B7%7D&id=rRtfx)

而通过![](https://www.yuque.com/api/services/graph/generate_redirect/latex?%CE%B1_1%5E%7Bnew%7D%3Dy_1(%CE%BE-%CE%B1_2%5E%7Bnew%7Dy_2)#card=math&code=%CE%B1_1%5E%7Bnew%7D%3Dy_1%28%CE%BE-%CE%B1_2%5E%7Bnew%7Dy_2%29&id=o1YrI)便可以得到![](https://www.yuque.com/api/services/graph/generate_redirect/latex?%CE%B1_1%5E%7Bnew%7D#card=math&code=%CE%B1_1%5E%7Bnew%7D&id=LuHnq)

对原始解进行条件约束下的修剪，将上述的![](https://www.yuque.com/api/services/graph/generate_redirect/latex?%CE%B1_2%5E%7Bnew%7D#card=math&code=%CE%B1_2%5E%7Bnew%7D&id=tdHtA)改为![](https://www.yuque.com/api/services/graph/generate_redirect/latex?%CE%B1_2%5E%7Bnew%2Cunclipped%7D#card=math&code=%CE%B1_2%5E%7Bnew%2Cunclipped%7D&id=zIpgR)，即：

![](https://www.yuque.com/api/services/graph/generate_redirect/latex?%CE%B1_2%5E%7Bnew%2Cunclipped%7D%3D%CE%B1_2%5E%7Bold%7D%2B%5Cfrac%7By_2(E_1-E_2)%7D%7B%CE%B7%7D#card=math&code=%CE%B1_2%5E%7Bnew%2Cunclipped%7D%3D%CE%B1_2%5E%7Bold%7D%2B%5Cfrac%7By_2%28E_1-E_2%29%7D%7B%CE%B7%7D&id=IsONT)

上式需要遵循拉**格朗日乘法**中的条件：

![](https://www.yuque.com/api/services/graph/generate_redirect/latex?s.t.%5Csum_%7Bi%3D1%7D%5E%7Bm%7D%CE%B1_iy%20_i%3D0#card=math&code=s.t.%5Csum_%7Bi%3D1%7D%5E%7Bm%7D%CE%B1_iy%20_i%3D0&id=u5zU6) 即 ![](https://www.yuque.com/api/services/graph/generate_redirect/latex?%CE%B1_1y_1%2B%CE%B1_2y_2%20%3D%20-%5Csum_%7Bi%3D3%7D%5E%7BN%7D%CE%B1_iy_i%20%3D%20%CE%BE#card=math&code=%CE%B1_1y_1%2B%CE%B1_2y_2%20%3D%20-%5Csum_%7Bi%3D3%7D%5E%7BN%7D%CE%B1_iy_i%20%3D%20%CE%BE&id=RW3qn)

![](https://www.yuque.com/api/services/graph/generate_redirect/latex?0%E2%89%A6%CE%B1_i%E2%89%A6C%2Ci%3D1%2C2%2C...%2Cm#card=math&code=0%E2%89%A6%CE%B1_i%E2%89%A6C%2Ci%3D1%2C2%2C...%2Cm&id=hjfrz)

满足**方形约束(Bosk constraint)**,在二维平面上如下图所示
![](https://cdn.jsdelivr.net/gh/xuezhaorong/Picgo//Source/fix-dir/F/Download/2024/05/14/16-24-08-758db5c416a794b67a9d97dfc1655767-v2-449670775bab3c385b5e5930fc6d2caa_720w-133833.png#id=ns35Q&originHeight=296&originWidth=720&originalType=binary&ratio=1&rotation=0&showTitle=false&status=done&style=none)

当![](https://www.yuque.com/api/services/graph/generate_redirect/latex?y_1%E2%89%A0y_2#card=math&code=y_1%E2%89%A0y_2&id=uIWp4)时，线性限制条件可以写成：![](https://www.yuque.com/api/services/graph/generate_redirect/latex?%CE%B1_1-%CE%B1_2%3Dk#card=math&code=%CE%B1_1-%CE%B1_2%3Dk&id=ZnbJT),根据`k`的正负可以得到不同的上下界：

![](https://www.yuque.com/api/services/graph/generate_redirect/latex?L%3Dmax(0%2C-k)%3Dmax(0%2C%CE%B1_2%5E%7Bold%7D-%CE%B1_1%5E%7Bold%7D)#card=math&code=L%3Dmax%280%2C-k%29%3Dmax%280%2C%CE%B1_2%5E%7Bold%7D-%CE%B1_1%5E%7Bold%7D%29&id=VggWH)

![](https://www.yuque.com/api/services/graph/generate_redirect/latex?H%3Dmin(C%2CC-k)%3Dmin(C%2CC%2B%CE%B1_2%5E%7Bold%7D-%CE%B1_1%5E%7Bold%7D)#card=math&code=H%3Dmin%28C%2CC-k%29%3Dmin%28C%2CC%2B%CE%B1_2%5E%7Bold%7D-%CE%B1_1%5E%7Bold%7D%29&id=A6qtq)

当![](https://www.yuque.com/api/services/graph/generate_redirect/latex?y_1%3Dy_2#card=math&code=y_1%3Dy_2&id=B66vV)时，限制条件可写成：![](https://www.yuque.com/api/services/graph/generate_redirect/latex?%CE%B1_1%2B%CE%B1_2%3Dk#card=math&code=%CE%B1_1%2B%CE%B1_2%3Dk&id=hzIMJ),上下界表示为：

![](https://www.yuque.com/api/services/graph/generate_redirect/latex?L%3Dmax(0%2Ck-C)%3Dmax(0%2C%CE%B1_1%5E%7Bold%7D%2B%CE%B1_2%5E%7Bokd%7D-C)#card=math&code=L%3Dmax%280%2Ck-C%29%3Dmax%280%2C%CE%B1_1%5E%7Bold%7D%2B%CE%B1_2%5E%7Bokd%7D-C%29&id=thAjn)

![](https://www.yuque.com/api/services/graph/generate_redirect/latex?H%3Dmin(C%2Ck)%3Dmin(C%2C%CE%B1_2%5E%7Bold%7D%2B%CE%B1_1%5E%7Bold%7D)#card=math&code=H%3Dmin%28C%2Ck%29%3Dmin%28C%2C%CE%B1_2%5E%7Bold%7D%2B%CE%B1_1%5E%7Bold%7D%29&id=PQ4Xb)

根据上下界，可以得到修剪后的![](https://www.yuque.com/api/services/graph/generate_redirect/latex?%CE%B1_2#card=math&code=%CE%B1_2&id=d7ozu)：

![](https://www.yuque.com/api/services/graph/generate_redirect/latex?f(%CE%B1_2%5E%7Bnew%7D)%3D%20%5Cbegin%7Bcases%7D%20H%26%20%CE%B1_2%5E%7Bnew%2Cunclipped%7D%3EH%20%5C%5C%20%CE%B1_2%5E%7Bmew%2Cunclippe%7D%26L%E2%89%A4%CE%B1_2%5E%7Bnew%2Cunclippe%7D%E2%89%A4H%20%5C%5CL%26%20%CE%B1_2%5E%7Bnew%2Cunclippe%7D%20%3CL%20%5Cend%7Bcases%7D#card=math&code=f%28%CE%B1_2%5E%7Bnew%7D%29%3D%20%5Cbegin%7Bcases%7D%20H%26%20%CE%B1_2%5E%7Bnew%2Cunclipped%7D%3EH%20%5C%5C%20%CE%B1_2%5E%7Bmew%2Cunclippe%7D%26L%E2%89%A4%CE%B1_2%5E%7Bnew%2Cunclippe%7D%E2%89%A4H%20%5C%5CL%26%20%CE%B1_2%5E%7Bnew%2Cunclippe%7D%20%3CL%20%5Cend%7Bcases%7D&id=Lt10Y)

得到![](https://www.yuque.com/api/services/graph/generate_redirect/latex?%CE%B1_2%5E%7Bnew%7D#card=math&code=%CE%B1_2%5E%7Bnew%7D&id=qG7Ax)后便可以根据![](https://www.yuque.com/api/services/graph/generate_redirect/latex?%CE%B1_1%5E%7Bold%7Dy_1%2B%CE%B1_2%5E%7Bold%7Dy_2%3D%CE%B1_1%5E%7Bnew%7Dy_1%2B%CE%B1_2%5E%7Bmew%7Dy_2#card=math&code=%CE%B1_1%5E%7Bold%7Dy_1%2B%CE%B1_2%5E%7Bold%7Dy_2%3D%CE%B1_1%5E%7Bnew%7Dy_1%2B%CE%B1_2%5E%7Bmew%7Dy_2&id=yS0xp)得到![](https://www.yuque.com/api/services/graph/generate_redirect/latex?%CE%B1_1%5E%7Bnew%7D#card=math&code=%CE%B1_1%5E%7Bnew%7D&id=DEyCF)：

![](https://www.yuque.com/api/services/graph/generate_redirect/latex?%CE%B1_1%5E%7Bnew%7D%3D%CE%B1_1%5E%7Bold%7D%2By_1y_2(%CE%B1_2%5E%7Bold%7D-%CE%B1_2%5E%7Bnew%7D)#card=math&code=%CE%B1_1%5E%7Bnew%7D%3D%CE%B1_1%5E%7Bold%7D%2By_1y_2%28%CE%B1_2%5E%7Bold%7D-%CE%B1_2%5E%7Bnew%7D%29&id=PJd9R)

更新阈值b

当更新了一对`α`之后都要重新计算阈值`b`,因为`b`隐式地关系到![](https://www.yuque.com/api/services/graph/generate_redirect/latex?f(x)#card=math&code=f%28x%29&id=w7dYk)的计算，关系到下一优化的时候误差![](https://www.yuque.com/api/services/graph/generate_redirect/latex?E_i#card=math&code=E_i&id=rdp1p)的计算

在**KKT**条件中：

- 如果$ α_i = 0$，则该样本应该正确分类，并且在间隔边界之外。
- 如果![](https://www.yuque.com/api/services/graph/generate_redirect/latex?0%20%3C%20%CE%B1_i%20%3C%20C#card=math&code=0%20%3C%20%CE%B1_i%20%3C%20C&id=ZawEK) ，则该样本应该正好位于间隔边界上，并且是**支持向量**。
- 如果![](https://www.yuque.com/api/services/graph/generate_redirect/latex?%CE%B1_i%3DC#card=math&code=%CE%B1_i%3DC&id=qzY3S) ，则该样本可能在间隔边界之内或者被错误分类。

当![](https://www.yuque.com/api/services/graph/generate_redirect/latex?%CE%B1_1%5E%7Bnew%7D#card=math&code=%CE%B1_1%5E%7Bnew%7D&id=Yro25)不在边界，即![](https://www.yuque.com/api/services/graph/generate_redirect/latex?0%3C%CE%B1_1%5E%7Bnew%7D%3CC#card=math&code=0%3C%CE%B1_1%5E%7Bnew%7D%3CC&id=temHR)，相应的数据点为支持向量，满足![](https://www.yuque.com/api/services/graph/generate_redirect/latex?y_1(w%5ET%2Bb)%3D1#card=math&code=y_1%28w%5ET%2Bb%29%3D1&id=UylRc)，同时乘上![](https://www.yuque.com/api/services/graph/generate_redirect/latex?y_1#card=math&code=y_1&id=uy1ew)得到![](https://www.yuque.com/api/services/graph/generate_redirect/latex?%5Csum_%7Bi%3D1%7D%5E%7BN%7D%CE%B1_iy_iK_(i%2C1)%2Bb%3Dy_1#card=math&code=%5Csum_%7Bi%3D1%7D%5E%7BN%7D%CE%B1_iy_iK_%28i%2C1%29%2Bb%3Dy_1&id=pdvmx)，进而得到![](https://www.yuque.com/api/services/graph/generate_redirect/latex?b_1%5E%7Bnew%7D#card=math&code=b_1%5E%7Bnew%7D&id=sZjVd)的值：

![](https://www.yuque.com/api/services/graph/generate_redirect/latex?b_1%5E%7Bnew%7D%3Dy_1-%5Csum_%7Bi%3D3%7D%5E%7BN%7D%CE%B1_iy_iK_%7Bi%2C1%7D-%CE%B1_1%5E%7Bnew%7Dy_iK_%7B1%2C1%7D-%CE%B1_2%5E%7Bnew%7Dy_2K_%7B2%2C1%7D#card=math&code=b_1%5E%7Bnew%7D%3Dy_1-%5Csum_%7Bi%3D3%7D%5E%7BN%7D%CE%B1_iy_iK_%7Bi%2C1%7D-%CE%B1_1%5E%7Bnew%7Dy_iK_%7B1%2C1%7D-%CE%B1_2%5E%7Bnew%7Dy_2K_%7B2%2C1%7D&id=WGxab)

其中上式的前两项可以写成：

![](https://www.yuque.com/api/services/graph/generate_redirect/latex?y_1-%5Csum_%7Bi%3D3%7D%5E%7BN%7D%CE%B1_iy_iK_%7Bi%2C1%7D%3D-E_i%2B%CE%B1_1%5E%7Bold%7Dy_1K_%7B1%2C1%7D%2B%CE%B1_2%5E%7Bold%7Dy_2K_%7B2%2C1%7D%2Bb%5E%7Bold%7D#card=math&code=y_1-%5Csum_%7Bi%3D3%7D%5E%7BN%7D%CE%B1_iy_iK_%7Bi%2C1%7D%3D-E_i%2B%CE%B1_1%5E%7Bold%7Dy_1K_%7B1%2C1%7D%2B%CE%B1_2%5E%7Bold%7Dy_2K_%7B2%2C1%7D%2Bb%5E%7Bold%7D&id=R14kN)

同样的![](https://www.yuque.com/api/services/graph/generate_redirect/latex?b_2%5E%7Bnew%7D#card=math&code=b_2%5E%7Bnew%7D&id=XVeW2)推导为：

![](https://www.yuque.com/api/services/graph/generate_redirect/latex?b_2%5E%7Bnew%7D%3D-E_2-y_1K_%7B1%2C2%7D(%CE%B1_1%5E%7Bnew%7D-%CE%B1_1%5E%7Bold%7D)-y_2K_%7B2%2C2%7D(%CE%B1_2%5E%7Bnew%7D-%CE%B1_2%5E%7Bold%7D%2Bb%5E%7Bold%7D)#card=math&code=b_2%5E%7Bnew%7D%3D-E_2-y_1K_%7B1%2C2%7D%28%CE%B1_1%5E%7Bnew%7D-%CE%B1_1%5E%7Bold%7D%29-y_2K_%7B2%2C2%7D%28%CE%B1_2%5E%7Bnew%7D-%CE%B1_2%5E%7Bold%7D%2Bb%5E%7Bold%7D%29&id=xvIdf)

当![](https://www.yuque.com/api/services/graph/generate_redirect/latex?b_1#card=math&code=b_1&id=sDi7n)和![](https://www.yuque.com/api/services/graph/generate_redirect/latex?b_2#card=math&code=b_2&id=py9Kx)都有效的时候他们是想等的，即![](https://www.yuque.com/api/services/graph/generate_redirect/latex?b%5E%7Bnew%7D%3Db_1%5E%7Bnew%7D%3Db_2%5E%7Bnew%7D#card=math&code=b%5E%7Bnew%7D%3Db_1%5E%7Bnew%7D%3Db_2%5E%7Bnew%7D&id=UyZBd)

当两个乘子都在边界上且![](https://www.yuque.com/api/services/graph/generate_redirect/latex?L%E2%89%A0H#card=math&code=L%E2%89%A0H&id=gXRsm)时，![](https://www.yuque.com/api/services/graph/generate_redirect/latex?b_1%2Cb_2#card=math&code=b_1%2Cb_2&id=mcKSt)之间的值为KKT条件一直的阈值，SMO选择他们的中点作为新的阈值：

![](https://www.yuque.com/api/services/graph/generate_redirect/latex?b%5E%7Bnew%7D%3D%5Cfrac%7Bb_1%5E%7Bnew%7D%2Bb_2%5E%7Bnew%7D%7D%7B2%7D#card=math&code=b%5E%7Bnew%7D%3D%5Cfrac%7Bb_1%5E%7Bnew%7D%2Bb_2%5E%7Bnew%7D%7D%7B2%7D&id=fWYiC)

代码实现：

注意其中每次迭代中选择一对α的策略：

第一个选取的α违背KKT条件的程度越大，则变量更新后可能导致的目标函数值增幅越大，第二个变量应选择一个使得目标函数增长最快的变量，这里选用了启发式的增长标准为:![](https://www.yuque.com/api/services/graph/generate_redirect/latex?%7CE_1-E_2%7C#card=math&code=%7CE_1-E_2%7C&id=o0BZd)

```python
import numpy as np
# from sklearn import svm

class SVC:
    def __init__(self, kernel='linear', C=1,max_iter=100):
        self.Y = []
        self.X = []
        self.m = 0
        self.E = []
        self.product_matrix = []
        self.alpha = []
        self._kernel = kernel
        self.b = 0.0
        self.C = C
        self.max_iter = max_iter

    def kernel(self, x1, x2):
        # 线性核
        if self._kernel == 'linear':
            return np.dot(x1, x2)
        # 多项式核 2次
        elif self._kernel == 'poly':
            return (np.dot(x1, x2)) ** 2
        return 0

    # 计算内积矩阵
    def computer_product_matrix(self, X):
        self.product_matrix = np.zeros((self.m, self.m)).astype(float)
        for i in range(self.m):
            for j in range(self.m):
                if self.product_matrix[i][j] == 0.0:
                    self.product_matrix[i][j] = self.product_matrix[j][i] = self.kernel(X[i], X[j])

    # 预测函数g(x)
    def function_g(self, i):
        return self.b + np.dot(np.dot(self.alpha , self.Y), self.product_matrix[i])

    # KKT条件判断
    def KKT(self, i):
        y_g = self.function_g(i) * self.Y[i]
        if self.alpha[i] == 0:
            return y_g >= 1
        elif 0 < self.alpha[i] < self.C:
            return y_g == 1
        else:
            return y_g <= 1

    # 选择变量
    def select_alpha(self):
        # 外层循环首先遍历所有满足0<a<C的样本点，检验是否满足KKT
        index_list = [i for i in range(self.m) if 0 < self.alpha[i] < self.C]
        # 否则遍历整个训练集
        non_satisfy_list = [i for i in range(self.m) if i not in index_list]
        index_list.extend(non_satisfy_list)
        for i in index_list:
            if self.KKT(i):
                continue
            Ei = self.E[i]
            # 如果E是+，选择最小的；如果E2是负的，选择最大的 max|Ei - Ej|
            if Ei >= 0:
                j = np.argmin(self.E)
            else:
                j = np.argmax(self.E)
            return i, j

    # 剪切
    def clip_alpha(self, _alpha, L, H):
        if _alpha > H:
            return H
        elif _alpha < L:
            return L
        else:
            return _alpha

    # 将Ei保存在一个列表里
    def create_E(self, Y):
        self.E = (np.dot((self.alpha * Y), self.product_matrix) + self.b) - Y

    def fit(self, X, Y):
        self.m = len(X)
        self.Y = Y
        self.X = X
        self.alpha = np.ones(self.m)
        self.computer_product_matrix(X)
        self.create_E(Y)

        for t in range(self.max_iter):
            alpha_i, alpha_j = self.select_alpha()

            # 上下界 判断y_i == y_j
            # 边界
            if Y[alpha_i] == Y[alpha_j]:
                L = max(0.0, self.alpha[alpha_i] + self.alpha[alpha_j] - self.C)
                H = min(float(self.C), self.alpha[alpha_i] + self.alpha[alpha_j])
            else:
                L = max(0.0, self.alpha[alpha_j] - self.alpha[alpha_i])
                H = min(float(self.C), self.C + self.alpha[alpha_j] - self.alpha[alpha_i])

            Ei = self.E[alpha_i]
            Ej = self.E[alpha_j]

            eta = self.kernel(X[alpha_i], X[alpha_i]) + self.kernel(X[alpha_j], X[alpha_j]) - 2 * self.kernel(
                X[alpha_i], X[alpha_j])
            if eta <= 0:
                continue

            alpha2_new_unc = self.alpha[alpha_i] + Y[alpha_j] * (Ei - Ej) / eta  # 此处有修改，根据书上应该是E1 - E2，书上130-131页
            alpha2_new = self.clip_alpha(alpha2_new_unc, L, H)

            alpha1_new = self.alpha[alpha_i] + Y[alpha_j] * Y[alpha_j] * (self.alpha[alpha_j] - alpha2_new)

            b1_new = (-Ei - Y[alpha_i] * self.kernel(X[alpha_i], X[alpha_i]) * (alpha1_new - self.alpha[alpha_i]) -
                      Y[alpha_j] * self.kernel(X[alpha_j], X[alpha_i]) * (alpha2_new - self.alpha[alpha_j]) + self.b)
            b2_new = (-Ej - Y[alpha_i] * self.kernel(X[alpha_i], X[alpha_j]) * (alpha1_new - self.alpha[alpha_i]) -
                      Y[alpha_j] * self.kernel(X[alpha_j], X[alpha_j]) * (alpha2_new - self.alpha[alpha_j]) + self.b)

            # 判断边界
            if 0 < alpha1_new < self.C:
                b_new = b1_new
            elif 0 < alpha2_new < self.C:
                b_new = b2_new
            else:
                # 选择中点
                b_new = (b1_new + b2_new) / 2

            # 更新参数
            self.alpha[alpha_i] = alpha1_new
            self.alpha[alpha_j] = alpha2_new
            self.b = b_new

            self.create_E(Y)

    def predict(self, X):
        r = self.b
        c = np.array([])
        for x in X:
            for i in range(self.m):
                r += self.alpha[i] * self.Y[i] * self.kernel(x, self.X[i])
            c = np.append(c,r)
            r = self.b

        return np.array([[1] if i > 0 else [0] for i in c]).reshape(-1,1)
```
