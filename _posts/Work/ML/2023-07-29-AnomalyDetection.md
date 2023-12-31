---
layout: post
title: 영상기반 이상탐지
subtitle: BDA에 응용을 위해 
comments: true
tags: [ML, CV, Anomaly Detection]
categories: [Work]
---
컴퓨터 비전 기반 Anomaly detection의 종류들 

# 알고리즘 타임라인 
Deep SVDD(2018) → Deep SAD(2020) → HSC(2021) → FCDD(2021) 
- Anomaly Detection 기본 개념 = Outlier 탐지 
<img src="https://lh3.googleusercontent.com/k3DRIxXETgehYvXeWJcMRg_LvMaHi-Aq1zQKp87yOPP_IeAXbXaC3xMehidis1mcmOCgIkO7PblmAi1PQeAmppz0V9ruR1GNUlAhnn3S91gRqstJ0BbNJzwQdmgP75WNmTy7sSXrJGU28yluvwTlxDk" width = "70%">
# Deep SVDD: Deep One-Class Classification (ICML 2018)
## Info
- Paper: Ruff, L et al (2018). Deep One-Class Classification, Proceedings of the 35th International Conference on Machine Learning
- Link: [http://proceedings.mlr.press/v80/ruff18a.html](http://proceedings.mlr.press/v80/ruff18a.html) 

## 기존 알고리즘: SVM & SVDD
- SVM(Support Vector Machine) 
	- 서로 다른 클래스를 분류하는 hyperplane을 찾는 방법

<img src="https://lh4.googleusercontent.com/ylBcQZ9VWWHmrHJciBQdEMqWLpUofhQs4mWzf5zz2OPayoLkdWviowjzeWrYEsdfFJEo010n-dge-vClq-U4U-L3KRDC2xCsG_mOhmZR3e_cpcMzHSvZ7yGKSNCoLeviAKpLBTdF0hkOStSrVGlpPE8" width = "50%">
- SVDD(Support Vector Data Description)
	- 비선형 SVM을 이용한 One-class classification의 대표적인 방법.
	- 데이터의 feature space에서 정상 데이터를 둘러싸는 Hypersphere를 찾고, 해당 boundary를 기준으로 이상치 탐지 
<img src = "https://lh3.googleusercontent.com/mApKmmjxUpGipFMaK3RJC30bLioJ5M1ObQqWNm8kFmnB7wNPw-u_gsuZgPjr8Bs_5ClOT3n6tZPwV6gy6Vsz72c7xiHVtresCL43-TInBeYpZtO6Lb8LCrL3nW9Epa44Jxwj7nUKD-qZHp0x37sc9Fk" width= "70%" >


## Deep-SVDD 
- Unsupervised anomaly detection 방법론 
	- 정상 데이터만을 사용하여 이상 탐지 모델을 학습 
	- 정상 데이터를 정상 feature를 대표하는 중심인 c에 가깝게 mapping하는 Kernel-based SVDD에 딥러닝을 접목
<img src = "https://lh4.googleusercontent.com/hpMYggs2xsKOcGxeNzx9Eb7v_kJj5SYkbWFnKPOG5ePzr46KbxQDOjezMH-rk_mUt9pTiUbWJpDXYR5ushrMqTHvdIzXKAcgHzPY4A_ddmdwv6DqkE6NdQ1N3YkWPHJChAtgzum36JQz3fQjjfix4EQ" width = "70%">


## 모델 구조
- SVDD 목적함수
![](https://lh4.googleusercontent.com/fbwNaGVouc8O8WwQtXBpnX9WPX3yYvF45nKek3iJpINiP7p6ejvCbbiB0bhpqPewZ_2mlhoXSEVuW4HRj4h9w5XIHJzjUxUSjxL21PDkT7U40vdD6cOR8Gq7eDAjqPTlV2r0QNZIDaTERBiPoALczzQ){: width = "70%" height = "70%}
- R 바깥의 점은 anomaly로 간주 ![](https://lh4.googleusercontent.com/EW691BrzZ69CAVjYZ5HMM-Q0EET2OV718aIvc4p31gZPE2Qzkdi9fyMolkm2H_-fxf5WdHmiL2O5Vl-UBdxtWkDFMrUMO5MZ85PXPZ55B8rAp2bdCYnVWwCUm4lcOur1d9BxzVAocOt5YLs9sD2PH5E){: width = "70%" height = "70%}
	- ξ: soft boundary 
	- ν: hyperparameter controlling penalty & volume of the sphere

- Deep-SVDD 목적함수 
![](https://lh3.googleusercontent.com/efUGqlF37nFbTSwrr4GVOSoLkrunY36B9sRJMgsx8ZwnQrPGm-1m7m89sUG8bcBJqmCElq_i_RRIhsA2YUEHEtpGj6QTFxA9ucflPnNeg-xBC7x6ix2NUCZ1bTArtn6YSNE6E0iD153bOesivSvirlU){: width = "70%" height = "70%}
![](https://lh5.googleusercontent.com/8p89PRMqjYWgEGSYCvbEe8WJdMPo45k6VawASl54YY2KmBrUoKYCMhOKczvHLjfOWX2PQ7dihjR4EaNcOJR61wugQeOYB7XngoIlWibSudEiyfUqIlAbuT7ayEnXyuczkgDYtvkFci4PiIFNYtc4V7w){: width = "70%" height = "70%}
* distance to the center 
	- 마지막 텀은 Weight decay regularizer로 작용 (오버피팅 방지, L2 regularizer)  
# Deep SAD: Deep Semi-Supervised Anomaly Detection (ICLR 2020)    
## Info    
- Paper: [https://arxiv.org/abs/1906.02694](https://arxiv.org/abs/1906.02694) 
- Github: [https://github.com/lukasruff/Deep-SAD-PyTorch](https://github.com/lukasruff/Deep-SAD-PyTorch)  

## Objective
- Semi-supervised anomaly detection 방법론; 정상/이상 데이터 전부 학습 
- Deep SVDD에 준지도학습을 적용, 소량의 이상 데이터를 추가해 정상/이상 feature가 각각 정상 center로부터 가깝게/멀게 학습해 성능을 높임 

## 모델 구조     
- Deep SVDD 목적함수 
![](https://lh3.googleusercontent.com/efUGqlF37nFbTSwrr4GVOSoLkrunY36B9sRJMgsx8ZwnQrPGm-1m7m89sUG8bcBJqmCElq_i_RRIhsA2YUEHEtpGj6QTFxA9ucflPnNeg-xBC7x6ix2NUCZ1bTArtn6YSNE6E0iD153bOesivSvirlU){: width = "70%" height = "70%}
- Deep SAD 목적함수 
![](https://lh6.googleusercontent.com/fCWCAjdGJ2FNSJSL0raQQ1RRX1_iZDO1aUHHt8cNGYbH-kIpIT1S5cwXyw1qMdQV2QOFrWl6rfgYOgVTg1myY6QeN8ijcx3uBUnm1fQjGZDsKi55SNDXP-6PartbGLNMtCU2Ds1rYzmMhu8eD8cg5ZU){: width = "70%" height = "70%}
- n개의 라벨링되지 않은 데이터와 m개의 라벨링 데이터 

# HSC: Rethinking Assumptions in Deep Anomaly Detection (ICML 2021)
## Info    
- paper: [https://arxiv.org/pdf/2006.00339.pdf](https://arxiv.org/pdf/2006.00339.pdf) 
- github: [https://github.com/lukasruff/Classification-AD](https://github.com/lukasruff/Classification-AD) 
## Objective    
- Semi-supervised anomaly detection + Outlier Exposure 방법론
- Deep SAD의 학습에 out-of-distribution data에 대한 cross-entropy classification을 추가해 이상치 탐지 성능을 높임 
## Anomaly detection approach     
- Unsupervised
	- 정상 데이터로 학습하는 이상 탐지 방법론
- Unsupervised Outlier Exposure
	- Unsupervised AD에 normal auxiliary data를 통합한 이상 탐지 방법
	- Semi-supervised AD or AD with negative examples 
- Supervised Outlier exposure
	- 정상 데이터와 이상 데이터를 분류하는 Classification 문제 
![](https://lh4.googleusercontent.com/4aVNaD0EBehcNsZmVHuJlk3BnfhU9klOBb_0OMoRGGQTECrE4nJMnHw0e_qzzeh46IAVGFFP3Fgm1qJB42SGWnCF6LL34bzeR1t6o9QoM_kPcjhjOvPgpQ4Pre_TOb3coQy-5TFoQytp2q83_RCP9w8){: width = "70%" height = "70%}

Outlier Exposure이 필요한 예시. 숫자가 아닌 인풋을 입력했을 때에도 숫자로 분류하는 것을 볼 수 있음 

## 모델 구조     
- 해결방안
	- 랜덤한 다른 종류의 정상 이미지를 학습에 추가 사용

![](https://lh3.googleusercontent.com/H9W2TjuqWNNuvctjI4phdxHA6YVSQZM1CUcNcILfJrdL2zHwlry5KR-PNemJB17Xv-FFjmBi22EUwATKCF77tjsBLwbHsnzSLt3xp7t42Ncxh8cjCHOgXbLYBSkXFcSMokm1G6prLEyOCMax9KYW91g){: width = "70%" height = "70%}

![](https://lh6.googleusercontent.com/QTbjW1hjc0MOPHeDwcBzSxbShYj8Sxw_lQfLNqjnYkrgt6XeOKnGmviVCrd40wjpLadTZAGJWolJLBsREsIamr4uRQRB4EZod08ZK4Pc6RQp-g3x_5qkFvZzugh7NqINIEucLVW_o5OoXubUTsgQj28){: width = "70%" height = "70%}

- HSC: Hypersphere Classifier
	- cross-entropy classification을 기반으로 Deep SAD를 발전시킨 모델 
- Objective function 
![](https://lh6.googleusercontent.com/9IBAKM_34xW_Uk7hSY4dQnQukTkO_8CO6oeQ1ODwHOgI9nl0czicMILI8dgExzkb9byoFsaLRCTXZ_K2rDCqCpVa9TFymNrDHV2CDUrzrtbeNktB-BD_MJYU0n7LEZJOu0u6KHYNiKmjAHyXlMv0XFo){: width = "70%" height = "70%}
- h: pseudo-Huber loss 

# FCDD: Explainable Deep One-Class Classification 
## Info
- Paper
	-  Liznerski, P. et al. (2021) ‘Explainable Deep One-Class Classification’. ICLR Conference paper, [https://arxiv.org/abs/2007.01760](https://arxiv.org/abs/2007.01760) 
- Github: [https://github.com/liznerski/fcdd](https://github.com/liznerski/fcdd) 

## Objective    
- FCDD: Fully Convolutional Data Description 
	- HSC를 기반으로 학습한 FCN의 heatmap에 fixed Gaussian kernel을 활용한 upsampling을 적용해 모델의 이상치 감지에 설명력을 더함 
	- 다만, Deep one-class classification 모델은 워터마크 등에 취약
	- 데이터 수집 시 전처리 필요 

## 모델 구조     
- Fully Convolutional Network (FCN)

![](https://lh6.googleusercontent.com/f9tdScKPd5OP-XExzc3NI_vDPUG1O5BgFLxLStF6tdFxOytfBiDE_dBn6KeAjHlkgMnnyKgIlU_lorZ-DaQTGp3eI05_fcWiwgGM4x_hwUtdwUK5XfmAF72nslSovuwcUG5jTbc-ciKCWWYOtYCM2oo){: width = "70%" height = "70%}

![](https://lh5.googleusercontent.com/xGA5i6vnfEKce7gsLSmv0cyP6iQ4XUEb8ppAQ6dwr03UeXR_wFzpFJEpwK5eVSsWXg2_opYTPLRJqEKVQkEfbgMvdqzkfFBxfXpq_ViYO6s7UzA3ot6KycWT7zXrO76K9zabAsucBBVCAYxsJO4Mai8){: width = "70%" height = "70%}

- Objective function ![](https://lh4.googleusercontent.com/7WfIHCn5Tw8UEwo6mO6ujrLZ1xHD0dKKlSX_Dc65B-4BOTys1dJP_C5L5ulOfMToiItQHe2nPCdVIEWygOEMWHFBPffzJ16s0BjIxjLC6Tk39ZKCYqjI9aeXC7uwofOtTNoLdlaq9aUim8mPqav0M2g{: width = "70%" height = "70%}
- 참고) HSC
![](https://lh6.googleusercontent.com/9IBAKM_34xW_Uk7hSY4dQnQukTkO_8CO6oeQ1ODwHOgI9nl0czicMILI8dgExzkb9byoFsaLRCTXZ_K2rDCqCpVa9TFymNrDHV2CDUrzrtbeNktB-BD_MJYU0n7LEZJOu0u6KHYNiKmjAHyXlMv0XFo){: width = "70%" height = "70%}
- c는 FCN 앞단에서 계산되므로 FCDD에서는 제거
![](https://lh5.googleusercontent.com/HmJC20AqChAUeNvuLzLHrv3QaNrOLtjN5Rck_j1Fpq179MLD8iXkjb3oiPNp9djLoTzDg8SA-laoPpLiYndnoWsDcBAC8kNrDrZb3ovm_t0U4iUSCM1rLSiRZmqba3KxZVDxle4Hyds6ZyVMt0G5akM){: width = "70%" height = "70%}
- Heatmap upsampling  
	- 가우시안 커널으로 heatmap 업샘플링 진행 

![](https://lh4.googleusercontent.com/nZEbn8efTL4W_dSx4o8Q1iEDAwQTJX5oYKMMcrEAlYz1OUXq1XU6otAQfSIG9ZagvfhVjMLxIk8AnDtZhGZijMQ19pOkRX6H_O85iRgTLus57bB6JbcdPLgRgWhbLDcuYNWfuW10JJvUM87XA3gfbc8){: width = "70%" height = "70%}

## 결과    

![](https://lh4.googleusercontent.com/WbsL2dTtybMnkgeFsdqjTlI_sNJv68tZ3O-BHWJXqsPXDR7MtQwRRqWAWUrj5VqLkEN7Or9iRyOEAXlemp9Icyu3z8cywPa23VDwGK-Z1cNiKK0Ab77WoWvJBQakuCAoBqmWFF3i9cehHikDawOYcsc){: width = "70%" height = "70%}

![](https://lh4.googleusercontent.com/sbvQopEJetnj51nCeOXppr1-ki0eADtO_gr0Jpr0D-K5i8LyQ8t-tL1s3CxVyXrcmwWnkJ8PePm7T25mWVoiZ5C-mBx5Z8sZBpwhKeiDOQ46H-4xa5JL8Zp2Ua8OWaj6kcLjP7NER7g1iWCmcnsNmsM){: width = "70%" height = "70%}

![](https://lh4.googleusercontent.com/2ZOC_xH1SPT1vwYf9mufSV9HxpopVFiZg1kKEPab_Kc_aSG7LYEu5eISo-IjHBXYZdCXoBptDM01QYIJ-iXh4T3vgjNOHPo3kUpXOHib5r_sX7B_b1LwVc-zHByI8FTnBHFb2UVD1jf9IScmBNdk2GY){: width = "70%" height = "70%}

# 응용: Application on Disaster assessment 
## Info     
- Paper: Disaster Anomaly Detector via Deeper FCDDs for Explainable Initial Responses (Open Access 2023 Jun, CVPR 제출본)
## 데이터셋
- AIDER: [https://github.com/ckyrkou/AIDER](https://github.com/ckyrkou/AIDER) 
	- Aerial Image Database for Emergency Response application 
	- Fire/smoke, Flood, Collapsed Building, Traffic Accidents 
![](https://lh6.googleusercontent.com/JJGiafFTRyYx8NHKpn9vA_xJOx2ijglmHrIagJjvzUBCl7U9SjLg_q1gCFreM8sdtG1417TuM6PUjHfdSBgpsU5Xsj_o0wGhtvSKuXhESnLT6OS1IiE1LZfBI7AKsi0SOZIxjZDAGpNNKB3V3oGZi14){: width = "70%" height = "70%}

## FCDD 알고리즘 결과

![](https://lh5.googleusercontent.com/ESAwx1UA1rXYf7p1hrgmSfI2KD4Cp0TQZe4ZP1rbiBJj0scbL-JuH3nYp3SEAA0hItYOlMRdIK-yKuIt2SUrlApCyuLiaM0cy2sCkZGzMS6nZCAc_2Hgo6cVn5eLnk_Yghvf9TyTltdKEB6WuQona4A){: width = "70%" height = "70%}

![](https://lh3.googleusercontent.com/eqJ19D4PJVx5LcqKaOCBVgKiQuwzXlTw5vrYeyErQWe0yozLjXNyfhKsWOaH9jtMK8YtZwgr6QsoqoQ22wyZvQPbYZ1r1Q8kmYsSNZKAi57oo4pbnqnE07-kRjLgba08g0RaKAdpDHDcsctSUBOO8E8){: width = "70%" height = "70%}


# 응용: Bomb Damage Assessment (BDA)

- BDA-G Standard (미군 양식)
![](https://lh4.googleusercontent.com/Uo8qdVgonUgpI8hbDctDQmjdG8f6ZMoU9I000MrWvOVf4tNEQItqrHAGwfw_TRLE40K2HzPGt-PtALWIdP4xAZtI_4rL6wpmAR7K_K3u54mBRwo841cNYXNaIArWhFdrlg2CMtNXmkNZqNWHg_HeZQo){: width = "70%" height = "70%}

- 이라크전 BDA 예시 
![](https://lh4.googleusercontent.com/znLOTni78WRFbBysaGHWz0xXOhUSgriTHuL9czHyLjQrvuDXXdFQKma0K-C3CkHN5JL77Mfasza4_-Q304D4YV-q8ouCnh71NvCKej5pwEfHQp2SblqKzY_bnXxrqlldhXByKecb2hIJSQPMc2NLv6k){: width = "70%" height = "70%}

![](https://lh4.googleusercontent.com/BRVbbfWLgaGlVh5aaB0UnAoB0LwQzaiF4dqOlYU8CNb5jVYXNlLfwbvhInZ9eu_c4iADSaA-NkSieI5ULUZB0hTjQz0CkbjgmQxKTSt3sf7T2fJFm9MWyfoIZzWglWqFm7t_M5jjUYlQEfep33Le3Vs){: width = "70%" height = "70%}

![](https://lh5.googleusercontent.com/kL3FZvlHV2d5TdAk1BNNkmc4ycSZOPBpDIhtH7kd3lx1Ytu0gpJRuOguPlUL4qxecu6IM-wCsdNfyWrLzd7lhPA01KTbfR87_S3-wHpHLBqK-BfN0mn-kLordPJPdKAJq8rh9P66Ab2cLvql8AUoOmk){: width = "70%" height = "70%}

![](https://lh4.googleusercontent.com/k0T98NgMg6U1vzx63CMB3u7zsn0qcEsRnFCjon7mlp-VdWXh6GB2i74jxVjwoyqa8rTdW3aWDJD4lA9Wnpnhaz5knnQQ_9Kcw8lKscMpfZQCq0dhPiyvq67-VtmIkzrM-tckuJLdBNtSoyVRbeyYJV0){: width = "70%" height = "70%}

![](https://lh5.googleusercontent.com/a7_s5uwggfozFXjdGTxHqbjIRBf1tnjrhdEVseIexeaxTSSxRg0LyBYmP4xLFdIf3DlbjM3KPNBplRqV-N5XpI7Wug7xQemimjqFb41yYcgSipt_cvsXQBYYzStkI1w6DlTjwUEPZI47Ae0bHDw9eBo){: width = "70%" height = "70%}

- WWII 시대 이미지 
![](https://lh4.googleusercontent.com/CRhEsMThsccoa9snV9eUSMtUi0jvwqQFwqj-j_gtyNV0bX4gxmkAoeNLdSwTPO0JNFzv_x3xAUEZ5_hVmTHDbSzNdpGkPBTuHAh1eCFwoPMLgctB7LkHWziG_WP-DoqW9ed4Ep6PBCM0dBprdwFwAdc){: width = "70%" height = "70%}

![](https://lh3.googleusercontent.com/2ZX-ElxpjtzEBO8mroCRPK9uCYwsKT-qBClfrYOr3NPPkM8WxxwFzW_72-m8l5LaaDB5W6knE5P9Bm_Yrq1nBGwnVyWV6HTkERGWwTOtthAc91rBbj2mvej7hjHkLHBIQH84Y_CzFG-oRfPAIu0JZE8){: width = "70%" height = "70%}

- Bomb Damage map (London, 1945)
![](https://lh3.googleusercontent.com/Ik3kkNM5vVUPAF_H9YuPmaWSU6XMDhYO9-xKUKWNveqiP5Pa7bG5Azz3KqNxX1YuJpYQXOJ3XUsh6rtFb9JI_mo0av1gI2KrQCWqjm_iol_dXdqXYXKoq_Ar-kR1glJMFEitkosPxpYm1hGv0GYzERs){: width = "70%" height = "70%}

# Reference
- Matlab(R2023a) Implementation: [https://kr.mathworks.com/help/deeplearning/ug/detect-anomalies-using-single-class-classification.html](https://kr.mathworks.com/help/deeplearning/ug/detect-anomalies-using-single-class-classification.html) 
- Ruff, L. et al. (2018) ‘Deep One-Class Classification’, in Proceedings of the 35th International Conference on Machine Learning. International Conference on Machine Learning, PMLR, pp. 4393–4402. Available at: https://proceedings.mlr.press/v80/ruff18a.html (Accessed: 25 July 2023).
- Ruff, L. et al. (2023) ‘Rethinking Assumptions in Deep Anomaly Detection’. arXiv. /home/ywoo/Documents/wyzworks.github.io/_posts/STP/CourseAvailable at: http://arxiv.org/abs/2006.00339 (Accessed: 25 July 2023).
- Liznerski, P. et al. (2021) ‘Explainable Deep One-Class Classification’. arXiv. Available at: https://doi.org/10.48550/arXiv.2007.01760.
- Yasuno, T., Okano, M. and Fujii, J. (2023) ‘Disaster Anomaly Detector via Deeper FCDDs for Explainable Initial Responses’. arXiv. Available at: http://arxiv.org/abs/2306.02517 (Accessed: 18 July 2023).
- Tilon, S. et al. (2020) ‘Post-Disaster Building Damage Detection from Earth Observation Imagery Using Unsupervised and Transferable Anomaly Detecting Generative Adversarial Networks’, Remote Sensing, 12(24), p. 4193. Available at: https://doi.org/10.3390/rs12244193.
