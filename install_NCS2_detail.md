參考下面這篇做法
  https://www.intel.com/content/www/us/en/support/articles/000057005/boards-and-kits.html

  先將 作業系統降版回 buster， 
  ```
  2021-05-07-raspios-buster-armhf-full.zip
  ```
  raspios image下載位置：
  https://downloads.raspberrypi.org/raspios_armhf/images/raspios_armhf-2021-05-28/

  openvino 也使用 2021/04版本，

1. 先將 source code check out 出來
```    
       git clone --depth 1 --branch releases/2021/4 https://github.com/openvinotoolkit/openvino.git
```

2. 切換到 source code 做下同步 (under openvino 下 git branch可檢查版本是否跟 releases/2021/4 預期一樣)
![image](https://user-images.githubusercontent.com/47648250/211591419-68f8e93b-dc19-458c-babc-db0ab8c1180f.png)
```
cd openvino
git submodule update --init --recursive
```

3. 執行 install_build_dependencies.sh 
```
sh ./install_build_dependencies.sh
```

4. 宣告環境變數
```       
export OpenCV_DIR=/usr/local/lib/cmake/opencv4
```

5. 切換目錄加入 cython>=0.29.17 ，執行 python套件安裝
```
cd ~/openvino/inference-engine/ie_bridges/python/
pip3 install -r requirements.txt
```
![image](https://user-images.githubusercontent.com/47648250/211591935-13e3130c-ba1c-499c-bb4e-2199df31c292.png)

6. 安裝 libusb
 ```
 sudo apt install libusb-1.0-0-dev   
 ```
 
7. 開始 make，會很久
```
cd ~/openvino/build
     
cmake -DCMAKE_BUILD_TYPE=Release \
-DCMAKE_INSTALL_PREFIX=/home/pi/openvino_dist \
-DENABLE_MKL_DNN=OFF \
-DENABLE_CLDNN=OFF \
-DENABLE_GNA=OFF \
-DENABLE_SSE42=OFF \
-DTHREADING=SEQ \
-DENABLE_OPENCV=OFF \
-DNGRAPH_PYTHON_BUILD_ENABLE=ON \
-DNGRAPH_ONNX_IMPORT_ENABLE=ON \
-DENABLE_PYTHON=ON \
-DPYTHON_EXECUTABLE=$(which python3.7) \
-DPYTHON_LIBRARY=/usr/lib/arm-linux-gnueabihf/libpython3.7m.so \
-DPYTHON_INCLUDE_DIR=/usr/include/python3.7 \
-DCMAKE_CXX_FLAGS="-Wno-psabi -latomic" ..

make -j4

sudo make install
```

8. 測試
```
cd ~/openvino/bin/armv7l/Release

./object_detection_sample_ssd -h

cd ~/Downloads

wget https://download.01.org/opencv/2021/openvinotoolkit/2021.2/open_model_zoo/models_bin/3/person-vehicle-bike-detection-crossroad-0078/FP16/person-vehicle-bike-detection-crossroad-0078.bin

wget https://download.01.org/opencv/2021/openvinotoolkit/2021.2/open_model_zoo/models_bin/3/person-vehicle-bike-detection-crossroad-0078/FP16/person-vehicle-bike-detection-crossroad-0078.xml

wget https://cdn.pixabay.com/photo/2018/07/06/00/33/person-3519503_960_720.jpg -O walk.jpg

sudo usermod -a -G users "$(whoami)"

source /home/pi/openvino_dist/bin/setupvars.sh

sh /home/pi/openvino_dist/install_dependencies/install_NCS_udev_rules.sh

cd ~/openvino/bin/armv7l/Release

./object_detection_sample_ssd -i ~/Downloads/walk.jpg -m ~/Downloads/person-vehicle-bike-detection-crossroad-0078.xml -d MYRIAD
```
![image](https://user-images.githubusercontent.com/47648250/211593740-6d85ad17-0574-4005-8385-820252f72975.png)
![image](https://user-images.githubusercontent.com/47648250/211593822-1ec1c9d0-95e6-4ab4-95b6-954daeae9b0d.png)
![image](https://user-images.githubusercontent.com/47648250/211593925-7be856db-d1ec-4943-82c3-7b2488945ff2.png)

##成功結果
![image](https://user-images.githubusercontent.com/47648250/211594087-4176c07a-1c0c-4b38-a63f-7a8bc7d5a8c4.png)
![image](https://user-images.githubusercontent.com/47648250/211594212-a3d931c7-25fd-4ebd-b240-a69f4d72b779.png)

我把build 好的 raspbian buster 32 bit 檔案上傳學校的 google drive 了，只要下載做 make install，可以省下 編譯的時間
https://drive.google.com/drive/folders/1VsP9INHoZWeCs1g91E7hplbaYUWwSs5Y?usp=sharing

支援 ubuntu 20.04 64 bit 的 簡易作法，也將 build好的 檔案做上傳到 google drive
https://drive.google.com/drive/folders/1Eg5YCvHxV-Cf61vBpGBXngdOaeSHx272?usp=sharing 
