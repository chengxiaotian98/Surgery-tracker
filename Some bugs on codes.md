# Some Bugs On GANerated Hands / DensePose
On 07/10 I tried to run those models, but both in vain. 

## GANerated Hands
It's a Matlab script. So I tried on my PC: <b>Win 10; 64 bit; NVIDIA GeForce GTX 950M; visual studio 2015</b>
### Requirement
* Caffe [×]: tried on both CPU/GPU Version, failed on building.
* CMake [√]:installed successfully

I followed the step in [Windows Caffe Github Repo](https://github.com/BVLC/caffe/tree/windows);
### GPU-Caffe install log
1. Install Cuda 8.0, report bugs on "visual studio integration"
2. Tried installing it without  "visual studio integration": succeed, but fail to verify the installation since it needs "visual studio integration"
3. Install Cudnn, add to cuda path and $PATH
4. Modify the [build_win.cmd](https://github.com/BVLC/caffe/blob/windows/scripts/build_win.cmd) in the repo, only build matlab caffe with GPU, reporting linking bugs: <br>
 
```
> "LINK : error LNK2001: 无法解析的外部符号 mexCreateMexFunction [D:\projects\software_default_project_directory\anaconda\caffe\build\Matlab\matlab.vcxproj] 
> LINK : error LNK2001: 无法解析的外部符号 mexDestroyMexFunction [D:\projects\software_default_project_directory\anaconda\caffe\build\Matlab\matlab.vcxproj]
> LINK : error LNK2001: 无法解析的外部符号 mexFunctionAdapter [D:\projects\software_default_project_directory\anaconda\caffe\build
\Matlab\matlab.vcxproj]
> D:/projects/software_default_project_directory/anaconda/caffe/build/Matlab/Release/caffe_.lib : fatal error LNK1120: 3个无法解析的外部命令 [D:\projects\software_default_project_directory\anaconda\caffe\build\Matlab\matlab.vcxproj]" 
```

### CPU-Caffe install log
1. Modify the [build_win.cmd](https://github.com/BVLC/caffe/blob/windows/scripts/build_win.cmd) in the repo, only build matlab caffe, reporting same bug as above.

I was assuming there went something wrong with my matlab. Is there any way to run it on a server, without graphic interface?

## DensePose
Still fail on Caffe2 setup
Followed the tutorial on [Caffe2 API](https://caffe2.ai/docs/getting-started.html?platform=ubuntu&configuration=compile#install-with-gpu-support)
Already installed <b> cuda 8.0, cudnn 7.0, nvidia-driver 384 </b> on Ubuntu 16.04 LTS, NVIDIA K80

There are several way to install, and I have tried prebuilt binaries and build from source

### Pre-Built Binaries
1. Install successfully without bugs by running:
   
   ```
   conda install pytorch-nightly cuda80 -c pytorch 
   ```
2. Verify installation
   ```
   # To check if Caffe2 build was successful
   python2 -c 'from caffe2.python import core' 2>/dev/null && echo "Success" || echo "Failure"
   
   Sucess
   ```
   ```
   # To check if Caffe2 GPU build was successful
   # This must print a number > 0 in order to use Detectron
   python2 -c 'from caffe2.python import workspace; print(workspace.NumCudaDevices())'

   0
   ```
   Seems working but could not detect any GPU device
### Build From Source 
1. Last week, after installing CUDA 8.0, cudnn 5.2, try running 
   ```
   git clone https://github.com/pytorch/pytorch.git && cd pytorch
   git submodule update --init --recursive
   python setup.py install
   ```
   failed at ``` python setup.py install ```, reporting that the machine is detected with CUDA 8.0 and giving hint that CUDA 9 and cudnn 7 or above should be the correct version
2. On 07/10, I remove CUDA 8.0 and reinstalled CUDA 9.0, re-ran the command above. Seems great without reporting bugs for like 5 minutes, but the system suddenly crashed down during the middle of installing.
3. Restarted the machine, re-ran the command, but it reported that the machine is detected with CUDA 8.0, though I was pretty sure I had removed all the Cuda 8.0 file according to [NVIDIA CUDA tool-kit document](https://docs.nvidia.com/cuda/cuda-installation-guide-linux/index.html); And the CUDA 9.0 was verified without any problem.
