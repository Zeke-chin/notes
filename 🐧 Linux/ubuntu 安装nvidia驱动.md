1. ## 卸载原有驱动

```bash
./NVIDIA-Linux-x86_64-510.60.02.run --uninstall
sudo apt-get remove --purge nvidia*    
    sudo vim /etc/modprobe.d/blacklist.conf  
    # 恢复权限  
    sudo chmod 644 /etc/modprobe.d/blacklist.conf
```

2. ## 禁用nouveau

- 添加黑名单，添加可编辑权限

`sudo chmod 666 /etc/modprobe.d/blacklist.conf`

- 编辑`sudo vim /etc/modprobe.d/blacklist.conf  `
  - 进入后添加最后行添加:

```
     blacklist nouveau  
     blacklist lbm-nouveau  
     options nouveau modeset=0  
     alias nouveau off  
     alias lbm-nouveau off  
```

  - 恢复权限:`sudo chmod 644 /etc/modprobe.d/blacklist.conf`

3. ## 更新启动项

```
     sudo update-initramfs -u  
	 后
     sudo reboot  
```
     # 验证: 在终端输 lsmod | grep nouveau ，若无任何显示说明已禁用

4. ## 关闭图形相关服务

```
     sudo service lightdm stop  
     sudo service docker stop  
     sudo service nvidia-docker stop
```

- 安装build依赖
 `sudo apt install gcc make -y`
	
- 安装
  
```
sudo ./NVIDIA-Linux-x86_64-510.60.02.run \
	--no-x-check --no-nouveau-check --no-opengl-files  
```
     # –no-x-check 不检查x服务  
     # –no-nouveau-check 不检查nouveau  
     # –no-opengl-files 不安装OpenGL文档
    
    > **安裝過程中的選項：**
    > 
    > The distribution-provided pre-install script failed! Are you sure you want to continue? 選擇 yes 繼續。
    > 
    > Would you like to register the kernel module souces with DKMS? This will allow DKMS to automatically build a new module, if you install a different kernel later? 選擇 No 繼續。
    > 
    > 問題大概是：Nvidia’s 32-bit compatibility libraries? 選擇 No 繼續。
    > 
    > Would you like to run the nvidia-xconfigutility to automatically update your x configuration so that the NVIDIA x driver will be used when you restart x? Any pre-existing x confile will be backed up. 選擇 Yes 繼續

5. ## 重启相关服务

```
sudo service lightdm start  
sudo service docker start  
sudo service nvidia-docker start
```

# 卸载

```
  If you plan to no longer use the NVIDIA driver, you should make sure that no X screens are configured to use the NVIDIA X driver in your X configuration file. If you used nvidia-xconfig to configure X, it may have created a backup of your original configuration. Would
  you like to run `nvidia-xconfig --restore-original-backup` to attempt restoration of the original X configuration file?


NO
```

# 品牌机装驱动(华硕整机)

1. 显示可用的驱动`
```
# ubuntu-drivers devices

== /sys/devices/pci0000:00/0000:00:03.1/0000:1c:00.0 ==
modalias : pci:v000010DEd00001C03sv00003842sd00006161bc03sc00i00
vendor   : NVIDIA Corporation
model    : GP106 [GeForce GTX 1060 6GB]
driver   : nvidia-driver-435 - distro non-free
driver   : nvidia-driver-440-server - distro non-free
driver   : nvidia-driver-455 - third-party free recommended
driver   : nvidia-driver-418-server - distro non-free
driver   : nvidia-driver-450-server - distro non-free
driver   : nvidia-driver-390 - distro non-free
driver   : nvidia-driver-450 - distro non-free
driver   : xserver-xorg-video-nouveau - distro free builtin
```

3. apt install 安装推荐(`recommended`)版本
`sudo apt install nvidia-driver-455`

4. 安装好`cudatoolkit`后 进`.zshrc / .bashrc`
```bash
export LD_LIBRARY_PATH=$LD_LIBRARY_PATH:/usr/local/cuda-12.3/lib64
export PATH=$PATH:/usr/local/cuda-12.3/bin
export CUDA_HOME=$CUDA_HOME:/usr/local/cuda-12.3
```


## 参考文档

- [https://www.notion.so/ubuntu-16-04-nvidia-510-2f761714d33e4efa941d6c4533a889c9#c8d10ccfa2ed4c1b8e9afe172144007f](https://www.notion.so/ubuntu-16-04-nvidia-510-2f761714d33e4efa941d6c4533a889c9#c8d10ccfa2ed4c1b8e9afe172144007f)
- [https://blog.csdn.net/qq_46350148/article/details/108650556](https://blog.csdn.net/qq_46350148/article/details/108650556)
- [https://askubuntu.com/questions/1288902/nvidia-drivers-instalation-an-alternate-method-of-installing-the-nvidia-drive](https://askubuntu.com/questions/1288902/nvidia-drivers-instalation-an-alternate-method-of-installing-the-nvidia-drive)