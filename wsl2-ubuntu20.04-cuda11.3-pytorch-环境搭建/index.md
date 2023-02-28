# WSL2+Ubuntu20.04+CUDA11.3+Pytorch环境搭建


## 一. 安装WSL2

​	具体安装的步骤可以直接看 [微软官方的安装文档](https://learn.microsoft.com/zh-cn/windows/wsl/install) ，官方文档写的很详细，满足系统要求，安装步骤一般

都能成功安装。

## 二. 安装显卡驱动——在windows上面

​	一般安装了英伟达显卡的电脑，都会安装适配的显卡驱动，一般能都满足要求，不放心的用Nvidia 

Geforce 更新一下即可，或者在[英伟达官网](https://www.nvidia.cn/Download/index.aspx?lang=cn)安装显卡驱动

​	{{<figure src = "/images/nvidia-driver-download.png" title = "nvidia-driver-download">}}

​	这里按照你自己的显卡型号和系统类型选择即可，下载类型选择Game Ready即可。

​	现在完成之后，安装默认指引安装。

## 三. 安装miniconda——创建虚拟环境

#### 1. 在WSL2中的Ubuntu20.04中使用命令行安装miniconda

```bash
wget https://repo.anaconda.com/miniconda/Miniconda3-latest-Linux-x86_64.sh
bash Miniconda3-latest-Linux-x86_64.sh
```

#### 2. 或者在官网直接下载安装包

​	打开miniconda官网，选择相应的安装包

​	{{<figure src = "/images/miniconda-linux-download.png" title = "miniconda-linux-download">}}

将安装包移动到ubuntu中使用下面的命令安装

```bash
bash Miniconda3-latest-Linux-x86_64.sh
```

#### 3. 安装的最后一步需要init，选择yes

​	安装之后，关闭当前shell，才可以使用conda命令

​	命令行前会出现base，每次系统启动都会自动激活conda的基础环境，想要退回到系统自带的环境，可以

有两种方法：

> ​	方法一：每次都运行 conda deactivate
>
> ​	方法二：运行 conda config --set auto_activate_base false 

## 四. 安装CUDA11.3

#### 1. 在Ubuntu中查看当前显卡驱动最高支持的CUDA版本

​	使用下面的命令查看显卡信息

```bash
nvidia-smi
```
​	{{<figure src = "/images/ubuntu-nvidia-smi.png" title = "ubuntu-nvidia-smi">}}

​	**注意**:这里的cuda version不是指只能安装cuda12.0，而是指最高支持12.0，一般都可以向下兼容，但是

有的显卡对cuda的最低版本有要求，比如我使用的这款GTX-3060，最低要求cuda11.1，我这里就以

cuda11.3做演示。

#### 2. 更新软件列表，同时安装必备的工具包

```bash
sudo apt-get update
sudo apt-get install build-essential
```

#### 3. 安装CUDA toolkit 

​	进入[官方CUDA13安装页面](https://developer.nvidia.com/cuda-11.3.0-download-archive)，依次选择Linux，x86_64，WSL-Ubuntu，2.0，runfile(local) 

​	{{<figure src = "/images/nvidia-cuda-local.png" title = "nvidia-cuda-local">}}

​	这里选择runfile(local)可以避免很多bug，执行下面的命令进行安装

```bash
wget https://developer.download.nvidia.com/compute/cuda/11.3.0/local_installers/cuda_11.3.0_465.19.01_linux.run
sudo sh cuda_11.3.0_465.19.01_linux.run
```

​	**注意**：这里在安装界面全部按照默认选项即可

#### 4. 将CUDA路径写入环境

​	编辑进入~/.bashrc

```bash
vim ~/.bashrc
```

​	将下面内容插入文件的最后

```bash
export CUDA_HOME=/usr/local/cuda 
export PATH=$PATH:$CUDA_HOME/bin 
export LD_LIBRARY_PATH=/usr/local/cuda11.3/lib64${LD_LIBRARY_PATH:+:${LD_LIBRARY_PATH}}
```

#### 5. 验证cuda是否安装成功

```bash
nvcc -V
```

​	如果一切正常，会显示下面的内容

{{<figure src = "/images/nvcc-V.png" title = "nvcc-V">}}

**注意**：我这种方式不需要安装cudnn，可以直接安装Pytorch

#### 6. 运行cuda自带的samples

```bash
cd /usr/local/cuda/samples/4_Finance/BlackScholes
sudo make
./BlackScholes 
```

​	正常会显示，如下：

​	{{<figure src  = "/images/cuda-sample-show.png" title = "cuda-sample-show">}}

## 五. 安装Pytorch 

#### 1. 建立虚拟环境

```bash
conda create -n pytorch python=3.8
conda activate pytorch
```

#### 2. 安装pytorch

```bash
pip install torch==1.12.1+cu113 torchvision==0.13.1+cu113 torchaudio==0.12.1 --extra-index-url https://download.pytorch.org/whl/cu113
```

​	**注意**：这里尽量使用pip安装，conda安装总是下载cpu版本，我们想要的是gpu版本的。

#### 3. 验证pytorch的gpu版本是否安装成功

​	输入 python，进入python命令行模式

```bash
python
```

​	输入下面的命令：

```python
import torch
torch.cuda.is_available()
```

​	返回true，则说明安装正常



---

写在最后：

为了这玩意儿，折腾了有小两天，然而并无卵用，工具的学习是为了内容，以后还是别轻易折腾了

