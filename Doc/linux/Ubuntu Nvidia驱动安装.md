# Ubuntu Nvidia驱动安装

## 查看合适的显卡驱动

```
sudo ubuntu-drivers devices
```

## 安装驱动

### 自动安装合适版本

```
sudo ubuntu-drivers autoinstall
```

### 指定安装版本

```
sudo apt install nvidia-418
```

## 安装nvidia-cuda-tookit

```
sudo apt install nvidia-cuda-tookit
```

## 安装nvidia-container-toolkit

### 在线安装

#### 配置仓库

```
curl -fsSL https://nvidia.github.io/libnvidia-container/gpgkey | sudo gpg --dearmor -o /usr/share/keyrings/nvidia-container-toolkit-keyring.gpg \
  && curl -s -L https://nvidia.github.io/libnvidia-container/stable/deb/nvidia-container-toolkit.list | \
    sed 's#deb https://#deb [signed-by=/usr/share/keyrings/nvidia-container-toolkit-keyring.gpg] https://#g' | \
    sudo tee /etc/apt/sources.list.d/nvidia-container-toolkit.list
```

#### 更新仓库

```
sudo apt-get update
```

#### 安装nct

```
sudo apt-get install -y nvidia-container-toolkit
```

### 离线安装

```
https://mirror.cs.uchicago.edu/nvidia-docker/libnvidia-container/stable/ubuntu18.04/amd64/
```

```
#需要先安装container1
sudo dpkg -i ./libnvidia-container1_1.9.0-1_amd64.deb
#再安装libnvidia-container-tools
sudo dpkg -i ./libnvidia-container-tools_1.9.0-1_amd64.deb
#最后安装nvidia-container-toolkit
sudo dpkg -i ./nvidia-container-toolkit_base_1.9.0-1_amd64.deb
sudo dpkg -i ./nvidia-container-toolkit_1.9.0-1_amd64.deb
```

### 配置docker

```
sudo nvidia-ctk runtime configure --runtime=docker
```

```
vim /etc/docker/daemon.json
{
    "runtimes": {
        "nvidia": {
            "args": [],
            "path": "nvidia-container-runtime"
        }
    }
}
```



### 重启docker daemon

```
sudo systemctl restart docker
```

## 重启操作系统

```
reboot
```

## 查看信息

```
nvidia-smi

nvcc -V
```



查看驱动版本

```
modinfo nvidia | grep version
```

查看显卡型号

```
lspci | grep -i nvidia
```

