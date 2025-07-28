# first update the kernel

Kernel update instructions to the latest stable for debian is mentioned in this folder's notes as kernel_updating . please read , please .

# Source

This [debian wiki](https://wiki.debian.org/NvidiaGraphicsDrivers) is the reference and you MUST read it ! . and also if you want to use CUDA for Tensorflow you MUST read it's documentation .
# Nvidia driver installation

#### 1. **Ensure Non-Free Repositories Are Enabled**

Your `/etc/apt/sources.list` already includes `contrib`, `non-free`, and `non-free-firmware`. No changes are needed.

#### 2. **Update the System**

Update the package lists:

```bash
sudo apt update
```

#### 3. **Install Prerequisites**

Install the required tools:

```bash
sudo apt install dkms build-essential linux-headers-$(uname -r)
```

#### 4. **Install the NVIDIA Driver**

Install the driver package:

```bash
sudo apt install nvidia-driver
```

#### 5. **Verify the Installation**

Reboot the system:

```bash
sudo systemctl reboot
```

After reboot, check the GPU status:

```bash
nvidia-smi
```

#### 6. **Optional: Install CUDA Toolkit**

If you need CUDA support (e.g., for AI development):

```bash
sudo apt install nvidia-cuda-toolkit
```

#### 7. **Blacklist Nouveau (if necessary)**

If you encounter issues with the Nouveau driver, create a configuration file:

```bash
sudo nano /etc/modprobe.d/blacklist-nouveau.conf
```

Add the following:

```
blacklist nouveau
options nouveau modeset=0
```

Update the initramfs and reboot:

```bash
sudo update-initramfs -u
sudo reboot
```
