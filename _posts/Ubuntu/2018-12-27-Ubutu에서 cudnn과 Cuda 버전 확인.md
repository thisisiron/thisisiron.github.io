## cudnn version 확인 하는 법
```cat /usr/include/cudnn.h | grep CUDNN_MAJOR -A 2```
<img src="/assets/images/cudnn_version.png" >

## Cuda version 확인하는 법
```nvcc --version``` 명령어를 사용하지 않고 Cuda version을 확인하는 명령어

```cat /usr/local/cuda/version.txt```
<img src="/assets/images/cuda_version.png" >

## Nvidia GPU 상태 확인
```nvidia-smi```
<img src="/assets/images/nvidia_gpu_version.png" >