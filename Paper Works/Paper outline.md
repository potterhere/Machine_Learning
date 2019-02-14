# 低小慢雷达流水线式实时数据处理系统的研究与实现

## 绪论
1. 研究背景
2. 国内外现状
3. 本文创新与改进

## 雷达原理
1. 雷达目标检测原理 (信号处理)
2. 雷达系统框架
3. 雷达数据处理流程 Pipeline

## 航迹预测 与 航迹相关
1. 目标特征分析
2. 数据过滤方案 (杂波图等 噪声、异常数据抑制方法)
3. 航迹预测方案 (卡尔曼 、$\alpha - \beta - \gamma$、本项目采用的有效方法)
4. 航迹相关方案 (近邻、联合密度估计JPDA、本项目采用的有效方法)

## 设计与实现
1. 软件设计框架
   1. MVC 结构
   2. 数据提取 (网口报文转换数据结构)
   3. 扇区循环存放结构
   4. 多进程+多线程+队列 数据处理模块
   5. 航迹显示 (P显、A显、B显等)
   6. 数据输出 (数据库，UDP网络广播)
   
2. 实现方法
   Python + PyQt5 + SQLite + WinpCap
   
3. 优化方法
   * 利用队列通行 组成的流水线式并发 处理机制
   * 矩阵代替循环运算提升效率
   * 数据通信稳定响应机制
   * 鲁棒性设计

## 实现结果
1. 雷达数据处理效果
2. 满负荷性能测试
3. 数据处理准确度测试
4. 数据处理鲁棒性测试

## 总结与展望

## 感谢