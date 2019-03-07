# tf_cpp_jetson_xavier
Build Tensorflow C++ library on Nvidia Jetson Xavier
$ cd ~/src/tensorflow-1.12.0
$ ./configure
WARNING: ignoring http_proxy in environment.
WARNING: --batch mode is deprecated. Please instead explicitly shut down your Bazel server using the command "bazel shutdown".
You have bazel 0.15.2- (@non-git) installed.
Please specify the location of python. [Default is /usr/bin/python]: /usr/bin/python3
   
   
Found possible Python library paths:
  /usr/local/lib/python3.5/dist-packages
  /usr/lib/python3/dist-packages
Please input the desired Python library path to use.  Default is [/usr/local/lib/python3.5/dist-packages]
   
Do you wish to build TensorFlow with jemalloc as malloc support? [Y/n]: 
jemalloc as malloc support will be enabled for TensorFlow.
   
Do you wish to build TensorFlow with Google Cloud Platform support? [Y/n]: n
No Google Cloud Platform support will be enabled for TensorFlow.
   
Do you wish to build TensorFlow with Hadoop File System support? [Y/n]: n
No Hadoop File System support will be enabled for TensorFlow.
   
Do you wish to build TensorFlow with Amazon S3 File System support? [Y/n]: n
No Amazon S3 File System support will be enabled for TensorFlow.
   
Do you wish to build TensorFlow with Apache Kafka Platform support? [Y/n]: n
No Apache Kafka Platform support will be enabled for TensorFlow.
   
Do you wish to build TensorFlow with XLA JIT support? [y/N]: 
No XLA JIT support will be enabled for TensorFlow.
   
Do you wish to build TensorFlow with GDR support? [y/N]: 
No GDR support will be enabled for TensorFlow.
   
Do you wish to build TensorFlow with VERBS support? [y/N]: 
No VERBS support will be enabled for TensorFlow.
   
Do you wish to build TensorFlow with OpenCL SYCL support? [y/N]: 
No OpenCL SYCL support will be enabled for TensorFlow.
   
Do you wish to build TensorFlow with CUDA support? [y/N]: y
CUDA support will be enabled for TensorFlow.
   
Please specify the CUDA SDK version you want to use, e.g. 7.0. [Leave empty to default to CUDA 9.0]: 
   
   
Please specify the location where CUDA 9.0 toolkit is installed. Refer to README.md for more details. [Default is /usr/local/cuda]: 
   
   
Please specify the cuDNN version you want to use. [Leave empty to default to cuDNN 7.0]: 7.3.1
   
Please specify the location where cuDNN 7 library is installed. Refer to README.md for more details. [Default is /usr/local/cuda]:/usr/lib/aarch64-linux-gnu
   
   
Do you wish to build TensorFlow with TensorRT support? [y/N]: y
TensorRT support will be enabled for TensorFlow.
   
Please specify the location where TensorRT is installed. [Default is /usr/lib/aarch64-linux-gnu]:
   
   
Please specify the NCCL version you want to use. [Leave empty to default to NCCL 1.3]: 
   
   
Please specify a list of comma-separated Cuda compute capabilities you want to build with.
You can find the compute capability of your device at: https://developer.nvidia.com/cuda-gpus.
Please note that each additional compute capability significantly increases your build time and binary size. [Default is: 3.5,5.2]6.2
   
   
Do you want to use clang as CUDA compiler? [y/N]: 
nvcc will be used as CUDA compiler.
   
Please specify which gcc should be used by nvcc as the host compiler. [Default is /usr/bin/gcc]: 
   
   
Do you wish to build TensorFlow with MPI support? [y/N]: 
No MPI support will be enabled for TensorFlow.
   
Please specify optimization flags to use during compilation when bazel option "--config=opt" is specified [Default is -march=native]: 
   
   
Would you like to interactively configure ./WORKSPACE for Android builds? [y/N]: 
Not configuring the WORKSPACE for Android builds.
   
Preconfigured Bazel build configs. You can use any of the below by adding "--config=<>" to your build command. See tools/bazel.rc for more details.
	--config=mkl         	# Build with MKL support.
	--config=monolithic  	# Config for mostly static monolithic build.
Configuration finished
#download patch
git apply patch
bazel build --config=opt --config=cuda --local_resources 8192,2.0,1.0  //tensorflow:libtensorflow_cc.so

### 方式一

#### 安装Bazel

1. 安装JDK8：`sudo apt install openjdk-8-jdk`

2. 添加Bazel分发源地址：

   ```
   echo "deb [arch=amd64] http://storage.googleapis.com/bazel-apt stable jdk1.8" | sudo tee /etc/apt/sources.list.d/bazel.list
   curl https://bazel.build/bazel-release.pub.gpg | sudo apt-key add -
   ```

3. 安装Bazel：`sudo apt update && sudo apt install bazel`

4. 更新Bazel：`sudo apt upgrade bazel`

build bazel
#### 配置第三方依赖

1. 从GitHub上下载TensorFlow源码：`git clone --recursive https://github.com/tensorflow/tensorflow`
2. 进入目录：`cd tensorflow/contrib/makefile`
3. 执行文件：`./build_all_linux.sh`

#### 编译TensorFlow

1. 进入TensorFlow根目录：`cd tensorflow`
2. 配置TensorFlow：`./configure`
3. 使用Bazel编译C++ API的库：`bazel build //tensorflow:libtensorflow_cc.so`

#### 安装

1. 建立文件夹

```
sudo mkdir /usr/local/tensorflow
```

2. 拷贝头文件

```
sudo mkdir /usr/local/tensorflow/include
sudo cp -r tensorflow/contrib/makefile/downloads/eigen/Eigen /usr/local/tensorflow/include/
sudo cp -r tensorflow/contrib/makefile/downloads/eigen/unsupported /usr/local/tensorflow/include/
sudo cp -r tensorflow/contrib/makefile/downloads/absl/absl /usr/local/tensorflow/include/

# sudo cp -r tensorflow/contrib/makefile/gen/protobuf/include/google /usr/local/tensorflow/include/
sudo cp tensorflow/contrib/makefile/downloads/nsync/public/* /usr/local/tensorflow/include/
sudo cp -r bazel-genfiles/tensorflow /usr/local/tensorflow/include/
sudo cp -r tensorflow/cc /usr/local/tensorflow/include/tensorflow
sudo cp -r tensorflow/core /usr/local/tensorflow/include/tensorflow
sudo mkdir /usr/local/tensorflow/include/third_party
sudo cp -r third_party/eigen3 /usr/local/tensorflow/include/third_party/
```

3. 拷贝库文件

```
sudo mkdir /usr/local/tensorflow/lib
sudo cp bazel-bin/tensorflow/libtensorflow_*.so /usr/local/tensorflow/lib
```



nvidia@jetson-0423418010444:~/Documents/cpp_project$ ./tfcc
2019-03-07 16:41:17.469494: I tensorflow/stream_executor/cuda/cuda_gpu_executor.cc:931] ARM64 does not support NUMA - returning NUMA node zero
2019-03-07 16:41:17.469989: I tensorflow/core/common_runtime/gpu/gpu_device.cc:1432] Found device 0 with properties: 
name: Xavier major: 7 minor: 2 memoryClockRate(GHz): 1.5
pciBusID: 0000:00:00.0
totalMemory: 15.45GiB freeMemory: 1.10GiB
2019-03-07 16:41:17.470150: I tensorflow/core/common_runtime/gpu/gpu_device.cc:1511] Adding visible gpu devices: 0
2019-03-07 16:41:19.059567: I tensorflow/core/common_runtime/gpu/gpu_device.cc:982] Device interconnect StreamExecutor with strength 1 edge matrix:
2019-03-07 16:41:19.059842: I tensorflow/core/common_runtime/gpu/gpu_device.cc:988]      0 
2019-03-07 16:41:19.059982: I tensorflow/core/common_runtime/gpu/gpu_device.cc:1001] 0:   N 
2019-03-07 16:41:19.060585: I tensorflow/core/common_runtime/gpu/gpu_device.cc:1115] Created TensorFlow device (/job:localhost/replica:0/task:0/device:GPU:0 with 589 MB memory) -> physical GPU (device: 0, name: Xavier, pci bus id: 0000:00:00.0, compute capability: 7.2)
2019-03-07 16:41:21.223866: I ./main.cc:55] epoch 1: w=0.751655 b=0.29391 loss=0.0274053
2019-03-07 16:41:21.542139: I ./main.cc:55] epoch 2: w=0.846608 b=0.406805 loss=0.164987
2019-03-07 16:41:21.826643: I ./main.cc:55] epoch 3: w=0.941089 b=0.448278 loss=0.00171172
2019-03-07 16:41:22.117194: I ./main.cc:55] epoch 4: w=1.02611 b=0.466717 loss=0.00212118
2019-03-07 16:41:22.421440: I ./main.cc:55] epoch 5: w=1.10209 b=0.488073 loss=0.0786828
2019-03-07 16:41:22.725282: I ./main.cc:55] epoch 6: w=1.17624 b=0.479675 loss=0.00696869
2019-03-07 16:41:23.001284: I ./main.cc:55] epoch 7: w=1.24259 b=0.485361 loss=0.00326194
2019-03-07 16:41:23.289471: I ./main.cc:55] epoch 8: w=1.303 b=0.484753 loss=0.0274161
2019-03-07 16:41:23.565903: I ./main.cc:55] epoch 9: w=1.35979 b=0.486861 loss=0.00443319
2019-03-07 16:41:23.839075: I ./main.cc:55] epoch 10: w=1.41042 b=0.487121 loss=0.0335449
2019-03-07 16:41:24.112628: I ./main.cc:55] epoch 11: w=1.45836 b=0.493528 loss=0.0132444
2019-03-07 16:41:24.392869: I ./main.cc:55] epoch 12: w=1.50249 b=0.493722 loss=0.000421004
2019-03-07 16:41:24.673248: I ./main.cc:55] epoch 13: w=1.54214 b=0.492729 loss=0.0124819
2019-03-07 16:41:24.943316: I ./main.cc:55] epoch 14: w=1.57859 b=0.492936 loss=0.0205041
2019-03-07 16:41:25.215525: I ./main.cc:55] epoch 15: w=1.61338 b=0.494677 loss=0.00140292
2019-03-07 16:41:25.486765: I ./main.cc:55] epoch 16: w=1.6444 b=0.495493 loss=0.00356482
2019-03-07 16:41:25.746055: I ./main.cc:55] epoch 17: w=1.67282 b=0.494353 loss=0.00804859
2019-03-07 16:41:26.002922: I ./main.cc:55] epoch 18: w=1.69897 b=0.49576 loss=0.00925522
2019-03-07 16:41:26.231166: I ./main.cc:55] epoch 19: w=1.72344 b=0.497813 loss=0.00412729
2019-03-07 16:41:26.467576: I ./main.cc:55] epoch 20: w=1.74556 b=0.496992 loss=0.00545393
2019-03-07 16:41:26.694135: I ./main.cc:55] epoch 21: w=1.76627 b=0.500005 loss=0.000584862
2019-03-07 16:41:26.922207: I ./main.cc:55] epoch 22: w=1.78507 b=0.499904 loss=0.000940883
2019-03-07 16:41:27.145010: I ./main.cc:55] epoch 23: w=1.80216 b=0.499362 loss=0.00290919
2019-03-07 16:41:27.385359: I ./main.cc:55] epoch 24: w=1.81826 b=0.500923 loss=0.000320149
2019-03-07 16:41:27.620054: I ./main.cc:55] epoch 25: w=1.83263 b=0.499837 loss=0.00290858
2019-03-07 16:41:27.834869: I ./main.cc:55] epoch 26: w=1.8463 b=0.501314 loss=0.00032206
2019-03-07 16:41:28.046506: I ./main.cc:55] epoch 27: w=1.85856 b=0.501068 loss=0.00116968
2019-03-07 16:41:28.277158: I ./main.cc:55] epoch 28: w=1.86988 b=0.501059 loss=0.00112716
2019-03-07 16:41:28.498097: I ./main.cc:55] epoch 29: w=1.88047 b=0.500762 loss=0.00014708
2019-03-07 16:41:28.709691: I ./main.cc:55] epoch 30: w=1.88993 b=0.501637 loss=0.000818347
2019-03-07 16:41:28.928918: I ./main.cc:55] epoch 31: w=1.89886 b=0.50171 loss=0.000140877
2019-03-07 16:41:29.147404: I ./main.cc:55] epoch 32: w=1.9067 b=0.502913 loss=0.00112039
2019-03-07 16:41:29.367084: I ./main.cc:55] epoch 33: w=1.91438 b=0.502272 loss=8.76888e-05
2019-03-07 16:41:29.581509: I ./main.cc:55] epoch 34: w=1.92122 b=0.503096 loss=9.92497e-06
2019-03-07 16:41:29.783177: I ./main.cc:55] epoch 35: w=1.9275 b=0.502563 loss=6.96661e-05
2019-03-07 16:41:29.999185: I ./main.cc:55] epoch 36: w=1.93322 b=0.503206 loss=0.000259633
2019-03-07 16:41:30.208383: I ./main.cc:55] epoch 37: w=1.93856 b=0.504136 loss=8.23007e-05
2019-03-07 16:41:30.422683: I ./main.cc:55] epoch 38: w=1.94346 b=0.503883 loss=6.48337e-05
2019-03-07 16:41:30.630648: I ./main.cc:55] epoch 39: w=1.94798 b=0.503872 loss=2.32864e-05
2019-03-07 16:41:30.839528: I ./main.cc:55] epoch 40: w=1.95209 b=0.503324 loss=8.82509e-05
2019-03-07 16:41:31.046315: I ./main.cc:55] epoch 41: w=1.95593 b=0.503888 loss=6.81071e-08
2019-03-07 16:41:31.259884: I ./main.cc:55] epoch 42: w=1.95942 b=0.503865 loss=1.0062e-08
2019-03-07 16:41:31.463522: I ./main.cc:55] epoch 43: w=1.96256 b=0.504159 loss=0.000160442
2019-03-07 16:41:31.664602: I ./main.cc:55] epoch 44: w=1.96555 b=0.504346 loss=7.69988e-05
2019-03-07 16:41:31.870408: I ./main.cc:55] epoch 45: w=1.96821 b=0.50445 loss=0.000138752
2019-03-07 16:41:32.063447: I ./main.cc:55] epoch 46: w=1.97076 b=0.504565 loss=2.90031e-05
2019-03-07 16:41:32.254081: I ./main.cc:55] epoch 47: w=1.97305 b=0.504098 loss=4.98831e-05
2019-03-07 16:41:32.472025: I ./main.cc:55] epoch 48: w=1.97515 b=0.504518 loss=4.20674e-05
2019-03-07 16:41:32.671546: I ./main.cc:55] epoch 49: w=1.9771 b=0.504679 loss=2.87651e-05
2019-03-07 16:41:32.860057: I ./main.cc:55] epoch 50: w=1.97888 b=0.504727 loss=1.25812e-05
2019-03-07 16:41:33.066668: I ./main.cc:55] epoch 51: w=1.98052 b=0.504632 loss=4.54919e-06
2019-03-07 16:41:33.265434: I ./main.cc:55] epoch 52: w=1.98199 b=0.504659 loss=5.72449e-05
2019-03-07 16:41:33.464912: I ./main.cc:55] epoch 53: w=1.98341 b=0.504865 loss=1.27067e-06
2019-03-07 16:41:33.668524: I ./main.cc:55] epoch 54: w=1.98469 b=0.50482 loss=3.59992e-08
2019-03-07 16:41:33.863386: I ./main.cc:55] epoch 55: w=1.98587 b=0.504924 loss=1.81591e-06
2019-03-07 16:41:34.048780: I ./main.cc:55] epoch 56: w=1.98691 b=0.504967 loss=3.36097e-05
2019-03-07 16:41:34.239706: I ./main.cc:55] epoch 57: w=1.98793 b=0.50514 loss=5.60373e-06
2019-03-07 16:41:34.437763: I ./main.cc:55] epoch 58: w=1.98882 b=0.50514 loss=2.03703e-05
2019-03-07 16:41:34.630575: I ./main.cc:55] epoch 59: w=1.98966 b=0.505059 loss=1.30769e-05
2019-03-07 16:41:34.821846: I ./main.cc:55] epoch 60: w=1.99043 b=0.505072 loss=1.63617e-05
2019-03-07 16:41:35.018024: I ./main.cc:55] epoch 61: w=1.99115 b=0.505079 loss=6.36616e-07
2019-03-07 16:41:35.201758: I ./main.cc:55] epoch 62: w=1.99177 b=0.505054 loss=1.19506e-05
2019-03-07 16:41:35.404581: I ./main.cc:55] epoch 63: w=1.99239 b=0.505126 loss=6.05516e-06
2019-03-07 16:41:35.594773: I ./main.cc:55] epoch 64: w=1.99294 b=0.505213 loss=4.29734e-06
2019-03-07 16:41:35.595074: I ./main.cc:58] elapsed time： 16.2057s
nvidia@jetson-0423418010444:~/Documents/cpp_project$ ^C
nvidia@jetson-0423418010444:~/Documents/cpp_project$ 

sudo cp -r tensorflow/contrib/makefile/downloads/absl/absl /usr/local/tensorflow/include/
