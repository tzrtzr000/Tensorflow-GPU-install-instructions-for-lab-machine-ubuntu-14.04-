# Tensorflow GPU installation on ubuntu 14.04 or 16.04(lab machine)    


0. update apt-get   
``` bash 
sudo apt-get update
```
   
1. Install apt-get deps  
``` bash
sudo apt-get install git python-dev python3-dev python-numpy python3-numpy build-essential python-pip python3-pip python-virtualenv swig python-wheel libcurl3-dev   
```

2. install nvidia drivers 
``` bash
# The 14.04 installer works with 14.10.
curl -O http://developer.download.nvidia.com/compute/cuda/repos/ubuntu1404/x86_64/cuda-repo-ubuntu1404_8.0.61-1_amd64.deb
# For 16.04: curl -O http://developer.download.nvidia.com/compute/cuda/repos/ubuntu1464/x86_64/cuda-repo-ubuntu1604_8.0.61-1_amd64.deb
dpkg -i ./cuda-repo-ubuntu1604_8.0.61-1_amd64.deb
sudo apt-get update
sudo apt-get install cuda -y
```  

2a. check nvidia driver install 
``` bash
nvidia-smi   

# you should see a list of gpus printed    
# if not, the previous steps failed.   
``` 

3. install cuda toolkit (MAKE SURE TO SELECT N TO INSTALL NVIDIA DRIVERS)
``` bash
wget https://s3.amazonaws.com/personal-waf/cuda_8.0.61_375.26_linux.run 
# Alternative: https://s3.amazonaws.com/tzr-tools/GPU-Support/cuda_8.0.61_375.26_linux.run

# for installing multiple CUDA versions in a machine:
# https://blog.kovalevskyi.com/multiple-version-of-cuda-libraries-on-the-same-machine-b9502d50ae77

sudo sh cuda_8.0.61_375.26_linux.run   # press and hold s to skip agreement   

# for cuda 9.0: sudo sh cuda_9.0.176_384.81_linux.run

# Do you accept the previously read EULA?
# accept

# Install NVIDIA Accelerated Graphics Driver for Linux-x86_64 361.62?
# ************************* VERY KEY ****************************
# ******************** DON"T SAY Y ******************************
# n

# Install the CUDA 8.0 Toolkit?
# y

# Enter Toolkit Location
# press enter


# Do you want to install a symbolic link at /usr/local/cuda?
# y

# Install the CUDA 8.0 Samples?
# y

# Enter CUDA Samples Location
# press enter    

# now this prints: 
# Installing the CUDA Toolkit in /usr/local/cuda-8.0 …
# Installing the CUDA Samples in /home/liping …
# Copying samples to /home/liping/NVIDIA_CUDA-8.0_Samples now…
# Finished copying samples.
```    

4. Install cudnn   
``` bash
wget https://s3.amazonaws.com/personal-waf/cudnn-8.0-linux-x64-v5.1.tgz
# wget https://s3.amazonaws.com/tzr-tools/GPU-Support/cudnn-8.0-linux-x64-v5.0-ga.tgz
sudo tar -xzvf cudnn-8.0-linux-x64-v5.1.tgz   
sudo cp cuda/include/cudnn.h /usr/local/cuda/include
sudo cp cuda/lib64/libcudnn* /usr/local/cuda/lib64
sudo chmod a+r /usr/local/cuda/include/cudnn.h /usr/local/cuda/lib64/libcudnn*
```    

5. Add the following command lines to the end of ~/.bashrc:   
``` bash
export LD_LIBRARY_PATH=/usr/local/cuda/lib64
export PATH="/usr/local/cuda/bin:/usr/local/cuda-8.0/bin:$PATH"
sudo ldconfig
export CUDA_HOME=/usr/local/cuda
```   

6. Reload bashrc     
``` bash 
source ~/.bashrc
```   

7. Install miniconda  ( or [Install pip and virtualenv](https://www.saltycrane.com/blog/2010/02/how-install-pip-ubuntu/))
``` bash
wget https://repo.continuum.io/miniconda/Miniconda3-latest-Linux-x86_64.sh
bash Miniconda3-latest-Linux-x86_64.sh   

# press s to skip terms   

# Do you approve the license terms? [yes|no]
# yes

# Miniconda3 will now be installed into this location:
# accept the location

# Do you wish the installer to prepend the Miniconda3 install location
# to PATH in your /home/ghost/.bashrc ? [yes|no]
# yes    

```   

8. Reload bashrc     
``` bash 
source ~/.bashrc
```   

9. Create conda env to install tf   
``` bash
conda create -n tensorflow

# press y a few times 
```   

10. Activate env   
``` bash
source activate tensorflow   
```

11. Install tensorflow with GPU support for python 2.7 for r0.11 Version 
``` bash
# pip install --ignore-installed --upgrade aTFUrl
# pip install --ignore-installed --upgrade https://storage.googleapis.com/tensorflow/linux/gpu/tensorflow-0.11.0rc2-cp27-none-linux_x86_64.whl

#for 0.12: 
pip install --ignore-installed --upgrade https://storage.googleapis.com/tensorflow/linux/gpu/tensorflow_gpu-0.12.0-cp27-none-linux_x86_64.whl
# for windows: 
# pip install --upgrade https://storage.googleapis.com/tensorflow/windows/cpu/tensorflow-0.12.0rc0-cp35-cp35m-win_amd64.whl
# pip install --upgrade https://storage.googleapis.com/tensorflow/windows/gpu/tensorflow_gpu-0.12.0rc0-cp35-cp35m-win_amd64.whl

# if not working try downloading and then installing locally:
# wget https://storage.googleapis.com/tensorflow/linux/gpu/tensorflow-0.11.0rc2-cp27-none-linux_x86_64.whl 
# pip install --ignore-installed --upgrade tensorflow-0.11.0rc2-cp27-none-linux_x86_64.whl
```   

12. Test tf install   
``` bash
# start python shell   
python

# run test script   
import tensorflow as tf   

hello = tf.constant('Hello, TensorFlow!')
sess = tf.Session()
print(sess.run(hello))
```  
