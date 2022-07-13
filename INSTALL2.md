# วิธีการติดตั้ง PyslowFast
# Note:ทดสอบบน Ubuntu 20.4
1. ติดตั้ง CUDA 
```
  1. ถ้ามี Driver ติดตั้งแล้ว เช็คเวอร์ชั่น CUDA ที่ Compatible กับไดรเวอร์  (nvidia-smi) แนะนำให้ไม่เกินเวอร์ชั่น 11.6
        (optinal) หากไดรเวอร์ใหม่เกิน CUDA > 11.6 แนะนำให้ลบออกให้หมดก่อน
        ด้วยคำสั่ง
        # ลบ cuda
        sudo /usr/local/cuda-11.4/bin/cuda-uninstaller 
        # ลบ nvidia driver
        sudo /usr/bin/nvidia-uninstall
        สามารใช้คำสั่ง
        sudo apt-get --purge remove "*cublas*" "cuda*" "nsight*"
        sudo apt-get --purge remove "*nvidia*"
        เพื่อล้างไฟล์ที่ตกค้างได้
        
  2. ลง CUDA
     2.1 ติดตั้ง package ที่จำเป็นก่อน
         sudo apt-get update
         sudo apt-get upgrade -y
         sudo apt-get install -y build-essential cmake unzip pkg-config
         sudo apt-get install -y libxmu-dev libxi-dev libglu1-mesa libglu1-mesa-dev
         sudo apt-get install -y libjpeg-dev libpng-dev libtiff-dev
         sudo apt-get install -y libavcodec-dev libavformat-dev libswscale-dev libv4l-dev
         sudo apt-get install -y libxvidcore-dev libx264-dev
         sudo apt-get install -y libgtk-3-dev
         sudo apt-get install -y libopenblas-dev libatlas-base-dev liblapack-dev gfortran
         sudo apt-get install -y libhdf5-serial-dev graphviz
         sudo apt-get install -y python3-dev python3-tk python-imaging-tk
         sudo apt-get install -y linux-image-generic linux-image-extra-virtual
         sudo apt-get install -y linux-source linux-headers-generic
         
      2.2 ติดตั้ง Graphic Drivers Personal Package Archives
         sudo add-apt-repository ppa:graphics-drivers/ppa
         sudo apt-get update
         
      2.3 ค้นหา Driver
          ubuntu-drivers devices
      
      2.4 ติดตั้งไดรเวอร์ ไดรเวอร์ที่เป็นเวอร์ชั่น > 5xx จะมาพร้อม CUDA เวอร์ชั่นใหม่ แนะนำให้ติดตั้ง 4xx
          sudo apt-get install nvidia-driver-460
          
      2.5

```


