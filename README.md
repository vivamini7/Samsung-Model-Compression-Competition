# Samsung-Model-Compression-Competition

본 레포지토리는 **2025 Samsung AI Challenge**의 최종 결과 내용을 정리. 코드는 비공개.

## 🏆 2등 (우수상) 수상
- **huggingface_모델**: [Qwen3-30B-A3B-PDS · Hugging Face](https://huggingface.co/homeb82784/Qwen3-30B-A3B-PDS)

| 팀원   | 소속   |
| ------ | ------ |
| 김도현 | 한양대 |
| 윤상민 | 연세대 |
| 정연석 | 고려대 |

---

## 📌 프로젝트 개요
- **목표**: MoE(Mixture of Experts) 기반 대형 언어 모델에서 **파라미터 수를 줄이면서도 성능 저하를 최소화**  
- **접근 전략**: Expert Pruning, Expert Merging, Activated Expert 조정 및 Training-free 보완 기법 활용  

---

## 🔧 Method

### Expert Pruning
- Router Logit 기반 Expert 활성화 비율 계산  
- Threshold 이하 Expert 제거  

### Expert Merging
- Router Score 가중합 → Expert Representation 중심 계산  
- Hierarchical Clustering 기반 Expert 그룹화  
- 그룹 내 Expert를 **SVD + Coefficient 학습**으로 병합  

### Adjustment of Activated Experts
- 토큰별 활성 Expert 수 감소 → 추론 비용 절감  
- 성능 저하는 **Training-free 보완 알고리즘**으로 완화  

---

## 📊 Experiments

### Benchmark Datasets
- **English:** Hellaswag, MMLU, Winogrande  
- **Korean:** CLIcK, KMMLU  
- **조건:** Zero-shot 평가  

### Expert Pruning (Top-k 기준)

| Top-k | Params | MMLU  | Hellaswag | Winogrande | CLIcK | KMMLU |
|-------|--------|-------|-----------|------------|-------|-------|
| 8     | 27.31B | 0.7628 | 0.5950 | 0.7048 | 0.6276 | 0.5671 |
| 6     | 26.79B | 0.7608 | 0.5944 | 0.7040 | 0.6271 | 0.5624 |
| 4     | 25.88B | 0.7431 | 0.5938 | 0.7111 | 0.5900 | 0.5365 |
| 2     | 24.12B | 0.7316 | 0.5928 | 0.7088 | 0.5639 | 0.5285 |

➡️ **Top-6 pruning**이 성능-압축률 균형에서 최적  

---

### Activated Experts per Token

| Experts | MMLU  | Hellaswag | Winogrande | CLIcK | KMMLU |
|---------|-------|-----------|------------|-------|-------|
| 8       | 0.7787 | 0.5952 | 0.7024 | 0.6301 | 0.5772 |
| 6       | 0.7691 | 0.5915 | 0.6638 | 0.6090 | 0.5712 |
| 4       | 0.7072 | 0.5572 | 0.6006 | 0.5519 | 0.5056 |
| 2       | 0.2797 | 0.3602 | 0.5099 | 0.2887 | 0.2041 |

➡️ 최종적으로 **Activated Expert = 4** 선택  

---

### Pruning vs. Pruning + Merging
- Merging은 효과가 크지 않고 일부 데이터셋에서 성능 하락  
- **Pruning 단독 적용**이 더 안정적  

---

### Mitigation of Reduced Performance
- Training-free 기법 2가지 적용  
- **Method 2**가 가장 효과적 → 성능 완화 확인  

---

## ✅ Final Decision
- **Top-6 Pruning 적용**  
- **Activated Experts = 4**  
- Training-free 보완 알고리즘(Method 2) 적용  

---

## 🚧 한계 및 향후 개선
### 한계
- Merging 과정에서 학습 안정성 부족  
- 일부 데이터셋에서 성능 하락  

### 향후 개선 방향
- Expert Merging 하이퍼파라미터 탐색 및 안정화  
- 레이어별 Expert 수 동적 조정  

---
