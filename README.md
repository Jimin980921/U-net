## 주제  
__U-net알고리즘을 이용한 디노이징된 지정맥 고화질로 복원__  
- 개발환경: python3, tensorflow 1.14.0, keras=2.3.1  

------------------------------------

## 사용 알고리즘  

__U-Net__: Convolutional Networks for Biomedical Image Segmentation  
<img src="https://user-images.githubusercontent.com/57060127/89655110-90c2bf00-d904-11ea-9250-1dae9deea8cb.JPG" width="50%">  
* 특징  
  - 의약쪽에서 탁원한 효과를 보이는 Segmentation network  
  - classification + Localization  
  - 활성화함수 relu사용  
  
---------------------------------------------------------------------


## 개발 단계
- __1단계 : 데이터 특징점 강화__  
   - y_train의 지정맥데이터에 adaptive threshold를 이용하여 특징점 강조  
   - 원본 지정맥(x_train)과 특징점 강조 지정맥(y_train) 1:1쌍을 이루어 학습    
   
   
(a) __원본지정맥__ |  (b) __(a)를 수동추출__ | (c) __(b)의 특징점 강조(adaptive threshold)__ |
:------------------------------------:|:-------------------------:|:--------------------------:|
![](https://user-images.githubusercontent.com/57060127/86255296-e8795680-bbf1-11ea-95c9-d8af8b8534f1.jpg)  |  ![](https://user-images.githubusercontent.com/57060127/86255546-32fad300-bbf2-11ea-8f59-d7019f45d9df.jpeg)  |  ![](https://user-images.githubusercontent.com/57060127/93278404-85b25700-f7ff-11ea-9f2c-e74d3903e80b.JPG)  
<br>
<br>


- __2단계 : 정맥데이터 분리__  
   - x_train 1880개, x_test 600개, y_train 1880개  
<br>

- __3단계 : 딥러닝__
   - __U_net 알고리즘__ 으로 epoch=45, batch_size=30으로 학습하여 데이터 예측  
   - pred에서 예측확률이 0.3이상 부분만 출력  
   
(a) __x_train__ |  (b) __y_train__ | (c) __임계값(0.3)이상 예측__ |
:------------------------------------:|:-------------------------:|:--------------------------:|
![](https://user-images.githubusercontent.com/57060127/89191583-192c2180-d5de-11ea-8597-22f691eed448.JPG)  |  ![](https://user-images.githubusercontent.com/57060127/89191580-18938b00-d5de-11ea-905a-afdc52f102bb.JPG)  |  ![](https://user-images.githubusercontent.com/57060127/89191572-16313100-d5de-11ea-8b43-ba7522f5e475.JPG)  
<br>
<br>

- __4단계 : 정확도계산__  
   - 정답 데이터와 pred 데이터에서 __mean_iou(화소값 교집합/화소값 합집합)__ 로 정확도계산  
<br>

---------------------------------------------------------------------------------


## 결과  
디노이징된 정맥 원본사진에 정맥특징점 강화  
 
(a) __원본__ |  (b) __예측__ | (c) __(b)에서 임계값이상 특징점 이진화__ |  (d) __세선화__
:------------------------------------:|:-------------------------:|:--------------------------:|:----------------------------:
![](https://user-images.githubusercontent.com/57060127/86254185-6fc5ca80-bbf0-11ea-95c0-b5e69eb57521.jpg)  |  ![](https://user-images.githubusercontent.com/57060127/86254553-efec3000-bbf0-11ea-9bd4-e90a98270d6f.jpg)  |  ![](https://user-images.githubusercontent.com/57060127/86254701-2629af80-bbf1-11ea-8fb1-bbc4c9ad926d.jpg)  |  ![](https://user-images.githubusercontent.com/57060127/86254716-2e81ea80-bbf1-11ea-82ee-72c7d823c870.jpg)
<br>
<br>

----------------

- html5up-hyperspace= 웹 구현  
- data augumentation= brightness, contrast, mixture기법을 통해 data양을 증가  
- keras_u-net= keras코드 분석 및 실습  
참고: https://www.kaggle.com/keegil/keras-u-net-starter-lb-0-277  

- mean_iou 함수화: pred사진과 후처리한(정맥추출)사진의 정확도를 계산하기위해 하드코딩  
