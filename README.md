# 竞争性自适应重加权算法-CARS-python版

#### 介绍
说明：  竞争性自适应重加权算法（CARS）是通过自适应重加权采样（ARS）技术选择出PLS模型中回归系数绝对值大的波长点，去掉权重小的波长点，利用交互验证选出RMSECV指最低的子集，可有效寻出最优变量组合。

具体使用 请各位先阅读源码 查看 相关问题可先参考我的另一个spa项目，cars具有随机性，建议运行五次选取最佳rmsecv及波段数。具体更新挑错会在四月中下旬，如有问题请先留言。最好在issues中提交详细问题，确保可以复现问题

输入数据为np.ndarray！！！
