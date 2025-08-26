# 模型压缩概述

## 背景

- 为什么要模型压缩？

    端侧设备存在资源限制

- 为什么能模型压缩？

    深度神经网络模型存在冗余性

- 什么是模型压缩？

    利用网络模型冗余性的特点，减小模型规模的方法

## 模型压缩方法

- 剪枝：修剪不重要的网络连接
- 量化：将连续型数据量化为低位宽离散数据
- 知识蒸馏：大模型指导小模型学习
- 低秩分解：通过低秩矩阵近似原矩阵
- 轻量化网络：使用轻量化卷积核代替传统卷积
- 网络结构搜索：自动化地设计优异网络模型

## 剪枝

![剪枝](Overview_of_Model_Compression/image.png)

## 量化

![量化](Overview_of_Model_Compression/image-1.png)

## 知识蒸馏

![知识蒸馏](Overview_of_Model_Compression/image-2.png)

## 低秩分解

![低秩分解](Overview_of_Model_Compression/image-3.png)

## 轻量化网络

![轻量化网络](Overview_of_Model_Compression/image-4.png)

## 网络结构搜索

![网络结构搜索](Overview_of_Model_Compression/image-5.png)
