# Self-Attention 机制学习笔记

## 1. 基本概念

### 向量序列作为输入
![向量序列输入示例](Self_attention/image.png)
![输入向量的特征表示](Self_attention/image-1.png)
![多个向量组成的序列](Self_attention/image-2.png)

### 输出结果
![Self-attention的输出](Self_attention/image-3.png)

## 2. Self-Attention机制

### 基本原理
![Self-attention基本概念](Self_attention/image-4.png)
![注意力权重计算](Self_attention/image-5.png)

### 寻找序列中的相关向量
![查询相关向量的过程](Self_attention/image-6.png)
![注意力分数计算](Self_attention/image-7.png)
![softmax归一化处理](Self_attention/image-8.png)
![加权求和得到输出](Self_attention/image-9.png)
![完整的attention流程](Self_attention/image-10.png)

### 矩阵表示方法
![Q、K、V矩阵计算](Self_attention/image-11.png)
![矩阵乘法实现attention](Self_attention/image-12.png)
![并行计算所有位置](Self_attention/image-13.png)
![最终的矩阵运算公式](Self_attention/image-14.png)

## 3. Multi-head Self-attention

![多头注意力机制](Self_attention/image-15.png)
![多头注意力的计算过程](Self_attention/image-16.png)

## 4. Positional Encoding (位置编码)

![位置编码的必要性和实现](Self_attention/image-17.png)

## 5. Self-attention的应用

### 语音处理
![Self-attention在语音识别中的应用](Self_attention/image-18.png)

### 图像处理
![Self-attention在图像处理中的应用](Self_attention/image-19.png)

## 6. 模型比较

### Self-attention vs CNN
![Self-attention与卷积神经网络的比较](Self_attention/image-20.png)

### Self-attention vs RNN
![Self-attention与循环神经网络的比较](Self_attention/image-21.png)

### 图网络应用
![Self-attention在图网络中的应用](Self_attention/image-22.png)
