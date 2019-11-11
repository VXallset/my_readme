# Accelerating Lightweight Openpose using OpenVINO
We can convert tensorflow/onnx/caffe/mxnet model files to  an Intermediate Representation (IR) of the network, which is a pair of files that describe the whole model: .xml and .bin file at first. Then optimizes inference execution for target hardware and delivers inference solution with reduced footprint on embedded inference platforms.


##  1. Install OpenVINO Environment

### 1.1 Overview
OpenVINO can be run on different hardwares, including 6th-10th Generation Intel® Core™ processors,  Intel® Movidius™ Neural Compute Stick (1 and 2), and  Intel® Vision Accelerator Design with Intel® Movidius™ VPUs etc. Please refer to the [OpenVINO Toolkit mannual](https://docs.openvinotoolkit.org/latest/index.html) to install the OpenVINO on different machines. Here I take my PC for example. 

- Hardware: Intel Core i7-8700 processer @3.2GHz

- Operating System: Ubuntu 18.04


### 1.2 Installation Steps

- Download the Intel® Distribution of OpenVINO™ toolkit package file from  [Intel® Distribution of OpenVINO™ toolkit for Linux*](https://software.intel.com/en-us/openvino-toolkit/choose-download?elq_cid=5925583&erpm_id=8976297)

- cd to the downloads directory and unpack it (Please modify the version number, my version is 2019.3.376).

		cd ~/Downloads/
		tar -xvzf l_openvino_toolkit_p_2019.3.376.tgz
	    
- Go to the l_openvino_toolkit_p_2019.3.376 directory.
	
		cd l_openvino_toolkit_p_2019.3.376

- Install the toolkit using GUI Wizard and follow the instruction. NOTE: The Intel® Media SDK component is always installed in the /opt/intel/mediasdk directory regardless of the OpenVINO installation path chosen.

		sudo ./install_GUI.sh

- Install external software dependencies.

		cd /opt/intel/openvino/install_dependencies


- Set the environment variables. When you finish this step, you can open a new terminal and see '[setupvars.sh] OpenVINO environment initialized'.

		source /opt/intel/openvino/bin/setupvars.sh
		echo 'source /opt/intel/openvino/bin/setupvars.sh'>>/etc/.bashrc
		
if the download speed is too slow, please add other source to your source list, such as:

		echo 'deb http://mirrors.aliyun.com/ubuntu/ bionic main restricted universe multiverse
		deb http://mirrors.aliyun.com/ubuntu/ bionic-security main restricted universe multiverse
		deb http://mirrors.aliyun.com/ubuntu/ bionic-updates main restricted universe multiverse
		deb http://mirrors.aliyun.com/ubuntu/ bionic-proposed main restricted universe multiverse
		deb http://mirrors.aliyun.com/ubuntu/ bionic-backports main restricted universe multiverse
		deb-src http://mirrors.aliyun.com/ubuntu/ bionic main restricted universe multiverse
		deb-src http://mirrors.aliyun.com/ubuntu/ bionic-security main restricted universe multiverse
		deb-src http://mirrors.aliyun.com/ubuntu/ bionic-updates main restricted universe multiverse
		deb-src http://mirrors.aliyun.com/ubuntu/ bionic-proposed main restricted universe multiverse
		deb-src http://mirrors.aliyun.com/ubuntu/ bionic-backports main restricted universe multiverse

		deb https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ bionic main restricted universe multiverse
		deb-src https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ bionic main restricted universe multiverse
		deb https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ bionic-updates main restricted universe multiverse
		deb-src https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ bionic-updates main restricted universe multiverse
		deb https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ bionic-backports main restricted universe multiverse
		deb-src https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ bionic-backports main restricted universe multiverse
		deb https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ bionic-security main restricted universe multiverse
		deb-src https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ bionic-security main restricted universe multiverse
		deb https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ bionic-proposed main restricted universe multiverse
		deb-src https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ bionic-proposed main restricted universe multiverse
		
		###### Ubuntu Main Repos
		deb http://mirrors.ustc.edu.cn/ubuntu-old-releases/ubuntu/ zesty main universe

		###### Ubuntu Update Repos
		deb http://mirrors.ustc.edu.cn/ubuntu-old-releases/ubuntu/ zesty-updates main universe'>>/etc/apt/source.list

- Model optimizer configuration steps. We need to convert onnx file, so we only run the install_prerequisites_onnx.sh file.

		cd /opt/intel/openvino/deployment_tools/model_optimizer/install_prerequisites
		sudo ./install_prerequisites_onnx.sh

- Verification 

		cd /opt/intel/openvino/deployment_tools/demo
		./demo_squeezenet_download_convert_run.sh

## 2. Accelerate Lightweight Openpose Using OpenVINO

### Run the Lightweight Openpose
Please go to [Githhub](https://github.com/Daniil-Osokin/lightweight-human-pose-estimation.pytorch) to download the Lightweight Openpose source code implemented with Pytorch. You may also need to download th COCO API/dataset to do the testing or training. 

 Now we have the converted model and the OpenVINO, next we need to compile the source code (C++ version) to run the network. The source code of human_pose_estimation_demo is provided by OpenVINO Toolkit, which can be found in the _/opt/intel/openvino/deployment_tools/open_model_zoo/demos_ folder. What we need to do is compiling and running it!
 
To Compile the C++ version of the source code, please refer to the _/opt/intel/openvino/deployment_tools/demo/demo_squeezenet_download_convert_run.sh_. For simplicity, I provided my test_LightweightOpenPose.sh. You can use this file to compile and run the converted Lightweight Openose network. Please Note that you may need to modify the paths to the .xml and .mp4 files.
	
