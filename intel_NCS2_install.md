# Intel NCS2 在 docker 作法

還有一個比較快方法，找一台桌機或筆電 

![image](https://user-images.githubusercontent.com/47648250/211589137-8b6c3ab7-6038-4b9b-8e4e-f3cd9aa0b7fc.png)


安裝 VM (OS安裝 ubuntu20.04)
 
 可參考下列網址製作，會比較快做出 
 https://github.com/openvinotoolkit/openvino_contrib/wiki/How-to-build-ARM-CPU-plugin#approach-1-build-opencv-openvino-and-the-plugin-using-pre-configured-dockerfile-cross-compiling-the-preferred-way

基本上，下列4種平台都有提供比較快速的作法

![image](https://user-images.githubusercontent.com/47648250/211589674-9243b0ae-779f-41e0-99e4-1aae3b202376.png)
```
git clone --recurse-submodules --single-branch --branch=releases/2021/4 https://github.com/openvinotoolkit/openvino_contrib.git
cd openvino_contrib/modules/arm_plugin
docker image build -t arm-buster -f Dockerfile.RPi32_buster .
```

Dockerfile.RPi32_buster 內容要調成個人資訊

![image](https://user-images.githubusercontent.com/47648250/211589963-18c511fc-16bd-4d22-b3ab-2f03d20dc701.png)
```
mkdir build
docker container run --rm -ti -v $PWD/build:/arm_cpu_plugin arm-buster
```
