---
layout: post
title: "[Computer Vision] OpenCV를 활용한 비디오 스태빌라이징"
subtitle:
categories: Work
tags: [CV]
comments: true
---

OpenCV를 활용한 영상 안정화 방법  
# Video Stabilization 
영상 안정화에는 HW, SW 방법을 모두 반영할 수 있다. 하드웨어의 경우에는 짐벌을 변경하던가, 자이로 등의 가속 센서를 통해서 카메라 자체적으로 진동을 해결할 수 있다. 혹은 렌즈 자체를 광학적으로 빛의 방향에 따라 이동시키는 방법도 있다.  
디지털 영상 안정화는 1) 모션 추정 2) 모션 스무싱 3) 이미지 매칭으로 구성된다.   

# OpenCV 기반 영상 스태빌  
## 영상 읽어오기  
### CV로 영상 읽기  
```python
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
```python
transforms = np.zeros((n_frames - 1, 3), np.float32)
```
  
### 특징점 찾기 GoodFeaturesToTrack
이전 프레임에서 다음 프레임으로 바뀔 때 변화에서 모션을 확인하기 위해 feature point(특징점)을 찾는다. 여러가지 알고리즘을 활용할 수 있지만 OpenCV에서는 GoodFeaturesToTrack라는 함수로 특징점을 연산한다.   

### Lucas-Kanade Optical Flow 
특징점을 찾았다면 Optical Flow를 활용해 프레임간의 이동을 추적한다. OpenCV에서는 calcOpticalFlowPyrLK 함수를 활용할 수 있다. 여기서 LK는 알고리즘의 창시자인 Lucas-Kanade를, Pyr은 피라미드를 말한다.   

### 모션 연산 
OpenCV에서는 estimateRigidTransform 함수를 활용해 프레임간에 발생하는 유클리디안 변형을 확인한다.   

```python 
for i in range(n_frames - 2):
	# OpenCV Feature extraction: GoodFeaturesToTrack 
	prev_pts = cv2.goodFeaturesToTrack(prev_gray, maxCorners = 200, qualityLevel = 0.01, minDistance = 30, blockSize = 3)
	success, curr = cap.read() 
	if not success:
		break 

	# Calculate optical flow of the features
	# Algorithm uses Lucas-Kanade Optical Flow: calcOpticalFlowPyrLK
	curr_gray = cv2.cvtColor(curr, cv2.COLOR_BGR2GRAY)
	curr_pts, status, err = cv2.calcOpticalFlowPyrLK(prev_gray, curr_gray, prev_pts, None)

	# Sanity check
	assert prev_pts.shape == curr_pts.shape

	# filter only valid points
	idx = np.where(status == 1)[0]
	prev_pts = prev_pts[idx]
	curr_pts = curr_pts[idx]

	# Estimate motion of the features 
	m = cv2.estimateRigidTransform(prev_pts, curr_pts, fullAffine = False)
```

# Reference 
[LearnOpenCV](https://learnopencv.com/video-stabilization-using-point-feature-matching-in-opencv/)
