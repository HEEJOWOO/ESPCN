# ESPCN : https://arxiv.org/abs/1609.05158

#### <I studied while referring to various sites, but it is not enough. Thanks anytime for feedback.>
### <heejo5@naver.com>

Real-Time Single Image and Video Super-Resolution Using an Efficient Sub-Pixel Convolutional Neural Network
-----------------------------------------------------------------------------------------------------------
* LR 공간에서 작업을 한 뒤에 마지막층에서 HR로 변환하는 Sub-Pixel Conv Layer를 제안 하였으며 이 방법을 통해 메모리와 계산량 효율 적으로 사용 할 수 있었고 재구성의 정확도 또한 향상 

ESPCN Architecture
------------------
![image](https://user-images.githubusercontent.com/61686244/94778665-a209e280-0400-11eb-82e3-a7bac876f0b8.png)
* 우선 ISR은 네트워크를 통해 측정된 HR img 이고 ILR은 IHR에서 downscale된 img이며 IHR은 Original HR img임
* IHR에서 ILR로 downscale 할 때는 Gaussian filter을 이용해 blurry하게 만든 다음 upscaling ratio r로 downscale 됨
* ILR은 H x W x C 구조를 따르며 IHR은 rH x rW x rC의 모양을 가지게됨
* 네트워크 구조를 보면 알 수있듯이 LR img가 upscaling되지 않은채로 network의 input으로 들어가고 오른쪽 아래의 수식에서 볼 수 있듯이 Conv 2개를 거쳐 특징들을 추출해낸 다음 Sub-pixel Conv layer에서 HR space로 upscaling을 함
* 첫 번째 convolution layer를 통해 5x5 필터로 64개 채널을 뽑고 두번째 convolution layer를 통해 3x3 필터로 32개 채널을 뽑고 후에 sub pixel convolution layer를 통해 3x3 필터로 1개 채널을 뽑음

Convolution
-----------
![image](https://user-images.githubusercontent.com/61686244/94778872-f7de8a80-0400-11eb-8944-feda69be8819.png)

Transposed Convolution
----------------------
![image](https://user-images.githubusercontent.com/61686244/94798906-43069680-041d-11eb-96bf-51b07cf16cc6.png)

Efficient Sub-pixel Convolution Layer
-------------------------------------
![image](https://user-images.githubusercontent.com/61686244/94798998-6f221780-041d-11eb-9b54-e3192626451c.png)
* Transposed convolution의 단점을 개선하여 나온  sub pixel conv layer
* LR 특징 맵으로 부터 HR 이미지를 만들어내는 layer 각 특징 맵 마다 upscaling filter가 존재
* 식 3을 보게 되면 ps부분이 Efficient sub pixel Conv layer를 의미하며 Convolution Layer를 거쳐 나온 특징 맵을 주기적으로 셔플하여 재배치 한다는 것을 의미
* Sub pixel conv layer인 periodic shuffle ps를 거치전에는 shape이 H x W x (C x R x R)이였다가 ps를 거친 후 모양은 rH x rW x C로 변하게됨
* 식 5는 ESPCN의 Loss function은 MSE Loss이며 이는 LR img를 input으로 넣고, conv lyaer를 거쳐 sub pixel conv layer인 PS layer를 거쳐서 나온 output을 HR img에 대해서 pixel-wise mean square를 적용한 것 

The mean PSNR for different models
----------------------------------
![image](https://user-images.githubusercontent.com/61686244/94798998-6f221780-041d-11eb-9b54-e3192626451c.png)
![image](https://user-images.githubusercontent.com/61686244/94799516-38003600-041e-11eb-8cd8-b420793de1c8.png)

* 첫 번쨰 결과로 sub pixel convolution layer의 효과에 대해서 실험을 한 결과
* 첫 번째로 SR분야에서 평가지표로 많이 사용되는 네트워크 SRCNN과 비교한 결과로 91개의 img를 이용하여 학습한뒤에 비교해본 결과는 SRCNN과 같은 결과를 냈지만 ImageNet image 로 트레이닝을하고 ReLU 활성함수를 사용한 ESPCN 네트워크는 SRCNN과 비교했을때 더 좋은 결과를 달성

HD Videos from Xiph database
----------------------------
![image](https://user-images.githubusercontent.com/61686244/94799616-59f9b880-041e-11eb-9d63-8e49bacdbbe4.png)

![image](https://user-images.githubusercontent.com/61686244/94799476-2323a280-041e-11eb-8c44-294ce78f3272.png)

Conclusions
-----------
* 첫 번째 레이어에서 non-adaptive upscaling한 모델이 adaptive scaling한 모델과 비교해 SISR upscaling에 대해 성능이 더 낮고, 계산 복잡도가 높음
* HR Space 대신 LR Space 에서 특징을 추출함
* LR Space를 이용하기위해 sub-pixel Convolution layer를 사용-> 효율적인 메모리, 계산량
* 기존의 CNN방식과 비교해 속도와 성능 모두 향상, 시간 10배, img+0.15dB, vid+0.39dB
* 단일 GPU로 실시간 SR HD영상을 만들 수 있음
