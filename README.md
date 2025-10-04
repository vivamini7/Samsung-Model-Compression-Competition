# Samsung-Model-Compression-Competition

팀 정보

팀명: 프로메테우스

팀원:

김도현 (한양대)

윤상민 (연세대)

정연석 (고려대)

📝 Problem Definition

대형 언어 모델(LLM)에서 파라미터 수를 줄이면서도 성능 저하를 최소화하는 방법을 탐구.
특히 MoE(Mixture of Experts) 구조에서 전문가(Expert) 수를 줄이거나 병합하는 방식에 집중.

가설:
Base 모델과 Pruning/Merging 모델의 Representation을 비슷하게 맞추면,
성능은 유지하면서 파라미터 수를 줄일 수 있다.

데이터셋:
C4 데이터셋 기반 Router Logit 사용 (특정 도메인 치우침 최소화 목적).

🔧 Method
1. Expert Pruning

각 Expert의 활성화 비율(Router Logit 기반)을 계산

특정 Threshold 이하의 Expert 제거

2. Expert Merging

Router Score로 가중합 → Expert Representation의 중심 계산

Hierarchical Clustering으로 비슷한 Expert 그룹화

그룹 내 Expert를 SVD + 학습된 Coefficient로 병합

3. Adjustment of Activated Experts

토큰별 활성 Expert 수를 줄여 추론 비용 절감

성능 저하는 추가 알고리즘으로 보완

📊 Experiments & Results
Benchmark Datasets

English: Hellaswag, MMLU, Winogrande

Korean: CLIcK, KMMLU

조건: Zero-shot 평가

Expert Pruning (Top-k 기준)
Top-k	# Params	MMLU	Hellaswag	Winogrande	CLIcK	KMMLU
8	27.31B	0.7628	0.5950	0.7048	0.6276	0.5671
6	26.79B	0.7608	0.5944	0.7040	0.6271	0.5624
4	25.88B	0.7431	0.5938	0.7111	0.5900	0.5365
2	24.12B	0.7316	0.5928	0.7088	0.5639	0.5285

➡️ Top6 pruning이 성능-압축률 균형에서 최적

Activated Experts per Token
Experts	MMLU	Hellaswag	Winogrande	CLIcK	KMMLU
8	0.7787	0.5952	0.7024	0.6301	0.5772
6	0.7691	0.5915	0.6638	0.6090	0.5712
4	0.7072	0.5572	0.6006	0.5519	0.5056
2	0.2797	0.3602	0.5099	0.2887	0.2041

➡️ Activated Expert = 4 선택

Pruning vs. Pruning + Merging

Merging 효과가 크지 않고, 일부 데이터셋에서 성능 하락 관찰

안정성 문제로 최종적으로 Pruning만 적용

Mitigation of Reduced Performance

Training-free 알고리즘 2가지 방법 실험

Method 2가 더 안정적이며 성능 완화에 효과적

✅ Final Decision

Top 6 Pruning 적용

Activated Expert = 4

Training-free 보완 기법(Method 2) 적용

🔮 Future Work

Expert Merging 하이퍼파라미터 서치 및 Coefficient 학습 안정화

레이어별 Expert 수 동적 조정

🙏 Acknowledgement

Samsung AI Challenge 2025
