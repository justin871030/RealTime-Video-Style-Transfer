# RealTime-Video-Style-Transfer
![Teaser](/image/demo.gif)
A Style-Transfer module for RealTime camera videos.

---
## Preparation & Installation 

### Preparation
The trained-weights and data our system needs are stored [here](https://drive.google.com/drive/folders/1ZGiSLfpzYJQ050VhV8kYr7nuAatot-Rj?usp=sharing). 

The weights **rvm_mobilenetv3.pth** should be put under **/style_transfer/test/Model/**, **style_net-TIP-final.pth** should be put under **/mat/checkpoints/**.
### Installation test on Ubuntu 18
The codes are run on **cuda=11.0, torch=1.9.0, torchvision=0.10.0**
```sh
conda create -n <env_name> python=3.8
conda install cudatoolkit==11.0.3 -c conda-forge
conda install pytorch==1.9.0 torchvision==0.10.0  cudatoolkit=11.0 -c pytorch -c conda-forge
pip install -r requirement.txt
```  

### Installation test on Ubuntu 20.04.3 LTS
We built a new environment and test successfully with following steps.
```sh
conda create -n <env_name> python=3.8
conda activate <env_name>
pip install opencv-python
#### Install the pytorch, torchvision, and cudatoolkit you want. ####
conda install pytorch==1.9.0 torchvision==0.10.0 torchaudio==0.9.0 cudatoolkit=10.2 -c pytorch
#####################################################################
pip install scikit-image
pip install kornia
```
### Run
If you want to run the module:
```sh
python test.py
```
---

## Virtual Camera Creation

We use **pyvirtualcam** to create a virtual camera if you want to use this module on Google Meet. 

The package works on Windows and Linux, but we only test on Ubuntu 20.04.3 LTS.

### Python Package

Install **pyvirtualcam** from PyPI with:
```sh
pip install pyvirtualcam
```

### Ubuntu Package

**pyvirtualcam** uses **v4l2loopback** virtual cameras on Linux.

Install **v4l2loopback** on Ubuntu with:
```sh
sudo apt install v4l2loopback-dkms
```

### Usage
Create a virtual camera.
```sh
# Close the virtual devices
sudo modprobe -r v4l2loopback
# Create a virtual device (Google Meet)
sudo modprobe v4l2loopback video_nr=2 card_label="Virtual Camera" exclusive_caps=1
# Create a virtual device (Teams)
sudo modprobe v4l2loopback video_nr=2 card_label="Virtual Camera" exclusive_caps=0
# Check the devices
v4l2-ctl --list-devices
```
To use this module to send processed images to the virtual camera.
```sh
python test.py --virtual_camera
```
---

## Processing for Videos

We use **imageio**, **moviepy** to collect the audio and infomation of the video, and **tqdm** is for checking the progress. 
### Python Package

Install **imageio**, **moviepy**, and **tqdm** from PyPI with:
```sh
pip install imageio
pip install moviepy
pip install tqdm
```
### Usage
Generate the styled video:
```sh
python test.py --process_video --input <input_video> --output <output_video>
```
---

## Reference
This system mainly incorporated matting and style-transfer modules, which were forked from following implementations:

- [Usability wrapper for ReReVST](https://github.com/petteriTeikari/ReReVST-UX-Wrapper)

- [Robust Video Matting (RVM)](https://github.com/PeterL1n/RobustVideoMatting)

Here are the relevant papers:

- [Consistent Video Style Transfer via Relaxation and Regularization](https://daooshee.github.io/ReReVST/)

- [Robust High-Resolution Video Matting with Temporal Guidance](https://peterl1n.github.io/RobustVideoMatting/) 