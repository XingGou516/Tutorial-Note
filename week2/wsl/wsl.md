# WSL Ubuntu 22.04 配置学习笔记

本笔记整理了WSL (Windows Subsystem for Linux) Ubuntu 22.04的完整配置指南，包括安装、配置、优化等各个方面。

## 目录结构

- **[installation.md](./installation.md)** - WSL安装指南（标准安装、离线安装、自定义位置）
- **[management.md](./management.md)** - WSL管理和维护（重命名、迁移、备份、多版本管理）
- **[development.md](./development.md)** - 开发环境配置（编程语言、数据库、容器、VS Code集成、Linux基础）

## 快速开始

### 基本安装流程
1. 启用WSL功能和虚拟机平台
2. 重启计算机
3. 安装Ubuntu发行版
4. 基础配置（用户账户、换源、基础工具）

### 系统要求
- Windows 10 版本 2004+ 或 Windows 11
- 启用虚拟化功能的处理器
- 推荐 16GB RAM（最低 8GB）

## 主要特性

### WSL优势
- 在Windows上原生运行Linux环境
- 与Windows系统深度集成
- 支持GPU加速和WSL2虚拟化
- 完整的Linux文件系统和包管理

### 支持的功能
- 多发行版并存管理
- 自定义安装位置
- 内存和CPU资源控制
- VS Code无缝集成
- Docker容器支持

## 学习路径

1. **入门**：基础安装和配置
2. **进阶**：多版本管理和系统优化
3. **开发**：完整开发环境搭建
4. **运维**：备份、迁移和故障排除

## 参考资料

本笔记整理参考了以下资料：

### 官方文档
- [Microsoft WSL 官方文档](https://learn.microsoft.com/zh-cn/windows/wsl/) - Windows Subsystem for Linux 官方完整文档

### 参考博客文章
1. [Windows10/11 三步安装wsl2 Ubuntu20.04（任意盘）](https://zhuanlan.zhihu.com/p/466001838) - 林夕丶
2. [在网络状况不佳的情况下离线安装wsl2/linux发行版](https://zhuanlan.zhihu.com/p/640621418) - 一只大肥猫呦
3. [【调试笔记-20240522-Windows-WSL 修改已安装发行版名称】](https://blog.csdn.net/dvd37784302/article/details/139106309) - David唐辉
4. [在 Win11安装 Ubuntu20.04子系统 WSL2 到其他盘](https://blog.csdn.net/orange1710/article/details/131904929) - 1710orange
5. [Linux命令行基础教程](https://www.kancloud.cn/thinkphp/linux-command-line/39431) - Linux命令行基础知识参考

### 社区资源
- [WSL GitHub 仓库](https://github.com/microsoft/WSL)

---
*最后更新：2025年7月22日*