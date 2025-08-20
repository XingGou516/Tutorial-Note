# Batch Normalization 学习笔记

## 1. Changing Landscape:
![训练过程中的环境变化](Batch_Normalization/image.png)

## 2. Feature Normalization:
![特征归一化概念](Batch_Normalization/image-1.png)

## 3. Batch Normalization-Training:
![批量归一化训练过程](Batch_Normalization/image-2.png)
![批量归一化算法步骤](Batch_Normalization/image-3.png)
γ 和 β 是需要学习的参数：
- γ：缩放参数，控制方差
- β：偏移参数，控制均值

![可学习参数示意图](Batch_Normalization/image-4.png)

## 4. Batch Normalization-Testing:
![批量归一化测试阶段](Batch_Normalization/image-5.png)