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

