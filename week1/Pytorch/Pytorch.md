# PyTorch 神经网络训练与测试指南

## 1. 数据加载 (Data Loading)

![alt text](Pytorch/image.png)
![alt text](Pytorch/image-1.png)
![alt text](Pytorch/image-3.png)
![alt text](Pytorch/image-2.png)

## 2. 张量 (Tensors)

### 2.1 创建张量 (Creating Tensors)
![alt text](Pytorch/image-4.png)

**维度顺序：高宽长**
![alt text](Pytorch/image-5.png)

### 2.2 常用操作 (Common Operations)
![alt text](Pytorch/image-6.png)
![alt text](Pytorch/image-7.png)
![alt text](Pytorch/image-8.png)
![alt text](Pytorch/image-9.png)
![alt text](Pytorch/image-10.png)

### 2.3 数据类型 (Data Types)
### 2.3 数据类型 (Data Types)

| Data type | dtype | tensor |
|---|---|---|
| 32-bit floating point | torch.float | torch.FloatTensor |
| 64-bit integer(signed) | torch.long | torch.LongTensor |
| 64-bit floating point | torch.double | torch.DoubleTensor |
| 16-bit floating point | torch.half | torch.HalfTensor |
| 8-bit integer(unsigned) | torch.uint8 | torch.ByteTensor |
| 8-bit integer(signed) | torch.int8 | torch.CharTensor |
| 16-bit integer(signed) | torch.short | torch.ShortTensor |
| 32-bit integer(signed) | torch.int | torch.IntTensor |
| Boolean | torch.bool | torch.BoolTensor |

### 2.4 设备选择 (Device Selection)
![alt text](Pytorch/image-11.png)

### 2.5 梯度计算 (Gradient Calculation)
![alt text](Pytorch/image-12.png)

## 3. 神经网络模块 (torch.nn.module)
## 3. 神经网络模块 (torch.nn.module)

![alt text](Pytorch/image-13.png)
![alt text](Pytorch/image-14.png)
![alt text](Pytorch/image-15.png)
![alt text](Pytorch/image-16.png)
![alt text](Pytorch/image-17.png)
![alt text](Pytorch/image-18.png)

## 4. 神经网络循环 (Neural Network Loops)

### 4.1 训练循环 (Training Loop)
![alt text](Pytorch/image-19.png)

### 4.2 验证循环 (Validation Loop)
![alt text](Pytorch/image-20.png)

### 4.3 测试循环 (Testing Loop)
![alt text](Pytorch/image-21.png)

## 5. 模型保存与加载 (Save/Load Trained Model)

![alt text](Pytorch/image-22.png)