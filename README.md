竞争性自适应重加权采样法（competitive adapative reweighted sampling， CARS）是一种结合蒙特卡洛采样与PLS模型回归系数的特征变量选择方法，模仿达尔文理论中的 ”适者生存“ 的原则（Li et al., 2009）。CARS 算法中，每次通过自适应加权采样（adapative reweighted sampling， ARS）保留PLS模型中 回归系数绝对值权重较大的点作为新的子集，去掉权值较小的点，然后基于新的子集建立PLS模型，经过多次计算，选择PLS模型交互验证均方根误差（RMSECV）最小的子集中的波长作为特征波长。

CARS算法的具体过程如下。

1. 采用 蒙特卡洛采样法，每次随机从校正集中选择一定数量（一般为80%）的样本进入建模集，剩余的20%作为预测集建立PLS模型。蒙特卡洛的采样次数（N）需要提前设定。记录每一次采样过程PLS模型中的回归系数的绝对值权重，|b_i|为第i个变量的回归系数绝对值，w_i为第i个变量的回归系数绝对值权重

   ![image-20210730111928657](https://gitee.com/aBugsLife/imgReponsitory/raw/master/img/image-20210730111928657.png)

   w_i=|b_i|/\sum_{i=1}^m|b_i|
    m为每次采样中剩余的变量数。
2. 利用指数衰减函数（exponentially decreasing function， EDF）强行去除回归系数绝对值权重相对较小的波长。在第i次基于MC采样建立PLS模型时，根据EDF得到保留的波长点的比例R_i为

   ![image-20210730112026278](https://gitee.com/aBugsLife/imgReponsitory/raw/master/img/image-20210730112026278.png)

   R_i=\mu e^{-k_i}
   式中，\mu和k是常数，可以按照以下两种情况计算。
   1. 在一次采样并进行相应计算时，所有的波长都参与了建模分析，因此此时保留的波长点的比例为1。
   2. 在最后一次采样在（第N次）完成并进行相应计算时，只剩下两个波长参与PLS建模，此时保留的波长点的比例为 2/n，其中n是原始波长点数。
      由以上最初及最后一次采样的情况可知，\mu和k的计算公式为

      ![image-20210730112048527](https://gitee.com/aBugsLife/imgReponsitory/raw/master/img/image-20210730112048527.png)

      \mu=(\cfrac{n}{2})^{\cfrac{1}{N-1}},k=\cfrac{ln(\cfrac{n}{n})}{N-1}
3. 在每次采样时，都从上一次采样时的变量数中采用自适应加权采样（ARS）选择数量为R_i * n个的波长变量，进行PLS建模，计算RMSECV。
4. 在N次采样完成之后，CARS 算法得到了N组候选的特征波长子集，以及对应的RMSECV值，选择RMSECV最小值所对应的波长变量子集为特征波长。
   

说明： 竞争性自适应重加权算法（CARS）是通过自适应重加权采样（ARS）技术选择出PLS模型中回归系数绝对值大的波长点，去掉权重小的波长点，利用交互验证选出RMSECV指最低的子集，可有效寻出最优变量组合。

具体使用 请各位先阅读源码 查看 相关问题可先参考我的另一个spa项目，cars具有随机性，建议运行五次选取最佳rmsecv及波段数。具体更新挑错会在四月中下旬，如有问题请先留言。最好在issues中提交详细问题，确保可以复现问题

输入数据为np.ndarray！！！
