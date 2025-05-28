## Install CUDA + cuDNN + Conda Jupyter notebook on Ubuntu 25.04 plucky

--- GPU 5060ti 16g, cpu AMD

#### - 1. Install Nvidia driver for 5060ti on Ubuntu 25.04 plucky
First, install Nvidia driver following this [forum discussion](https://forums.developer.nvidia.com/t/we-would-like-to-know-when-the-nvidia-drivers-for-5060ti-on-ubuntu-will-be-released/331207/2).
Reminder. Usually a newly installed ubuntu system has not yet set root password. You may need to set a password for root first, for step init 3
```
$ sudo passwd root
```
Write your username's password (the one you set for the normal user when install)
then input twice the new password for root user. Done!
To switch to another user or root
```
$ su - username
$ su - root
```

#### - 2. Install CUDA and cuDNN on Ubuntu 25.04 plucky
Second, install CUDA and cuDNN in ubuntu following [this](https://zhuanlan.zhihu.com/p/691711768).
##### 2.1 CUDA installation
Following the instruction at the [cuda download site](https://developer.nvidia.com/cuda-downloads?target_os=Linux&target_arch=x86_64&Distribution=Ubuntu).
###### Step 1. Install cuda toolkit
```
$ wget https://developer.download.nvidia.com/compute/cuda/repos/ubuntu2404/x86_64/cuda-ubuntu2404.pin
$ sudo mv cuda-ubuntu2404.pin /etc/apt/preferences.d/cuda-repository-pin-600
$ wget https://developer.download.nvidia.com/compute/cuda/12.9.0/local_installers/cuda-repo-ubuntu2404-12-9-local_12.9.0-575.51.03-1_amd64.deb
$ sudo dpkg -i cuda-repo-ubuntu2404-12-9-local_12.9.0-575.51.03-1_amd64.deb
$ sudo cp /var/cuda-repo-ubuntu2404-12-9-local/cuda-*-keyring.gpg /usr/share/keyrings/
$ sudo apt-get update
$ sudo apt-get -y install cuda-toolkit-12-9
```
###### Step 2. Configure your system to use CUDA by updating environment variables
then use 
```
$ vim ~/.bashrc
```
to open bashrc and add these two lines into the end of the file,
```
export PATH=/usr/local/cuda/bin${PATH:+:${PATH}}
export LD_LIBRARY_PATH=/usr/local/cuda/lib64${LD_LIBRARY_PATH:+:${LD_LIBRARY_PATH}}
```
reload the environment setting
```
$ source ~/.bashrc
```
Now check if the CUDA is correctly installed,
```
$ nvcc -V
```
###### Further notes

In case if installed the wrong cuda version, then need to remove old version first and then install new version,
```
$ sudo apt remove nvidia-cuda-toolkit && sudo apt autoremove
$ sudo apt-get -y install cuda-toolkit-xx-x
```

##### 2.2 cuDNN installation
Download the right version from [this site](https://developer.nvidia.com/rdp/cudnn-archive). Unzip and copy files to corresponding cuda paths
```
$ tar -xvf cudnn-linux-x86_64-8.9.7.29_cuda12-archive.tar.xz 
$ sudo cp cudnn-linux-x86_64-8.9.7.29_cuda12-archive/include/cudnn*.h    /usr/local/cuda/include
$ sudo cp cudnn-linux-x86_64-8.9.7.29_cuda12-archive/lib/libcudnn*    /usr/local/cuda/lib64
$ sudo chmod a+r /usr/local/cuda/include/cudnn.h   /usr/local/cuda/lib64/libcudnn*
```
Please remember to copy those * in the code, otherwise cannot correctly install cuDNN. If successful, you'll get result #define CUDNN_MAJOR x message from 
```
$ cat /usr/local/cuda/include/cudnn_version.h | grep CUDNN_MAJOR -A 2
```
or 
```
$ cat /usr/local/cuda/include/cudnn.h | grep CUDNN_MAJOR -A 2
```

#### - 3. Install Anaconda and jupyter notebook on Ubuntu 25.04 plucky
Following [this](https://www.geeksforgeeks.org/how-to-install-jupyter-notebook-in-linux/), use Anaconda to install Jupyter Notebook.
##### Step 1. Download Anaconda package, and then bash. Say yes when ask for adding paths to bashrc.
```
$ cd ~/Downloads
$ chmod +x Anaconda3-2024.10-1-Linux-x86_64.sh
$ bash Anaconda3-2024.10-1-Linux-x86_64.sh
$ source ~/.bashrc
$ conda --version
```
##### Step 2. Set up conda environment
```
$ conda create --name myenv 
$ conda activate myenv
```
##### Step 3. Launch jupyter notebook
```
$ jupyter notebook
```
#### - 3. Set Conda virtual environment and install pytorch
##### Step 1. Set Conda virtual environment
```
$ conda activate myenv
$ conda install ipykernel 
$ ipython kernel install --user --name=myenv
$ conda deactivate
$ jupyter notebook
```
now you can see the newly created environment is in the kernel list of jupyter notebook.
##### Step 2. Install pytorch
PyTorch stopped providing conda packages starting [October 2024](https://github.com/pytorch/pytorch/issues/138506). Therefore, I use the o[fficially recommended method](https://pytorch.org/get-started/locally/) and installed PyTorch through pip3.

Although Python 3 is usually installed by default on Ubuntu, pip is not. So, install pip with:
```
$ sudo apt install python3-pip
```
Then activate the environment and install pytorch
```
$ conda activate myenv
$ pip3 install --pre torch torchvision torchaudio --index-url https://download.pytorch.org/whl/nightly/cu128
$ conda deactivate
$ jupyter notebook
```
Now run in jupyter notebook selecting 'conda env:myenv' kernel
```
import torch
print(torch.cuda.is_available())  # Should print: True
print(torch.cuda.get_device_name(0))  # Should print: NVIDIA GeForce RTX 5060 Ti

device = torch.device("cuda" if torch.cuda.is_available() else "cpu")

print(device)
print(torch.cuda.memory_allocated())  # Should return a non-zero value
print(torch.cuda.is_available())  # Should print: True
```

In case if want to uninstalled old versions,
```
$ pip uninstall torch torchvision torchaudio -y
$ pip cache purge
```






