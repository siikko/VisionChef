# 🍳 VisionChef

<img src="https://raw.githubusercontent.com/VisionChef/.github/main/profile/banner.png" alt="VisionChef Banner" width="100%" />

> **사용자가 가진 식재료를 AI가 시각적으로 파악해 맞춤형 요리를 제안하는 스마트 쿠킹 서비스**
>
> 단순한 레시피 검색을 넘어, 실시간 소통을 통해 요리의 완성까지 밀착 가이드하는 **인터랙티브 AI 코파일럿**입니다.

<br>

## 📌 프로젝트 소개

VisionChef는 냉장고 속 재료를 카메라로 찍으면, AI가 재료를 자동으로 인식하고 만들 수 있는 요리를 추천해줍니다.
추천에서 그치지 않고, 요리 과정 전반을 실시간으로 함께하는 **쿠킹 코파일럿** 경험을 제공합니다.

<img src="https://raw.githubusercontent.com/VisionChef/.github/main/profile/pipeline.png" alt="VisionChef Pipeline" width="100%" />

<br><br><br>

## ✨ 주요 기능

| 기능 | 설명 |
|------|------|
| 🥦 **식재료 시각 인식** | YOLOv8 모델이 카메라 속 식재료를 자동 감지 |
| 🍽️ **맞춤 레시피 추천** | 보유 재료 기반 RAG + LLM 레시피 검색 및 생성 |
| 🎙️ **음성 인터랙션** | STT/TTS로 손 안 쓰고 요리하며 AI와 소통 |
| 💬 **실시간 쿠킹 코파일럿** | 요리 중 질문하면 즉시 답변 & 단계별 가이드 |
| ✋ **제스처 컨트롤** | MediaPipe 손 인식으로 터치 없이 UI 조작 |
| ⏱️ **AI 타이머 & 단계 이동** | LLM이 대화 중 자동으로 타이머 설정 및 레시피 단계 이동 |
| 📺 **유튜브 레시피 연동** | YouTube 영상 검색 및 타임라인 기반 구간 재생 |

<br>

## 🛠️ 기술 스택

**AI / ML**
- **Computer Vision** — YOLOv8 기반 식재료 객체 탐지
- **LLM** — SKT A.X-4.0-Light (에이전트 툴 콜링 포함)
- **RAG** — ChromaDB · LangChain · BAAI/bge-m3 임베딩
- **STT / TTS** — Whisper · VARCO TTS API
- **Gesture** — MediaPipe 손 제스처 인식

**Web**
- **Backend** — FastAPI · Uvicorn
- **Frontend** — React

<br>

## 📁 레포지토리 구조

```
VisionChef/
├── LLM/      # RAG 파이프라인, STT, TTS
├── CV/       # YOLOv8 식재료 인식 모델
├── Web/      # FastAPI 백엔드 + React 프론트엔드
└── .github/  # 조직 프로필
```

<br>

## 👥 팀원

| 이름 | 역할 | GitHub |
|------|------|--------|
| 김진명 | LLM | [@JM-KIMM](https://github.com/JM-KIMM) |
| 이시은 | LLM | [@siikko](https://github.com/siikko) |
| 강민솔 | CV | [@min8-8](https://github.com/min8-8) |
| 이승진 | CV | [@L-SeungJin](https://github.com/L-SeungJin) |

<br>

---

<p align="center">
  인하대학교 인공지능공학과 &nbsp;|&nbsp; VisionChef Team
</p>