# WSL管理和维护

## 发行版管理

### 查看和管理发行版
```powershell
# 列出所有已安装的发行版
wsl -l -v

# 启动特定发行版
wsl -d Ubuntu-22.04

# 停止特定发行版
wsl --terminate Ubuntu-22.04

# 停止所有WSL实例
wsl --shutdown

# 设置默认发行版
wsl --setdefault Ubuntu-22.04
```

### 发行版重命名

#### 方法一：导出/导入重命名
```powershell
# 1. 停止发行版
wsl --terminate Ubuntu

# 2. 导出发行版
wsl --export Ubuntu D:\WSL\Backup\ubuntu-backup.tar

# 3. 注销原发行版
wsl --unregister Ubuntu

# 4. 导入为新名称
wsl --import Ubuntu-22.04 D:\WSL\Ubuntu2204 D:\WSL\Backup\ubuntu-backup.tar --version 2

# 5. 设置默认用户
ubuntu2204 config --default-user your_username
```

#### 方法二：修改注册表
```cmd
# 打开注册表编辑器
regedit

# 导航到
HKEY_CURRENT_USER\SOFTWARE\Microsoft\Windows\CurrentVersion\Lxss

# 修改 DistributionName 键值
# 重启系统生效
```

### 多版本管理

#### 规划命名策略
```
Ubuntu-18.04    # Ubuntu 18.04 LTS
Ubuntu-20.04    # Ubuntu 20.04 LTS  
Ubuntu-22.04    # Ubuntu 22.04 LTS
Ubuntu-Dev      # 开发环境
Ubuntu-Prod     # 生产测试环境
```

#### 并行安装多版本
```powershell
# 安装Ubuntu 20.04
wsl --import Ubuntu-20.04 D:\WSL\Ubuntu2004 ubuntu2004.tar --version 2

# 安装Ubuntu 22.04
wsl --import Ubuntu-22.04 D:\WSL\Ubuntu2204 ubuntu2204.tar --version 2

# 切换使用
wsl -d Ubuntu-20.04
wsl -d Ubuntu-22.04
```

## 发行版迁移

### 完整迁移流程
```powershell
# 1. 创建目标目录
mkdir D:\WSL\Ubuntu2204
mkdir D:\WSL\Backup

# 2. 停止WSL
wsl --shutdown

# 3. 导出发行版
wsl --export Ubuntu-22.04 D:\WSL\Backup\ubuntu2204-export.tar

# 4. 验证导出文件
dir D:\WSL\Backup\

# 5. 注销原发行版
wsl --unregister Ubuntu-22.04

# 6. 导入到新位置
wsl --import Ubuntu-22.04 D:\WSL\Ubuntu2204 D:\WSL\Backup\ubuntu2204-export.tar --version 2

# 7. 验证迁移
wsl -l -v
```

### 批量迁移脚本
```powershell
# migrate-wsl.ps1
param(
    [string]$DistroName,
    [string]$TargetPath,
    [string]$BackupPath = "D:\WSL\Backup"
)

Write-Host "开始迁移 $DistroName 到 $TargetPath"

# 停止WSL
wsl --shutdown

# 创建目录
New-Item -ItemType Directory -Path $TargetPath -Force
New-Item -ItemType Directory -Path $BackupPath -Force

# 导出
$exportFile = "$BackupPath\$DistroName-$(Get-Date -Format 'yyyyMMdd').tar"
wsl --export $DistroName $exportFile

if ($LASTEXITCODE -eq 0) {
    # 注销原发行版
    wsl --unregister $DistroName
    
    # 导入到新位置
    wsl --import $DistroName $TargetPath $exportFile --version 2
    
    Write-Host "迁移完成！" -ForegroundColor Green
} else {
    Write-Host "迁移失败！" -ForegroundColor Red
}
```

## 性能优化

### 内存控制配置
在 `C:\Users\YourUsername\.wslconfig` 中配置：

```ini
[wsl2]
# 处理器核心数
processors=4

# 内存限制
memory=4GB

# 交换文件大小
swap=2GB

# 交换文件位置
swapFile=D:\\WSL\\swap.vhdx

# 启用localhost转发
localhostForwarding=true

# 内核参数
kernelCommandLine = vsyscall=emulate

# 限制虚拟硬盘大小
vmIdleTimeout=60000
```

### 不同内存配置建议
```ini
# 8GB内存系统
[wsl2]
processors=4
memory=2GB
swap=1GB
localhostForwarding=true

# 16GB内存系统
[wsl2]
processors=6
memory=6GB
swap=2GB
localhostForwarding=true

# 32GB内存系统
[wsl2]
processors=8
memory=12GB
swap=4GB
localhostForwarding=true
```

### 性能监控
```bash
# WSL内部监控
free -h                    # 内存使用
htop                       # 进程监控
df -h                      # 磁盘使用
iostat                     # IO统计
```

```powershell
# Windows端监控
Get-Process "*wsl*"        # WSL进程
wsl --status              # WSL状态
```

## 清理和维护

### WSL内部清理
```bash
# 清理包缓存
sudo apt autoremove -y
sudo apt autoclean
sudo apt clean

# 清理日志
sudo journalctl --vacuum-time=7d
sudo journalctl --vacuum-size=100M

# 清理临时文件
sudo rm -rf /tmp/*
sudo rm -rf /var/tmp/*
rm -rf ~/.cache/*

# 清理snap包缓存
sudo snap list --all | awk '/disabled/{print $1, $3}' | while read snapname revision; do sudo snap remove "$snapname" --revision="$revision"; done
```

### 虚拟硬盘压缩
```powershell
# 停止WSL
wsl --shutdown

# 找到vhdx文件位置
# 通常在：%LOCALAPPDATA%\Packages\CanonicalGroupLimited.Ubuntu*\LocalState\

# 使用diskpart压缩
diskpart
# 在diskpart中执行：
# select vdisk file="完整路径\ext4.vhdx"
# attach vdisk readonly
# compact vdisk
# detach vdisk
# exit
```

### 自动清理脚本
```bash
#!/bin/bash
# cleanup-wsl.sh

echo "开始清理WSL系统..."

# 清理包管理器缓存
sudo apt autoremove -y
sudo apt autoclean

# 清理日志
sudo journalctl --vacuum-time=3d

# 清理用户缓存
rm -rf ~/.cache/*
rm -rf ~/.npm/_cacache
rm -rf ~/.yarn/cache

# 清理临时文件
sudo rm -rf /tmp/*
sudo rm -rf /var/tmp/*

# 显示清理后的空间
echo "清理完成！当前磁盘使用情况："
df -h /
```

## 网络配置

### 端口转发
```powershell
# 转发WSL端口到Windows
netsh interface portproxy add v4tov4 listenport=8080 listenaddress=0.0.0.0 connectport=8080 connectaddress=$(wsl hostname -I)

# 查看端口转发规则
netsh interface portproxy show all

# 删除端口转发
netsh interface portproxy delete v4tov4 listenport=8080 listenaddress=0.0.0.0
```

### 网络故障排除
```bash
# WSL网络诊断
ip addr show                # 查看网络接口
ip route show              # 查看路由表
systemd-resolve --status   # DNS状态
ping google.com            # 测试网络连通性
```

```powershell
# Windows网络重置
wsl --shutdown
netsh winsock reset
netsh int ip reset
ipconfig /flushdns
```

## 故障排除

### 常见问题解决

#### WSL启动失败
```powershell
# 检查WSL状态
wsl --status

# 检查虚拟化支持
systeminfo | findstr /i "hyper-v"

# 重置网络
netsh winsock reset
netsh int ip reset
```

#### 内存占用过高
```powershell
# 限制内存使用
# 编辑 .wslconfig 文件

# 临时释放内存
wsl --shutdown
```

#### 文件系统错误
```bash
# 检查文件系统
sudo fsck /dev/sdc

# 修复文件系统
sudo fsck -f /dev/sdc
```