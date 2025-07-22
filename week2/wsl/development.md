# 开发环境配置

## VS Code集成

### 安装Remote-WSL扩展
```powershell
# 在Windows中安装VS Code
# 安装Remote - WSL扩展
```

### WSL中配置VS Code
```bash
# 安装基础工具
sudo apt update
sudo apt install -y curl wget git

# 在WSL中打开VS Code
code .

# 或者从Windows打开WSL目录
code \\wsl$\Ubuntu-22.04\home\username\project
```

### 推荐的VS Code扩展
```bash
# 在WSL中通过命令行安装扩展
code --install-extension ms-python.python
code --install-extension ms-vscode.cpptools
code --install-extension ms-vscode.cmake-tools
code --install-extension bradlc.vscode-tailwindcss
code --install-extension esbenp.prettier-vscode
```

## Python开发环境

### 安装Python和包管理器
```bash
# 更新系统
sudo apt update && sudo apt upgrade -y

# 安装Python 3.11
sudo apt install -y software-properties-common
sudo add-apt-repository ppa:deadsnakes/ppa
sudo apt update
sudo apt install -y python3.11 python3.11-venv python3.11-dev

# 安装pip
curl https://bootstrap.pypa.io/get-pip.py -o get-pip.py
python3.11 get-pip.py
rm get-pip.py

# 设置Python别名
echo 'alias python=python3.11' >> ~/.bashrc
echo 'alias pip=pip3' >> ~/.bashrc
source ~/.bashrc
```

### 虚拟环境管理
```bash
# 安装虚拟环境工具
pip install virtualenv pipenv poetry

# 创建虚拟环境
python -m venv myproject-env
source myproject-env/bin/activate

# 使用pipenv
cd myproject
pipenv install
pipenv shell

# 使用poetry
poetry new myproject
cd myproject
poetry install
poetry shell
```

### Python开发工具链
```bash
# 安装开发工具
pip install --upgrade pip setuptools wheel

# 代码格式化和检查
pip install black flake8 pylint mypy

# 测试框架
pip install pytest pytest-cov

# Jupyter环境
pip install jupyter jupyterlab

# 常用数据科学包
pip install numpy pandas matplotlib seaborn scikit-learn

# Web开发
pip install django flask fastapi uvicorn
```

## Docker集成

### 安装Docker Engine
```bash
# 更新包索引
sudo apt update

# 安装必要包
sudo apt install -y ca-certificates curl gnupg lsb-release

# 添加Docker的官方GPG密钥
sudo mkdir -p /etc/apt/keyrings
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg

# 设置存储库
echo "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null

# 安装Docker Engine
sudo apt update
sudo apt install -y docker-ce docker-ce-cli containerd.io docker-compose-plugin

# 将用户添加到docker组
sudo usermod -aG docker $USER
newgrp docker

# 验证安装
docker --version
docker compose version
```

### Docker配置优化
```bash
# 创建daemon.json配置
sudo mkdir -p /etc/docker
sudo tee /etc/docker/daemon.json > /dev/null <<EOF
{
  "registry-mirrors": [
    "https://hub-mirror.c.163.com",
    "https://mirror.baidubce.com"
  ],
  "log-driver": "json-file",
  "log-opts": {
    "max-size": "10m",
    "max-file": "3"
  },
  "storage-driver": "overlay2"
}
EOF

# 重启Docker服务
sudo systemctl restart docker
```

### 常用Docker命令
```bash
# 镜像管理
docker images                    # 列出镜像
docker pull ubuntu:22.04        # 拉取镜像
docker rmi image_id             # 删除镜像

# 容器管理
docker ps                       # 列出运行中的容器
docker ps -a                    # 列出所有容器
docker run -it ubuntu:22.04 bash   # 运行容器
docker exec -it container_id bash  # 进入容器
docker stop container_id        # 停止容器
docker rm container_id          # 删除容器

# 数据卷
docker volume ls                # 列出数据卷
docker volume create myvolume   # 创建数据卷

# 网络
docker network ls               # 列出网络
docker network create mynet    # 创建网络

# Docker Compose
docker compose up -d            # 后台启动服务
docker compose down             # 停止服务
docker compose logs -f          # 查看日志
```

## 版本控制系统

### Git配置
```bash
# 安装最新版Git
sudo add-apt-repository ppa:git-core/ppa
sudo apt update
sudo apt install -y git

# 基础配置
git config --global user.name "Your Name"
git config --global user.email "your.email@example.com"
git config --global init.defaultBranch main

# 高级配置
git config --global core.editor "code --wait"
git config --global merge.tool "code --wait"
git config --global core.autocrlf input
git config --global pull.rebase false

# SSH密钥配置
ssh-keygen -t ed25519 -C "your.email@example.com"
eval "$(ssh-agent -s)"
ssh-add ~/.ssh/id_ed25519

# 查看公钥
cat ~/.ssh/id_ed25519.pub
```

### Git辅助工具
```bash
# 安装Git辅助工具
sudo apt install -y git-flow git-lfs

# 安装hub（GitHub CLI）
sudo snap install gh

# gh认证
gh auth login

# 常用Git别名
git config --global alias.st status
git config --global alias.co checkout
git config --global alias.br branch
git config --global alias.ci commit
git config --global alias.lg "log --oneline --graph --all"
```

## Linux系统基础

### 文件系统操作
```bash
# 目录导航
pwd                          # 显示当前目录
ls -la                       # 详细列出文件
cd /path/to/directory        # 切换目录
cd ~                        # 回到主目录
cd -                        # 回到上一个目录

# 文件操作
touch filename              # 创建空文件
mkdir -p dir1/dir2         # 递归创建目录
cp source destination      # 复制文件
mv source destination      # 移动/重命名文件
rm -rf directory          # 删除目录及内容
ln -s target linkname     # 创建软链接

# 文件查看
cat filename              # 查看文件内容
less filename            # 分页查看文件
head -n 10 filename      # 查看前10行
tail -f filename         # 实时查看文件末尾
wc -l filename          # 统计行数
```

### 文件权限管理
```bash
# 权限查看
ls -l filename

# 权限修改
chmod 755 filename       # 设置权限为rwxr-xr-x
chmod +x filename        # 添加执行权限
chmod -w filename        # 移除写权限

# 所有者修改
sudo chown user:group filename
sudo chown -R user:group directory

# 特殊权限
chmod +s filename        # 设置SUID
chmod +t directory       # 设置粘滞位
```

### 进程管理
```bash
# 进程查看
ps aux                   # 查看所有进程
ps aux | grep process    # 查找特定进程
top                      # 实时进程监控
htop                     # 增强版top（需安装）
pgrep process_name       # 根据名称查找进程ID

# 进程控制
kill PID                 # 终止进程
kill -9 PID             # 强制终止进程
killall process_name    # 终止所有同名进程
nohup command &         # 后台运行命令
jobs                    # 查看后台任务
fg %1                   # 将后台任务调到前台
```

### 网络工具
```bash
# 网络连接
ping hostname           # 测试网络连通性
wget URL               # 下载文件
curl -O URL            # 下载文件
curl -X POST URL       # 发送POST请求

# 网络状态
netstat -tulpn         # 查看网络连接
ss -tulpn             # 现代版netstat
lsof -i :80           # 查看端口占用

# 网络配置
ip addr show          # 查看IP地址
ip route show         # 查看路由表
systemctl status networking  # 网络服务状态
```

### 系统信息
```bash
# 系统信息
uname -a               # 系统信息
lsb_release -a         # 发行版信息
whoami                 # 当前用户
id                     # 用户ID和组信息
uptime                 # 系统运行时间
date                   # 当前日期时间

# 硬件信息
lscpu                  # CPU信息
free -h                # 内存使用情况
df -h                  # 磁盘使用情况
du -sh directory       # 目录大小
lsblk                  # 块设备信息
lspci                  # PCI设备
lsusb                  # USB设备
```

### 包管理
```bash
# APT包管理（Ubuntu/Debian）
sudo apt update                    # 更新包列表
sudo apt upgrade                   # 升级所有包
sudo apt install package_name     # 安装包
sudo apt remove package_name      # 卸载包
sudo apt autoremove              # 清理不需要的包
sudo apt search keyword          # 搜索包
apt list --installed             # 列出已安装包

# Snap包管理
sudo snap install package_name   # 安装snap包
sudo snap list                   # 列出已安装snap包
sudo snap refresh                # 更新snap包
sudo snap remove package_name    # 卸载snap包
```

### 文本处理
```bash
# 文本搜索
grep pattern filename            # 在文件中搜索模式
grep -r pattern directory       # 递归搜索目录
grep -v pattern filename        # 反向搜索（不包含模式）
grep -n pattern filename        # 显示行号

# 文本替换
sed 's/old/new/g' filename      # 替换文件中的文本
sed -i 's/old/new/g' filename   # 直接修改文件

# 文本处理
awk '{print $1}' filename       # 打印第一列
sort filename                   # 排序文件
uniq filename                   # 去重
cut -d',' -f1 filename         # 按分隔符切割
```

### 压缩和解压
```bash
# tar格式
tar -czf archive.tar.gz files   # 创建gzip压缩包
tar -xzf archive.tar.gz         # 解压gzip压缩包
tar -cjf archive.tar.bz2 files  # 创建bzip2压缩包
tar -xjf archive.tar.bz2        # 解压bzip2压缩包

# zip格式
zip -r archive.zip files        # 创建zip压缩包
unzip archive.zip               # 解压zip文件

# 其他格式
gzip filename                   # gzip压缩
gunzip filename.gz              # gzip解压
```

### 环境变量和配置
```bash
# 环境变量
echo $PATH                      # 查看PATH变量
export VAR=value               # 设置环境变量
unset VAR                      # 删除环境变量
env                            # 查看所有环境变量

# 配置文件
~/.bashrc                      # bash配置文件
~/.profile                     # 用户配置文件
/etc/environment              # 系统环境变量

# 重载配置
source ~/.bashrc              # 重载bashrc
source ~/.profile             # 重载profile
```

### Shell脚本基础
```bash
#!/bin/bash
# 基础脚本结构

# 变量
NAME="World"
echo "Hello, $NAME!"

# 条件语句
if [ -f "/etc/passwd" ]; then
    echo "File exists"
fi

# 循环
for i in {1..5}; do
    echo "Number: $i"
done

# 函数
function greet() {
    echo "Hello, $1!"
}
greet "User"
```

### 系统服务管理
```bash
# systemctl命令
sudo systemctl status service_name    # 查看服务状态
sudo systemctl start service_name     # 启动服务
sudo systemctl stop service_name      # 停止服务
sudo systemctl restart service_name   # 重启服务
sudo systemctl enable service_name    # 开机自启
sudo systemctl disable service_name   # 禁用自启

# 查看日志
journalctl -u service_name           # 查看服务日志
journalctl -f                        # 实时查看日志
```

### 安全和权限
```bash
# 用户管理
sudo adduser username              # 添加用户
sudo usermod -aG sudo username     # 添加到sudo组
sudo passwd username               # 修改密码
su - username                      # 切换用户

# 防火墙
sudo ufw status                    # 查看防火墙状态
sudo ufw enable                    # 启用防火墙
sudo ufw allow 22                  # 允许SSH端口
sudo ufw deny 80                   # 拒绝HTTP端口

# 文件加密
gpg --gen-key                      # 生成GPG密钥
gpg --encrypt --recipient user file # 加密文件
gpg --decrypt file.gpg             # 解密文件
```

---