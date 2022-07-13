# วิธีการติดตั้ง PyslowFast
## Note:ทดสอบบน Ubuntu 20.4
1. ติดตั้ง CUDA  + CUDNN
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
          
      2.5 Reboot เครื่อง และ Check Version CUDA ในกรณีนี้จะได้เป็น CUDA เวอร์ชั่น 11.4
      
      2.6 ติดตั้ง CUDA Toolkit เลือกให้ตรงเวอร์ชั่น , OS 
          https://developer.nvidia.com/cuda-toolkit-archive
          ในเคสนี้ใช้  Ubuntu20.4 ติดตั้งแบบ runfile (local)
          wget https://developer.download.nvidia.com/compute/cuda/11.4.0/local_installers/cuda_11.4.0_470.42.01_linux.run
          sudo sh cuda_11.4.0_470.42.01_linux.run --toolkit --silent --override
          
      2.7 ตั้งค่า enviroment variable ใน `~/.bashrc`
          sudo nano ~/.bashrc
          
          เติม enviroment variable ต่อท้ายตามนี้ (ถ้าเป็น cuda เวอร์ชั่นอื่นเปลี่ยน cuda-11.4 เป็น cuda 11.x ตามที่ติดตั้ง)
          
          export PATH=/usr/local/cuda-11.4/bin${PATH:+:${PATH}}
          export LD_LIBRARY_PATH=/usr/local/cuda-11.4/lib64:$LD_LIBRARY_PATH
          export LD_LIBRARY_PATH=/usr/local/cuda-11.4/include:$LD_LIBRARY_PATH
          export CUDA_HOME=/usr/local/cuda
          export USE_CUDA=1 
          export USE_CUDNN=1 
          export USE_MKLDNN=1  
      
      2.8 ติดตั้ง CUDNN เลือกให้ตรงกับเวอร์ชั่น CUDA ที่ติดตั้งไปก่อนหน้า (โหลดให้ตรง OS ด้วย)
          https://developer.nvidia.com/rdp/cudnn-archive
      
          ดาวโหลด cuDNN Runtime, Developer and Code Samples library ใช้ตัวติดตั้ว Deb(.deb) 
          และทำการติดตั้งด้วย
          sudo dpkg -i libcudnn8_8.2.4.15–1+cuda11.4_amd64.deb
          sudo dpkg -i libcudnn8-dev_8.2.4.15–1+cuda11.4_amd64.deb
          sudo dpkg -i libcudnn8-samples_8.2.4.15–1+cuda11.4_amd64.deb
```

2. ติดตั้ง Pytorch โดยการ Build ผ่าน Source ***แนะนำให้ทำใน virtual env อย่าง miniconda



