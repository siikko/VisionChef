VisionChef의 CV 파트는 카메라/이미지에서 주방 식재료(채소·과일·육류 등)를 검출해 상위 추천(LLM/RAG) 파이프라인에 전달하는 역할을 수행합니다. 이 폴더에는 YOLO 기반 학습·추론 관련 스크립트와 실험 로그가 포함됩니다.

## 개요

- 목적: 사용자가 카메라로 비춘 식재료를 자동으로 인식하여 레시피 추천에 필요한 재료 목록(클래스 + bbox + confidence)을 생성
- 접근 방식: Ultralytics YOLOv11 계열 객체 탐지 모델 사용
- 입력: 단일 이미지 → 확장: 연속 이미지, 웹캠 스트림
- 출력: 클래스명, 바운딩 박스(x,y,w,h), confidence


## 결과 요약
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

### 학습 예시 (Ultralytics 명령어 형태)

```bash
# data.yaml 준비 후
yolo detect train data=CV/data/data.yaml model=yolo11n.pt imgsz=640 epochs=100 batch=16 project=CV/runs name=train_expt
```

## 실험 결과(요약된 관찰)

- Precision-Recall curve: 전체 mAP@0.5 ≈ 0.936 (그래프 요약)
- Confidence 기반 F1/Precision/Recall curve: 신뢰도 0.0~0.8 구간에서 전반적으로 안정적 성능

---