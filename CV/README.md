# CV - 식재료 인식 모델
VisionChef의 CV 파트는 카메라/이미지에서 주방 식재료(채소·과일·육류 등)를 검출해 상위 추천(LLM/RAG) 파이프라인에 전달하는 역할을 수행합니다. 이 폴더에는 YOLO 기반 학습·추론 관련 스크립트와 실험 로그가 포함됩니다.

## 개요

- 목적: 사용자가 카메라로 비춘 식재료를 자동으로 인식하여 레시피 추천에 필요한 재료 목록(클래스 + bbox + confidence)을 생성
- 접근 방식: Ultralytics YOLOv11 계열 객체 탐지 모델 사용
- 입력: 단일 이미지 → 확장: 연속 이미지, 웹캠 스트림
- 출력: 클래스명, 바운딩 박스(x,y,w,h), confidence

## 제공 파일 및 디렉터리

- `CV/` : CV 관련 코드 및 스크립트
- `CV/runs/` : (로컬 훈련 시 생성) 훈련/추론 로그, `results.csv`, 시각화 이미지들
- 모델 파일(.pt)은 별도 저장소 또는 아티팩트 스토리지에 보관합니다. (예: `CV/best1.pt`, `CV/best2.pt` 등은 로컬에 존재할 수 있음)

> 훈련 결과 요약(첨부된 로그/그래프 기준)이므로, 상세 수치는 `runs/detect/<run>/results.csv`를 확인하세요.

## 학습 요약

- 총 학습 에폭(요약 기준): 68
- 최종/대표 지표
	- mAP@0.5 (전체 평균): 약 0.93 (로그 기준 최종값 ≈ 0.9307)
	- 그래프 요약에서는 전체 클래스 평균 mAP@0.5 ≈ 0.936으로 보고됨
	- Precision / Recall: 평균 0.88 ~ 0.91 구간에서 안정화

### 권장 환경 설치

```bash
python -m venv .venv
source .venv/bin/activate
pip install -U pip
pip install ultralytics numpy opencv-python matplotlib
```

### 빠른 추론 예시

```bash
# 단일 이미지 추론
yolo detect predict model=CV/best1.pt source=path/to/image.jpg conf=0.25 save=True

# 디렉터리 내 모든 이미지에 대해
yolo detect predict model=CV/best1.pt source=CV/images/ conf=0.25 save=True
```

### 학습 예시 (Ultralytics 명령어 형태)

```bash
# data.yaml 준비 후
yolo detect train data=CV/data/data.yaml model=yolov8n.pt imgsz=640 epochs=100 batch=16 project=CV/runs name=train_expt
```

### 평가 및 시각화

- 훈련 중 생성된 `runs/detect/<run>/results.csv` 파일을 통해 에폭별 loss/precision/recall/mAP 지표를 확인하세요.
- Confusion matrix, Precision-Recall, Confidence curves 등은 `runs/detect/<run>/plots/` 또는 실험 스크립트로 생성됩니다.

## 실험 결과(요약된 관찰)

- Precision-Recall curve: 전체 mAP@0.5 ≈ 0.936 (그래프 요약)
- Confidence 기반 F1/Precision/Recall curve: 신뢰도 0.0~0.8 구간에서 전반적으로 안정적 성능
- Confusion matrix: 일부 클래스(특히 `양파`)에서 background 또는 인접 클래스와의 혼동이 관측됨
- 샘플 수 분포: 일부 클래스는 빈도 편차가 있어 클래스 불균형 영향 존재

## 제한점 및 권장 개선 사항

1. 데이터 보강: `양파` 등 성능이 낮은 클래스에 대한 추가 이미지 수집 및 다양한 환경(조명/배경/각도) 포함
2. 전처리·증강: 컬러·조명 증강, 랜덤 크롭/회전, MixUp/Copy-Paste 등 적용 검토
3. 추론 안정화: 프레임 간 스무딩(temporal smoothing), 트래킹 기반 다수결, confidence threshold/NMS 튜닝
4. 소스 구분 정책: 조미료/액상 소스류 등 RGB만으로 구분이 어려운 항목은 UI에서 사용자가 직접 체크하도록 분리
5. 모델 경량화 및 배포: 작은 백본, 양자화(INT8) 혹은 TensorRT/ONNX 변환으로 속도 최적화

---

_팁: 빠른 점검은 `ultralytics`의 `yolo detect predict`를 사용하세요. 모델 파일은 로컬/아티팩트 저장소에 보관하세요._

## Results Gallery (결과 이미지)

아래 이미지는 훈련/평가 시 유용한 시각화 항목입니다. 로컬 실험에서 생성된 이미지를 `CV/runs/detect/<run>/plots/` 또는 `CV/plots/`에 복사하면 이 README에서 바로 표시됩니다.

- Precision-Recall Curve
- Precision-Recall-Confidence / F1-Confidence Curves
- Precision-Recall (per class) / mAP summary
- Confusion Matrix (raw and normalized)
- Class distribution (instance counts)
- Bounding box statistics (x,y,width,height heatmaps)

예시(이미지 파일은 사용자가 생성하거나 `runs` 폴더에서 복사합니다):

```markdown
![Precision-Recall Curve](runs/detect/train9/plots/precision_recall.png)
![Confidence Curves](runs/detect/train9/plots/confidence_curves.png)
![Precision-Recall per class](runs/detect/train9/plots/precision_recall_per_class.png)
![Confusion Matrix](runs/detect/train9/plots/confusion_matrix.png)
![Class Distribution](runs/detect/train9/plots/class_distribution.png)
![BBox Stats](runs/detect/train9/plots/bbox_stats.png)
```

만약 로컬에서 Ultralytics가 생성한 `results.csv`와 플롯들이 있다면, 다음처럼 README에 표시할 수 있도록 파일을 복사하세요:

```bash
# 예: 복사해서 저장
mkdir -p CV/runs/detect/train9/plots
cp runs/detect/train9/plots/*.png CV/runs/detect/train9/plots/ || true
```

아래는 플롯을 생성하는 간단한 예시 스크립트(파이썬)입니다. `runs/detect/<run>/results.csv`를 읽어 matplotlib로 커브를 그려 저장합니다.

```python
import pandas as pd
import matplotlib.pyplot as plt
df = pd.read_csv('CV/runs/detect/train9/results.csv')
# 예: mAP50 값 추이
plt.plot(df['epoch'], df['metrics/mAP50(B)'])
plt.xlabel('epoch')
plt.ylabel('mAP50')
plt.savefig('CV/runs/detect/train9/plots/mAP50_curve.png')
```

원하시면 첨부해주신 실험 결과(이미지)를 제가 `CV/runs/detect/train9/plots/`로 정리해 README에 임베드해드리거나, 플롯 생성용 완전한 스크립트를 만들어 드리겠습니다.
