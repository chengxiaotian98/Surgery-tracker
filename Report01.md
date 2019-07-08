# 07/01~07/04 Report
程笑天 MIKE CHENG 
## Brief Review
* [DensePose](http://densepose.org/) <br>
  Still trying to figure out the dependencies and environment
* [Cross-Modal VAE](http://openaccess.thecvf.com/content_cvpr_2018/papers/Spurr_Cross-Modal_Deep_Variational_CVPR_2018_paper.pdf)
<br> Aims to give an fine-tuned estimation of the hands. Perform not well on the [sugery videoset](https://www.youtube.com/playlist?list=PLegrqXHtHobDKdZDCcao5N9fweWrNIOej).

## DensePose
I have accountered problems in the setup.
### Literature Review
Could see the review I did in last semster, already attached to the drive.
### Baseline Running

#### Requirements
* NVIDIA GPU, Linux, Python2 <br>
  [√] On Google Cloud Platform (Ubuntu 16.04LTS,gcc 5.4.0, NVIDIA K80)
* Caffe2 (Suggested CUDA8.0 and cuDNN6.0.21 or 5.2), COCO API <br>
Stuck at Caffe2 setup, got a contradictory instruction from the document.
I will list all the steps below for reference.
#### Setup
Basically according to [Caffe2](https://caffe2.ai/docs/getting-started.html?platform=ubuntu&configuration=compile#install-with-gpu-support).<br>
1. Install all basic packages, CMake and gcc of correct version.<br>
[√] Without Problems
2. Update Nvidia drivers and cuda-toolkit.<br>
[√] Without Problem. Could refer to the [Nvidia document](https://docs.nvidia.com/cuda/cuda-installation-guide-linux/index.html) Section 2,3 and 7. Successfully installed all the NVIDIA Drivers 384.130, cuda 8.0.61. Could secure the installation with CudaSample Repo
3. Install cuDNN<br>
   [√?]]
   Installed 5.2. 6.0 needs registration.
4. Install pytorch with cuda (2 approaches)  <br>
    (1) clone from git [x],tried but failed. Gave error message saying would need cuda 9 and above, will do the whole installation in future days<br>
    (2) conda install torch [x],failed. installed successful though, in the test could not detect any device for cuda.
## Cross-Modal VAE hand pose estimation
### Literature Review
#### VAE (Variational auto-encoder)
##### Principal
According to original sample $x$, derives a latent distribution $z$, then perform the estimation: $\hat{x}$; The aim of VAE is to reduce the dimension generating a meaningful distribution, i.e. $p(z|x)$;<br>
$$p(z|x)=\frac{p(x|z)p(z)}{\int p(x|z)p(z)dz}$$
Since the calculation of the intergration could be taken up to exponential scale, they decided to replace a similar distribution $q(z|x)$ with $p(z|x)$;
Actually, we could use K-L Divergence to measure the closeness of two distributions. So we have 
$$\min{KL(q(z|x)\parallel p(z|x))}  $$
$$\equiv$$
$$\max{E_{q(z|x)}\log{p(x|z)}-KL(q(z|x)\parallel p(z))}$$
For the convenience, they assume $p(z)$ obey to $N(0,I)$. <br>
In fact, this guarantees the learning results $z$ could be general for the original data.
##### Practical tips
* The encoder only outputs $\mu$ and $\sigma$
* The network has a random float $\varepsilon$ obeying to $N(0,I)$ so that $z=\mu+\varepsilon \sigma$. 

#### Cross-Modal
![img](https://github.com/chengxiaotian98/Surgery-tracker/blob/master/illustrations/cross-modal-illust.png?raw=true)<br>
The input and output modality include 
* RGB image
* 2D key points of hand joints
* 3D key points of hand joints

The VAE projects the input into a common latent space $z$, and decode them into a different modality. For each pair of input-output modality (e.g. $RGB \rightarrow 3D points$), its encoder and decoder needs to be trained separately.
### Algorithm
![alg](https://github.com/chengxiaotian98/Surgery-tracker/blob/master/illustrations/cross-modal-algorithm.png?raw=true)
### [Setup](https://github.com/spurra/vae-hands-3d)
* Ubuntu 16.04
* tensorflow\torch\opencv
### Results
Since the inputs of the frame could be RGB, I have chosen some typical frames with clear hands in it of chosen videos from [Serena Surgeries](https://www.youtube.com/playlist?list=PLegrqXHtHobDKdZDCcao5N9fweWrNIOej).
![r1](https://github.com/chengxiaotian98/Surgery-tracker/blob/master/illustrations/classic_0.png?raw=true)
![r2](https://github.com/chengxiaotian98/Surgery-tracker/blob/master/illustrations/classic_1.png?raw=true)
![r3](https://github.com/chengxiaotian98/Surgery-tracker/blob/master/illustrations/classic_2.png?raw=true)
![r4](https://github.com/chengxiaotian98/Surgery-tracker/blob/master/illustrations/classic_4.png?raw=true)
![r5](https://github.com/chengxiaotian98/Surgery-tracker/blob/master/illustrations/classic_5.png?raw=true)

#### Remarks
* Could not locate the coordinates of the hands, had to get a Region of Interest beforehand.
* Failed to recover the hand pose when: (1) there are multiple hands in a frame; (2) the hand gets slightly occluded (3) the hands holding a tool (4) when the hand shows only a part of itself
#### Future Possible Improvements
* Performing a transfer learning on the new dataset might help to better the pose estimation
* May be a part of a pipeline where there will be an objector to have the exact bounding box

## Future Plan
* Trying to get the DensePose running
* Since my GCP server is running out of credit, I will learn how Sherlock works tomorrow
* Get some more state-of-art hand-tracker, including objection, segmentation and joint pose estimation.