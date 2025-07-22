# WSL Ubuntu 22.04 安装指南

## 前置要求

### 系统要求
- Windows 10 版本 2004 及更高版本（内部版本 19041+）
- Windows 11 任意版本
- 启用虚拟化功能的处理器

### 内存建议
- **最低要求**：8GB RAM
- **推荐配置**：16GB RAM 及以上
- **8GB内存用户**：可通过内存控制优化使用

## 安装方法选择

| 方法 | 适用场景 | 优点 | 缺点 |
|------|----------|------|------|
| 快速安装 | 网络良好，首次安装 | 简单快速 | 安装到C盘，需要网络 |
| 离线安装 | 网络受限，批量部署 | 稳定可重复 | 需要预下载 |
| 自定义安装 | 需要特定位置 | 灵活控制 | 步骤较多 |

## 方法一：快速安装

### 步骤1：启用WSL功能

以管理员身份打开PowerShell：

```powershell
# 一键安装WSL
wsl --install

# 手动启用功能（如果上述命令失败）
dism.exe /online /enable-feature /featurename:Microsoft-Windows-Subsystem-Linux /all /norestart
dism.exe /online /enable-feature /featurename:VirtualMachinePlatform /all /norestart
wsl --set-default-version 2
```

### 步骤2：重启系统
**重要**：必须重启计算机使功能生效

### 步骤3：安装Ubuntu
```powershell
# 查看可用发行版
wsl --list --online

# 安装Ubuntu 22.04
wsl --install -d Ubuntu-22.04
```

### 步骤4：初始配置
- 首次启动会自动打开Ubuntu终端
- 创建用户账户和密码
- 完成初始设置

## 方法二：离线安装

### 适用场景
- 网络连接不稳定
- 企业内网环境
- 批量部署需求
- 微软商店访问受限

### 步骤1：启用开发者模式
1. 打开Windows设置（Win + I）
2. 导航到"更新和安全" → "开发者选项"
3. 启用"开发人员模式"

### 步骤2：下载发行版

#### Ubuntu 22.04 LTS下载链接
```bash
# AMD64版本
https://wslstorestorage.blob.core.windows.net/wslblob/Ubuntu2204LTS-230518_x64.appx

# ARM64版本
https://wslstorestorage.blob.core.windows.net/wslblob/Ubuntu2204LTS-230518_ARM64.appx
```

#### 其他发行版
- **Ubuntu 20.04**: `https://wsldownload.azureedge.net/Ubuntu2004-230608_x64.appx`
- **Debian**: `https://wsldownload.azureedge.net/TheDebianProject.DebianGNULinux_1.12.2.0_neutral___76v4gfsz19hv4.AppxBundle`
- **Kali Linux**: `https://wsldownload.azureedge.net/KaliLinux_1.13.1.0.AppxBundle`

#### 下载方法
```powershell
# PowerShell下载
Invoke-WebRequest -Uri "下载链接" -OutFile "Ubuntu2204.appx" -UseBasicParsing

# 或使用浏览器/下载工具直接下载
```

### 步骤3：安装AppX包
```powershell
# 方法1：双击.appx文件安装
# 方法2：PowerShell安装
Add-AppxPackage -Path "路径\Ubuntu2204.appx"
```

### 步骤4：首次启动和配置
1. 从开始菜单启动Ubuntu
2. 等待初始化完成
3. 创建用户账户

## 方法三：自定义位置安装

### 适用场景
- C盘空间不足
- 需要安装到特定位置
- 系统重装时保留WSL环境

### 步骤1：基础安装
先完成方法一或方法二的基础安装

### 步骤2：导出和迁移
```powershell
# 停止WSL实例
wsl --shutdown

# 导出发行版
wsl --export Ubuntu-22.04 D:\WSL\ubuntu2204.tar

# 注销原发行版
wsl --unregister Ubuntu-22.04

# 导入到自定义位置
wsl --import Ubuntu-22.04 D:\WSL\Ubuntu2204 D:\WSL\ubuntu2204.tar --version 2

# 设置为默认发行版
wsl --setdefault Ubuntu-22.04
```

### 步骤3：恢复默认用户
```bash
# 在WSL中执行
sudo useradd -m -s /bin/bash your_username
sudo usermod -aG sudo your_username
sudo passwd your_username

# 在Windows中设置默认用户
ubuntu2204 config --default-user your_username
```

## 安装后必要配置

### 1. 系统更新
```bash
sudo apt update && sudo apt upgrade -y
```

### 2. 换源（提升速度）
```bash
# 备份原始源
sudo cp /etc/apt/sources.list /etc/apt/sources.list.backup

# 使用清华源
sudo tee /etc/apt/sources.list > /dev/null <<EOF
deb https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ jammy main restricted universe multiverse
deb https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ jammy-updates main restricted universe multiverse
deb https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ jammy-backports main restricted universe multiverse
deb https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ jammy-security main restricted universe multiverse
EOF

sudo apt update
```

### 3. 安装基础工具
```bash
sudo apt install -y \
    curl \
    wget \
    git \
    vim \
    build-essential \
    software-properties-common
```

### 4. 配置Git
```bash
git config --global user.name "Your Name"
git config --global user.email "your.email@example.com"
```

## 基础内存配置

### 创建.wslconfig文件
在Windows用户目录下创建 `C:\Users\YourUsername\.wslconfig`：

```ini
[wsl2]
# 基础配置示例
processors=4
memory=4GB
swap=2GB
localhostForwarding=true
```

**详细的内存优化配置请参考**: [管理和维护 → 性能优化](./management.md#性能优化)

## 验证安装

### 检查WSL状态
```powershell
# 查看已安装发行版
wsl -l -v

# 期望输出示例
#   NAME            STATE           VERSION
# * Ubuntu-22.04    Running         2
```

### 检查系统信息
```bash
# 在WSL中执行
cat /etc/os-release
uname -a
free -h
```

## 常见问题排除

### 问题1：安装失败
**解决方案**：
1. 确保已启用虚拟化功能
2. 检查Windows版本是否支持
3. 以管理员身份运行PowerShell
4. 重启后重试

### 问题2：网络下载慢
**解决方案**：
1. 使用离线安装方法
2. 配置代理或VPN
3. 使用国内镜像源

### 问题3：内存占用过高
**解决方案**：
1. 创建基础.wslconfig配置（见上文）
2. 定期清理缓存
3. 详细优化请参考 [管理和维护](./management.md)

### 问题4：无法启动
**解决方案**：
```powershell
# 重置WSL
wsl --shutdown
wsl --unregister Ubuntu-22.04
# 重新安装

# 检查日志
wsl --status
Get-EventLog -LogName Application -Source "Microsoft-Windows-Subsystem-Linux"
```

## VS Code快速集成

### 基础使用
```bash
# 在WSL中打开项目
cd /path/to/project
code .
```

**完整的VS Code开发环境配置请参考**: [开发环境配置 → VS Code集成](./development.md#vs-code集成)

---