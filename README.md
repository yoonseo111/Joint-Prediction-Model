# 🦾전이학습을 통한 신체 관절 좌표 예측 모델 개발
### 학부 프로젝트 (2023.03 - 2023.06)

## 📌연구 목표
### 전이학습을 통해 openpose에서 학습되지 않은 자세의 신체 좌표 예측력 개선
#### OpenPose란?
- 카네기 멜런 대학교에서 개발한 라이브러리로 사진 또는 영상에서 실시간으로 사람들의 손, 얼굴, 신체의 핵심적인 관절을 추출
- 신경망 구조의 네트워크가 이미지를 입력받은 후 이미지에 대한 신뢰도 맵과 위치 맵 출력
- 한계점: 정적이고 사지의 변동이 크지 않은 자세의 경우는 관절을 잘 인식하나, 다리를 머리 위쪽으로 높이 뻗은 자세나 물구나무를 선 자세와 같이 극적인 자세에서는 관절 좌표를 제대로 인식하지 못함\
➡️ 기존 라이브러리의 한계를 극복하고 정확하게 신체 관절 좌표를 예측할 수 있는 모델 개발

## 🧐연구 내용
### 전이학습을 활용하여 신체 관절 좌표 예측 모델 개발 및 성능 평가
#### 전이학습이란?
- 거대한 데이터셋으로 학습한 가중치의 일부를 유사하거나 새로운 분야의 신경망에 복사한 후 그대로 재학습을 수행하는 것
- 적은 데이터셋으로도 좋은 성능을 얻을 수 있고 시간과 리소스 절약 가능\
➡️ 선형 모델의 학습된 가중치를 고정시키고 마지막을 제외한 모든 계층을 가져와서 새로운 작업에 맞게 재사용하는 특징 추출 방법 사용\
➡️ openpose가 정확하게 인식하는 이미지와 관절 좌표를 직접 찍은 이미지들의 데이터값을 병합한 후 새로운 모델을 학습

## 🏞️데이터
### 기본적으로 정적인 자세나 움직임이 크지 않은 자세의 이미지는 제외
- openpose가 관절 좌표를 정확하게 인식하는 이미지 (2000장)
- openpose가 관절 좌표를 정확하게 인식하지 못해 관절 좌표를 직접 찍은 이미지 + 이미지 증강 (1088장)

## 연구 방법
### 전이학습의 선행모델 성능 비교 후 CNN 학습을 통해 새로운 모델 개발
#### 전이학습 선행모델
fully connected layer는 포함시키지 않고 그 전의 layer를 통해 이미지 특징 추출\
✅ 전이학습의 선행모델로 VGG16 선정
1. VGG16
2. DenseNet121
3. MobileNet

#### ⭐️ CNN 학습
1. 이미지의 높이와 너비를 신경망에 입력하기 적합한 형식으로 변환 후 관절 좌표 배열 생성
2. VGG16 모델을 사용하여 이미지 특징맵 추출
3. 학습데이터는 1번에서 추출한 (3088,15,2) 형태의 point array와 2번에서 추출한 (3088,7,7,512) 특징맵
4. 학습데이터를 8:2의 train data와 validation data로 나누어 CNN 학습
5. 모델 성능 평가
6. 학습된 CNN으로부터 15개의 예측 신체 관절 좌표값 도출
7. 좌표 값을 이미지에 점으로 찍어 시각화

## 연구 평가
😄 openpose에서 인식하지 못한 관절 좌표를 제대로 인식하는 것을 통해 기존 모델의 한계 극복\
😔 분석에 적합한 대용량의 이미지 파일이 존재하지 않아 직접 데이터셋을 수집해야 하는 번거로움

## 향후 연구
- 적은 데이터셋의 자세들에 대한 추가 연구
- 스포츠 선수들의 자세 인식 및 감지를 통해 AI 심판과 AI 코칭기술 발전 기대
