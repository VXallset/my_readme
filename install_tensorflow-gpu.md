'''
@contact: vxallset@outlook.com
@Last modified: Nov 11, 2019
'''

# Tensorflow-gpu-installation
Ubuntu18.04环境下安装GPU版本的TensorFlow 1.13/1.14/2.0. Install TensorFlow 1.13/1.14/2.0 on Ubuntu 18.04.

__请不要使用TensorFlow官方安装指南安装TensorFlow，因为里面（好像）有BUG...__ 我用[官网方法](https://tensorflow.google.cn/install/gpu)安装了好多次都没有成功，如果有和我类似的情况，可以参考以下内容。

### 1. 环境要求
- 可以安装cuda的Nvidia公司出品的显卡,如NVIDIA GTX1060, NVIDIA RTX2080, Nvidia Titan V等。
- Ubuntu 18.04

### 2. 安装目标

- NVIDIA 显卡驱动 410.48
- CUDA 10.0 (410.48)
- cudnn 7.6.2.24
- TensorFlow 1.14 (或其他版本，如1.13, 2.0) 

### 3. 清理系统环境

由于旧显卡驱动可能不支持cuda，或者和下载的cuda兼容性不一致，强烈建议先卸载显卡驱动。首先找到之前安装驱动的安装包，使用如下命令进行卸载

    sudo ./NVIDIA-Linux-x86_64-390.59.run --uninstall

若找不到安装驱动的安装包也没有关系，使用如下命令清除其他安装过的cuda/cublas和其他NVIDIA产品

    sudo apt-get purge nvidia-cuda*
    sudo apt-get purge nvidia-*
    sudo apt-get --purge remove "*cublas*" "cuda*"
    

旧的显卡驱动卸载后，重启电脑，屏幕分辨率会改变，这是正常情况。此时输入

    nvidia-smi

会提示Nvidia-smi还没有安装，此时我们开始进行下一步。

### 4. 安装CUDA10.0 及Nvidia显卡驱动
进入CUDA 10.0的[下载页面](https://developer.nvidia.com/cuda-10.0-download-archive?target_os=Linux&target_arch=x86_64&target_distro=Ubuntu&target_version=1804&target_type=runfilelocal)进行下载。注意，__目前TensorFlow只支持到10.0__，若是下载CUDA 10.1，则TensorFlow无法识别和使用。


__请 不要 下载TF官网建议的NVIDIA driver 418__,我个人认为此版本不适用于CUDA10.0. 适用于CUDA 10.0的显卡驱动版本是 __NVIDIA driver 410__ 官网给的安装显卡驱动的方式是（这可能是错误的，请不要执行）：

> #Install NVIDIA driver

> $sudo apt-get install --no-install-recommends nvidia-driver-418

_正确的做法：_
由于在运行CUDA10.0的.run文件时，会询问用户是否安装显卡驱动，我们只需要选择_是_就可以安装匹配CUDA 10.0 的显卡驱动，而且安装完CUDA后重启电脑，屏幕分辨率就会恢复。
在下载好CUDA 10.0后，输入以下命令：

    sudo chmod +x  cuda_10.0.130_410.48_linux.run
    sudo sh cuda_10.0.130_410.48_linux.run
然后一直按回车阅读licence并接收，然后后续的选项全部选择yes.等待安装过程完成之后，在命令行中输入：

    sudo gedit /etc/profile

在文件结尾，把cuda路径加到系统中：

    export PATH=/usr/local/cuda-10.0/bin:$PATH
    export LD_LIBRARY_PATH=/usr/local/cuda-10.0/lib64$LD_LIBRARY_PATH

然后重启电脑，在命令行中输入

    nvidia-smi

会出现显卡信息。

### 5. 安装cuDNN并将文件添加到系统中

进入[cuDNN下载页](https://developer.nvidia.com/rdp/cudnn-archive)下载cuDNN 7.6.2.24，并解压。然后在命令行中输入以下命令：

    sudo cp cuda/include/cudnn.h /usr/local/cuda/include
    sudo cp cuda/lib64/libcudnn* /usr/local/cuda/lib64
    sudo chmod a+r /usr/local/cuda/include/cudnn.h /usr/local/cuda/lib64/libcudnn*

### 6. 安装TensorFlow
这一步是最简单的，只需要选择需要安装的TensorFlow版本即可。命令如下：

    sudo pip3 install tensorflow-gpu #(如需其他版本的，使用 $ sudo pip3 install tensorflow-gpu==版本号)


