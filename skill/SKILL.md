---
name: paperbanana
description: 논문 방법론 텍스트로부터 출판 수준의 학술 다이어그램 생성
license: MIT-0
dependencies:
  env:
    - OPENROUTER_API_KEY (권장)
    - GOOGLE_API_KEY (대안)
  runtime:
    - python3
    - uv
---

# PaperBanana

논문의 방법론 섹션과 그림 캡션으로부터 출판 수준의 학술 다이어그램 및 파이프라인 그림을 생성합니다. PaperBanana는 멀티 에이전트 파이프라인(Retriever, Planner, Stylist, Visualizer, Critic)을 조율하여 NeurIPS, ICML, ACL 같은 학술대회에 적합한 카메라-레디 그림을 생성합니다.

## 환경 설정

```bash
cd <저장소-루트>
uv pip install -r requirements.txt
```

환경 변수 또는 `configs/model_config.yaml`을 통해 API 키를 설정하세요.

**옵션 1 (권장): OpenRouter API 키** — 텍스트 추론과 이미지 생성 모두에 하나의 키 사용:
```bash
export OPENROUTER_API_KEY="sk-or-v1-..."
```

**옵션 2: Google API 키** — Gemini API 직접 접근:
```bash
export GOOGLE_API_KEY="your-key-here"
```

두 키가 모두 설정된 경우, OpenRouter가 기본값으로 사용됩니다.

## 사용법

```bash
python skill/run.py \
  --content "방법론_텍스트" \
  --caption "그림_캡션" \
  --task diagram \
  --output output.png
```

## 파라미터

| 파라미터 | 필수 여부 | 기본값 | 설명 |
|-----------|----------|---------|-------------|
| `--content` | 예* | | 시각화할 방법론 섹션 텍스트 |
| `--content-file` | 예* | | 방법론 텍스트가 담긴 파일 경로 (`--content` 대안) |
| `--caption` | 예 | | 그림 캡션 또는 시각적 의도 |
| `--task` | 아니오 | `diagram` | 작업 유형: `diagram`(다이어그램) |
| `--output` | 아니오 | `output.png` | 출력 이미지 파일 경로 |
| `--aspect-ratio` | 아니오 | `21:9` | 종횡비: `21:9`, `16:9`, 또는 `3:2` |
| `--max-critic-rounds` | 아니오 | `3` | 최대 평가 개선 반복 횟수 |
| `--num-candidates` | 아니오 | `10` | 병렬로 생성할 후보 수 |
| `--retrieval-setting` | 아니오 | `auto` | 검색 모드: `auto`, `manual`, `random`, 또는 `none` |
| `--main-model-name` | 아니오 | `gemini-3.1-pro-preview` | VLM 에이전트용 메인 모델. 설정된 API 키에서 제공업체 자동 감지 |
| `--image-gen-model-name` | 아니오 | `gemini-3.1-flash-image-preview` | 이미지 생성용 모델. `gemini-3-pro-image-preview`도 지원 |
| `--exp-mode` | 아니오 | `demo_full` | 파이프라인: `demo_full`(Stylist 포함) 또는 `demo_planner_critic`(Stylist 미포함) |

*`--content` 또는 `--content-file` 중 하나가 필수입니다.

`--num-candidates` > 1일 경우, 출력 파일명은 `<기본명>_0.png`, `<기본명>_1.png` 등으로 지정됩니다.

## 출력

저장된 각 이미지의 절대 경로가 표준 출력(stdout)에 한 줄씩 출력됩니다.

## 예시

### 다이어그램

```bash
python skill/run.py \
  --content "트랜스포머 기반 인코더-디코더 아키텍처를 제안합니다. 인코더는 잔차 연결이 있는 12개의 자기 주의(self-attention) 레이어로 구성됩니다. 디코더는 크로스 어텐션을 사용하여 인코더 출력에 주의를 기울이고, 자기회귀 방식으로 목표 시퀀스를 생성합니다." \
  --caption "그림 1: 제안된 트랜스포머 아키텍처 개요" \
  --task diagram \
  --output architecture.png
```


## 중요 사항

- **실행 시간**: 단일 후보는 모델 및 네트워크 상태에 따라 일반적으로 3-10분 소요됩니다. 기본 10개 후보가 병렬로 실행될 경우, 총 ~10-30분 예상됩니다. 미리 계획하세요.
- **API 호출**: 각 후보는 여러 번의 LLM 호출을 포함합니다(Retriever + Planner + Stylist + Visualizer + 최대 3번의 Critic 라운드). 후보들은 효율성을 위해 병렬로 실행됩니다.
- **이미지 생성**: Visualizer 에이전트는 이미지 생성 모델(Gemini Image)을 호출하여 다이어그램을 렌더링합니다.

## 소개

PaperBanana는 자동 학술 삽화를 위한 참조 기반 멀티 에이전트 시스템인 **PaperVizAgent** 프레임워크를 기반으로 합니다. 다음 연구 논문의 일환으로 개발되었습니다:

> **PaperBanana: Automating Academic Illustration for AI Scientists**
> Dawei Zhu, Rui Meng, Yale Song, Xiyu Wei, Sujian Li, Tomas Pfister, Jinsung Yoon
> arXiv:2601.23265

이 프레임워크는 다섯 가지 전문화된 에이전트(Retriever, Planner, Stylist, Visualizer, Critic)로 구성된 협력 팀을 도입하여 원시 과학 콘텐츠를 출판 수준의 다이어그램으로 변환합니다. 평가는 **PaperBananaBench** 벤치마크에서 수행됩니다.
