## WSL - Nvidia GPU - Deep Learning

### Verify CUDA Support for WSL 2

Once a Windows NVIDIA GPU driver is installed on the system, CUDA becomes available within WSL 2. The CUDA driver installed on Windows host will be stubbed inside the WSL 2 as libcuda.so, therefore **users must not install any NVIDIA GPU Linux driver within WSL 2.**

[Offical Reference](https://learn.microsoft.com/en-us/windows/ai/directml/gpu-cuda-in-wsl)

[CUDA on WSL User Guide](https://docs.nvidia.com/cuda/wsl-user-guide/index.html)

[CUDA on Windows Subsystem for Linux (WSL)](https://developer.nvidia.com/cuda/wsl)\

[combined configuration for tensorflow, cuDnn, CUDA](https://www.tensorflow.org/install/source#tested_build_configurations)

[Other Reference](https://forums.developer.nvidia.com/t/windows-11-wsl2-cuda-windows-11-home-22000-708-nvidia-studio-driver-512-96/217721)

[Tensorflow on WSL2](https://www.tensorflow.org/install/pip#windows-wsl2) 


- compatiable versions will be used :  

  tensorflow=2.16.1  
  Python=3.9=3.12  
  Clang=17.0.6  
  Bazel=6.5.0  
  cuDNN=8.9  
  CUDA=12.3  
- Verify graphic card on Windows 11
```
control /name Microsoft.DeviceManager
```

### Install Nvidia GPU card driver

- Download Nvidia driver from https://www.nvidia.com/Download/index.aspx?lang=en-us and install the package on Windows 11

### Install CUDA Toolkit on WSL

- download CUDA Toolkit 12.3 for WSL: https://developer.nvidia.com/cuda-12-3-0-download-archive?target_os=Linux&target_arch=x86_64&Distribution=WSL-Ubuntu&target_version=2.0&target_type=deb_local

```
wget https://developer.download.nvidia.com/compute/cuda/repos/wsl-ubuntu/x86_64/cuda-wsl-ubuntu.pin
sudo mv cuda-wsl-ubuntu.pin /etc/apt/preferences.d/cuda-repository-pin-600
wget https://developer.download.nvidia.com/compute/cuda/12.3.0/local_installers/cuda-repo-wsl-ubuntu-12-3-local_12.3.0-1_amd64.deb
sudo dpkg -i cuda-repo-wsl-ubuntu-12-3-local_12.3.0-1_amd64.deb
sudo cp /var/cuda-repo-wsl-ubuntu-12-3-local/cuda-E7B9884F-keyring.gpg /usr/share/keyrings/
sudo apt-get update
sudo apt-get -y install cuda-toolkit-12-3
```
- ~~CUDA Toolkit 12.5 Downloads for WSL-Ubuntu 2.0 x86_64~~

```
wget https://developer.download.nvidia.com/compute/cuda/repos/wsl-ubuntu/x86_64/cuda-wsl-ubuntu.pin
sudo mv cuda-wsl-ubuntu.pin /etc/apt/preferences.d/cuda-repository-pin-600
wget https://developer.download.nvidia.com/compute/cuda/12.5.0/local_installers/cuda-repo-wsl-ubuntu-12-5-local_12.5.0-1_amd64.deb
sudo dpkg -i cuda-repo-wsl-ubuntu-12-5-local_12.5.0-1_amd64.deb
sudo cp /var/cuda-repo-wsl-ubuntu-12-5-local/cuda-*-keyring.gpg /usr/share/keyrings/
sudo apt-get update
sudo apt-get -y install cuda-toolkit-12-5
```
- check version of NVIDIA package:

```
nvidia-smi
```
- install Tensorflow 
```
python3 -m pip install tensorflow[and-cuda]==2.16.1
```

### Install Nvidia cuDNN

- download from https://developer.nvidia.com/cudnn-downloads the latest 
```
wget https://developer.download.nvidia.com/compute/cudnn/9.2.0/local_installers/cudnn-local-repo-ubuntu2204-9.2.0_1.0-1_amd64.deb
```
OR download from https://developer.nvidia.com/rdp/cudnn-archive and select the version for cudnn8.9, CUDA12.x, Ubuntu22.04. Then copy the file to WSL

- installation steps:
```
sudo dpkg -i cudnn-local-repo-ubuntu2204-8.9.7.29_1.0-1_amd64.deb
```

~~sudo cp /var/cudnn-local-repo-ubuntu2204-9.2.0/cudnn-local-8F8E147C-keyring.gpg /usr/share/keyrings/~~

```
sudo cp /var/cudnn-local-repo-ubuntu2204-8.9.7.29/cudnn-local-08A7D361-keyring.gpg /usr/share/keyrings/
```
```
sudo apt-get update
sudo apt-get -y install cudnn-cuda-12
sudo apt-get install libcudnn8 libcudnn8-dev
```
- check cuDNN installation version:

```
nvcc --version
```
- verify Tensorflow and GPU
```
python3 -c "import tensorflow as tf; print(tf.config.list_physical_devices('GPU'))"

python3 -c "import tensorflow as tf; print(tf.reduce_sum(tf.random.normal([1000, 1000])))"
```

**remove package**  [Reference](https://askubuntu.com/questions/151941/how-can-you-completely-remove-a-package)

- check package name
```
dpkg --list
```
```
sudo apt-get purge --auto-remove packagename
```