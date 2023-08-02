---
layout: post
title: "[Computer Vision] OpenCV를 활용한 비디오 스태빌라이징"
subtitle:
categories: Work
tags: [CV]
comments: true
---

OpenCV를 활용한 영상 안정화 

# Video Stabilization 
영상 안정화에는 HW, SW 방법을 모두 반영할 수 있다. 하드웨어의 경우에는 짐벌을 변경하던가, 자이로 등의 가속 센서를 통해서 카메라 자체적으로 진동을 해결할 수 있다. 혹은 렌즈 자체를 광학적으로 빛의 방향에 따라 이동시키는 방법도 있다.  
디지털 영상 안정화는 1) 모션 추정 2) 모션 스무싱 3) 이미지 매칭으로 구성된다. 

# OpenCV 기반 영상 스태빌  
## 영상 읽어오기  
### CV로 영상 읽기  
```
# Import numpy and OpenCV
import numpy as np 
import cv2

# Read input video 
cap = cv2.VideoCapture('video.mp4')
n_frames = int(cap.get(cv2.CAP_PROP_FRAME_COUNT))

w = int(cap.get(cv2.CAP_PROP_FRAME_WIDTH))
h = int(cap.get(CV2.CAP_PROP_FRAME_HEIGHT))

# Codec of output video 
fourcc = cv2.VideoWriter_fourcc(*'MPEG')

# Setup output video 
out = cv2.VideoWriter('video_out.mp4', fourcc, fps, (w,h))
```

### 흑백 프레임으로 변환 
```python
# Read first frame
_, prev = cap.read()
prev_gray = cv2.cvtColor(prev, cv2.COLOR_BGR2GRAY)
```

## 프레임간의 모션 찾기 
```Python
transforms = np.zeros((n_frames - 1, 3), np.float32)
```

### 특징점 찾기 GoodFeaturesToTrack
### Lucas-Kanade Optical Flow 
### 모션 연산 

## 

# Reference 
[LearnOpenCV](https://learnopencv.com/video-stabilization-using-point-feature-matching-in-opencv/)
