# วิธีการติดตั้ง PyslowFast ให้ทำตามลำดับ
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
      
  3. ติดตั้ง CUDNN เลือกให้ตรงกับเวอร์ชั่น CUDA ที่ติดตั้งไปก่อนหน้า (โหลดให้ตรง OS ด้วย)
          https://developer.nvidia.com/rdp/cudnn-archive
      
          ดาวโหลด cuDNN Runtime, Developer and Code Samples library ใช้ตัวติดตั้ว Deb(.deb) 
          และทำการติดตั้งด้วย
          sudo dpkg -i libcudnn8_8.2.4.15–1+cuda11.4_amd64.deb
          sudo dpkg -i libcudnn8-dev_8.2.4.15–1+cuda11.4_amd64.deb
          sudo dpkg -i libcudnn8-samples_8.2.4.15–1+cuda11.4_amd64.deb
```

2. ติดตั้ง Pytorch โดยการ Build ผ่าน Source ***แนะนำให้ทำใน virtual env อย่าง miniconda***
```
      1. อัพเดต conda
      conda update -n base -c defaults conda
      
      2. สร้าง virtual env ในที่นี้ใช้ python 3.9 (ตาม requirement ของ pyslowfast)
      conda create --name (ชื่อ env) python==3.9
      conda activate (ชื่อenv)
      
      3. ลง dependencies ที่จำเป็น
      conda install numpy ninja pyyaml mkl mkl-include setuptools cmake cffi typing_extensions future six requests dataclasses
      
      4. Clone pytorch repository
      git clone --recursive https://github.com/pytorch/pytorch
      cd pytorch
      
      5. กำหนด  CMAKE_PREFIX_PATH 
      export CMAKE_PREFIX_PATH="$~/miniconda3/envs/(ชื่อenv)" << กรณีใช้ miniconda
      
      6. install pytorch
      python setup.py install
      โดยให้สังเกตุใน log ด้วยว่า detectเจอ CUDA+CUDNN
      

```

3. ติดตั้ง ffmpeg `conda install -c conda-forge ffmpeg`
4. ติดตั้ง [torchvision](https://github.com/pytorch/vision/) ผ่าน souce (เช็คให้ชัวร์ว่าเจอ GPU torch.cuda.is_available())
```
      git clone https://github.com/pytorch/vision.git
      cd vision
      python setup.py install
```
5. ติดตั้ง [fvcore](https://github.com/facebookresearch/fvcore/): `pip install 'git+https://github.com/facebookresearch/fvcore'`
6. ติดตั้ง simplejson: `pip install simplejson`
7. ติดตั้ง PyAV: `pip install av` ***ห้ามใช้ conda install***
8. ติดตั้งiopath: `pip install -U iopath` or `conda install -c iopath iopath`
9. ติดตั้ง psutil: `pip install psutil`
10. ติดตั้ง OpenCV: `pip install opencv-python`
11. ติดตั้ง tensorboard  `pip install tensorboard`
12. ติดตั้ง moviepy: (สำหรับ visualizing video บน tensorboard) `conda install -c conda-forge moviepy` หรือ `pip install moviepy`
13. ติดตั้ง  pytorchvideo pip install "git+https://github.com/facebookresearch/pytorchvideo.git"
14. ติดตั้ง FairScale: `pip install 'git+https://github.com/facebookresearch/fairscale'`
15. ติดตั้ง [Detectron2](https://github.com/facebookresearch/detectron2):
```
    git clone https://github.com/facebookresearch/detectron2.git
    python -m pip install -e detectron2
```

16. ติดตั้ง pyslowfast 
```
    1. clone repo จากทางต้นทาง
    git clone https://github.com/facebookresearch/slowfast
    ****** หรือ clone จาก repository นี้จะแก้บัคให้หมดแล้ว *****
    cd slowfast
    
    2. add path ของ repo นี้ไป $PYTHONPATH
    export PYTHONPATH=/path/to/slowfast:$PYTHONPATH

    3.  ใน ไฟล์ setup.py แก้ install_requires จาก PIL >>>>> Pillow (ข้ามขั้นตอนนี้ถ้า clone จาก repo นี้)
    
    4. ติดตั้งผ่านคำสั่ง python setup.py build develop
    
```

17. แก้บัค Decoder Backend error (ข้ามขั้นตอนนี้ถ้า clone จาก repo นี้)
```
    1. ไป ที่ไฟล์ slowfast/datasets/decoder.py 
    
    2. แก้ไขตามนี้  https://github.com/facebookresearch/SlowFast/pull/541/files
    หรือใช้ไฟล์ decoder.py จาก repository นี้แทน

```
