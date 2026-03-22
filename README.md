# <div align="center">PaperBanana 🍌</div>
<div align="center">Dawei Zhu, Rui Meng, Yale Song, Xiyu Wei, Sujian Li, Tomas Pfister and Jinsung yoon
<br><br></div>

</div>
<div align="center">
<a href="https://huggingface.co/papers/2601.23265"><img src="assets/paper-page-xl.svg" alt="HuggingFace 논문 페이지"></a>
<a href="https://huggingface.co/datasets/dwzhu/PaperBananaBench"><img src="assets/dataset-on-hf-xl.svg" alt="HuggingFace 데이터셋"></a>
</div>

> 안녕하세요! PaperBanana의 원본 버전은 Google-Research의 [PaperVizAgent](https://github.com/google-research/papervizagent)로 이미 오픈소스로 공개되어 있습니다.
> 이 저장소는 해당 리포지토리의 내용을 포크하여 학술 논문 삽화 지원을 지속적으로 발전시키는 것을 목표로 합니다. 상당한 진전이 이루어졌지만, 더 안정적인 생성과 더 다양하고 복잡한 시나리오를 위해서는 아직 갈 길이 멉니다. PaperBanana는 모든 연구자들의 학술 삽화 제작을 지원하기 위한 완전한 오픈소스 프로젝트를 지향합니다. 커뮤니티에 기여하는 것이 목표이므로, 현재 상업적 목적으로 사용할 계획은 없습니다.




## 최신 소식
- **2026-03-11**: PaperBanana를 [ClawHub 스킬](https://clawhub.ai/skills/paperbanana)로 게시 — `clawhub install paperbanana`로 설치 가능합니다.
- **2026-03-11**: Streamlit UI에 모델 선택 기능 추가 — 메인 모델(VLM)과 이미지 생성 모델을 별도로 선택할 수 있으며, 프리셋 옵션 및 직접 입력을 지원합니다.
- **2026-03-11**: OpenRouter 지원 추가 — 통합 API를 통해 OpenAI, Anthropic 및 기타 제공업체의 모델을 사용할 수 있습니다.
- **2026-03-11**: 모든 기여자를 위한 Contributors 섹션 및 all-contributors 봇 지원 추가.

## TODO 목록
- [ ] 수동으로 선택한 예시를 사용할 수 있는 기능 추가. **사용자 친화적인** 인터페이스 제공.
- [ ] 통계 플롯 생성 코드 업로드.
- [ ] 스타일 가이드라인을 기반으로 기존 다이어그램을 개선하는 코드 업로드.
- [ ] 컴퓨터 과학 외 더 많은 분야를 지원하도록 참조 세트 확장.

**PaperBanana**는 자동 학술 삽화 생성을 위한 참조 기반 멀티 에이전트 프레임워크입니다. 전문화된 에이전트들로 구성된 창의적인 팀처럼 동작하며, **Retriever(검색), Planner(계획), Stylist(스타일), Visualizer(시각화), Critic(평가)** 에이전트의 오케스트레이션 파이프라인을 통해 원시 과학 콘텐츠를 출판 수준의 다이어그램과 플롯으로 변환합니다. 이 프레임워크는 참조 예시로부터의 인컨텍스트 학습과 반복적 개선을 활용하여 미적으로 아름답고 의미적으로 정확한 과학 삽화를 생성합니다.

다음은 PaperBanana로 생성된 다이어그램과 플롯의 예시입니다:
![예시](assets/teaser_figure.jpg)

## PaperBanana 개요

![PaperBanana 프레임워크](assets/method_diagram.png)

PaperBanana는 다섯 가지 전문화된 에이전트를 구조화된 파이프라인으로 조율하여 고품질 학술 삽화를 생성합니다:

1. **Retriever 에이전트**: 하위 에이전트를 가이드하기 위해 큐레이션된 컬렉션에서 가장 관련성 높은 참조 다이어그램을 식별합니다
2. **Planner 에이전트**: 인컨텍스트 학습을 사용하여 방법론 내용과 전달 의도를 종합적인 텍스트 설명으로 변환합니다
3. **Stylist 에이전트**: 자동으로 합성된 스타일 가이드라인을 사용하여 학술 미적 기준에 맞게 설명을 세련되게 다듬습니다
4. **Visualizer 에이전트**: 최신 이미지 생성 모델을 사용하여 텍스트 설명을 시각적 결과물로 변환합니다
5. **Critic 에이전트**: 다중 라운드 반복 개선을 통해 Visualizer와 폐쇄 루프 개선 메커니즘을 형성합니다

## 빠른 시작

### 1단계: 저장소 클론
```bash
git clone https://github.com/sumilee-pcu/paperbanana.git
cd paperbanana
```

### 2단계: 설정
PaperBanana는 YAML 설정 파일 또는 환경 변수를 통한 API 키 설정을 지원합니다.

`configs/model_config.template.yaml` 파일을 `configs/model_config.yaml`로 복사하여 모든 사용자 설정을 외부화하는 것을 권장합니다. 이 파일은 API 키와 설정을 안전하게 보호하기 위해 git에서 제외됩니다. `model_config.yaml`에서 두 모델 이름(`defaults.main_model_name`과 `defaults.image_gen_model_name`)을 입력하고, `api_keys` 아래에 최소 하나의 API 키를 설정하세요(예: Gemini 모델용 `google_api_key`).

많은 후보를 동시에 생성해야 할 경우, 높은 동시성을 지원하는 API 키가 필요합니다.

### 3단계: 데이터셋 다운로드
먼저 [PaperBananaBench](https://huggingface.co/datasets/dwzhu/PaperBananaBench)를 다운로드한 후, `data` 디렉토리 아래에 배치하세요(예: `data/PaperBananaBench/`). 프레임워크는 데이터셋 없이도 Retriever 에이전트의 few-shot 학습 기능을 우회하여 정상적으로 동작하도록 설계되어 있습니다. 원본 PDF에 관심이 있으시면 [PaperBananaDiagramPDFs](https://huggingface.co/datasets/dwzhu/PaperBananaDiagramPDFs)에서 다운로드하세요.

### 4단계: 환경 설치
1. Python 패키지 관리를 위해 `uv`를 사용합니다. [여기](https://docs.astral.sh/uv/getting-started/installation/)의 지침에 따라 `uv`를 설치하세요.

2. 가상 환경 생성 및 활성화
    ```bash
    uv venv  # 현재 디렉토리의 .venv/ 아래에 가상 환경을 생성합니다
    source .venv/bin/activate  # Windows에서는 .venv\Scripts\activate
    ```

3. Python 3.12 설치
    ```bash
    uv python install 3.12
    ```

4. 필요한 패키지 설치
    ```bash
    uv pip install -r requirements.txt
    ```

### PaperBanana 실행

#### 인터랙티브 데모 (Streamlit)
PaperBanana를 실행하는 가장 쉬운 방법은 인터랙티브 Streamlit 데모를 사용하는 것입니다:
```bash
streamlit run demo.py
```

웹 인터페이스는 두 가지 주요 워크플로우를 제공합니다:

**1. 후보 생성 탭**:
- 방법론 섹션 내용(마크다운 권장)을 붙여넣고 그림 캡션을 입력하세요.
- 설정을 구성하세요(파이프라인 모드, 검색 설정, 후보 수, 종횡비, 평가 라운드).
- "후보 생성"을 클릭하고 병렬 처리가 완료될 때까지 기다리세요.
- 진화 타임라인이 포함된 그리드에서 결과를 확인하고, 개별 이미지 또는 배치 ZIP을 다운로드하세요.

**2. 이미지 개선 탭**:
- 생성된 후보 또는 임의의 다이어그램을 업로드하세요.
- 원하는 변경 사항을 설명하거나 업스케일링을 요청하세요.
- 해상도(2K/4K)와 종횡비를 선택하세요.
- 개선된 고해상도 결과물을 다운로드하세요.

#### 커맨드라인 인터페이스
커맨드라인에서도 PaperBanana를 실행할 수 있습니다:
```bash
# 기본 설정으로 실행
python main.py

# 커스텀 설정으로 실행
python main.py \
  --dataset_name "PaperBananaBench" \
  --task_name "diagram" \
  --split_name "test" \
  --exp_mode "dev_full" \
  --retrieval_setting "auto"
```

**사용 가능한 옵션:**
- `--dataset_name`: 사용할 데이터셋 (기본값: `PaperBananaBench`)
- `--task_name`: 작업 유형 - `diagram`(다이어그램) 또는 `plot`(플롯) (기본값: `diagram`)
- `--split_name`: 데이터셋 분할 (기본값: `test`)
- `--exp_mode`: 실험 모드 (아래 섹션 참조)
- `--retrieval_setting`: 검색 전략 - `auto`, `manual`, `random`, 또는 `none` (기본값: `auto`)

**실험 모드:**
- `vanilla`: 계획이나 개선 없이 직접 생성
- `dev_planner`: Planner → Visualizer만 실행
- `dev_planner_stylist`: Planner → Stylist → Visualizer
- `dev_planner_critic`: Planner → Visualizer → Critic (다중 라운드)
- `dev_full`: 모든 에이전트를 포함한 전체 파이프라인
- `demo_planner_critic`: 데모 모드 (Planner → Visualizer → Critic), 평가 없음
- `demo_full`: 데모 모드 (전체 파이프라인), 평가 없음

### 시각화 도구

파이프라인 진화 및 중간 결과 확인:
```bash
streamlit run visualize/show_pipeline_evolution.py
```
평가 결과 확인:
```bash
streamlit run visualize/show_referenced_eval.py
```

## 프로젝트 구조
```
├── .venv/
│   └── ...
├── data/
│   └── PaperBananaBench/
│       ├── diagram/
│       │   ├── images/
│       │   ├── pdfs/
│       │   ├── test.json
│       │   └── ref.json
│       └── plot/
├── agents/
│   ├── __init__.py
│   ├── base_agent.py
│   ├── retriever_agent.py
│   ├── planner_agent.py
│   ├── stylist_agent.py
│   ├── visualizer_agent.py
│   ├── critic_agent.py
│   ├── vanilla_agent.py
│   └── polish_agent.py
├── prompts/
│   ├── __init__.py
│   ├── diagram_eval_prompts.py
│   └── plot_eval_prompts.py
├── style_guides/
│   ├── generate_category_style_guide.py
│   └── ...
├── utils/
│   ├── __init__.py
│   ├── config.py
│   ├── paperviz_processor.py
│   ├── eval_toolkits.py
│   ├── generation_utils.py
│   └── image_utils.py
├── visualize/
│   ├── show_pipeline_evolution.py
│   └── show_referenced_eval.py
├── scripts/
│   ├── run_main.sh
│   ├── run_demo.sh
├── configs/
│   └── model_config.template.yaml
├── results/
│   ├── PaperBananaBench_diagram/
│   └── parallel_demo/
├── main.py
├── demo.py
└── README.md
```

## 주요 기능

### 멀티 에이전트 파이프라인
- **참조 기반**: 생성적 검색을 통해 큐레이션된 예시에서 학습
- **반복적 개선**: Critic-Visualizer 루프를 통한 점진적 품질 향상
- **스타일 인식**: 자동으로 합성된 미적 가이드라인이 학술 품질을 보장
- **유연한 모드**: 다양한 사용 사례에 맞는 여러 실험 모드

### 인터랙티브 데모
- **병렬 생성**: 최대 20개의 후보 다이어그램을 동시에 생성
- **파이프라인 시각화**: Planner → Stylist → Critic 단계를 통한 진화 추적
- **고해상도 개선**: 이미지 생성 API를 사용한 2K/4K 업스케일링
- **배치 내보내기**: 모든 후보를 PNG 또는 ZIP으로 다운로드

### 확장 가능한 설계
- **모듈식 에이전트**: 각 에이전트를 독립적으로 설정 가능
- **작업 지원**: 개념 다이어그램과 데이터 플롯 모두 처리
- **평가 프레임워크**: 여러 지표를 사용한 실제 정답과의 내장 평가
- **비동기 처리**: 설정 가능한 동시성으로 효율적인 배치 처리


## 커뮤니티 지원
이 저장소 공개 즈음에 커뮤니티에서 이 작업을 재현하려는 여러 노력이 있음을 확인했습니다. 이러한 노력들은 독특한 관점을 제시하며 매우 가치 있다고 생각합니다. 다음의 훌륭한 기여들을 꼭 확인해 보세요(누락된 것이 있으면 추가 요청 환영):
- https://github.com/llmsresearch/paperbanana
- https://github.com/efradeca/freepaperbanana
- https://github.com/elpsykongloo/PaperBanana-Pro — PaperBanana-Pro: 더 안정적인 파이프라인과 더 사용자 친화적인 경험을 갖춘 지속적으로 업데이트되는 중국어 강화 버전

또한 이 방법론의 개발과 함께 많은 다른 연구들이 자동 학술 삽화 생성이라는 동일한 주제를 탐구하고 있습니다. 일부는 편집 가능한 생성 그림까지 지원합니다. 이들의 기여는 생태계에 필수적이며 충분한 주목을 받을 가치가 있습니다(마찬가지로 추가 요청 환영):
- https://github.com/ResearAI/AutoFigure-Edit
- https://github.com/OpenDCAI/Paper2Any
- https://github.com/BIT-DataLab/Edit-Banana

전반적으로, 현재 모델들의 기본 기능이 자동 학술 삽화 생성 문제를 해결하는 데 훨씬 가까워졌다는 점에서 고무적입니다. 커뮤니티의 지속적인 노력으로, 가까운 미래에 학술 연구 반복과 시각적 소통을 가속화할 고품질 자동 드로잉 도구를 갖게 될 것으로 믿습니다.

PaperBanana를 더욱 발전시키기 위한 커뮤니티 기여를 환영합니다!

## 기여자

코드, 버그 보고, 아이디어, 피드백 등을 통해 PaperBanana 개선에 도움을 주신 모든 기여자들에게 감사드립니다!

<!-- ALL-CONTRIBUTORS-LIST:START -->
<!-- prettier-ignore-start -->
<!-- markdownlint-disable -->
<table>
  <tbody>
    <tr>
      <td align="center" valign="top" width="16.66%"><a href="https://github.com/dwzhu-pku"><img src="https://github.com/dwzhu-pku.png?s=100" width="100px;" alt="Dawei Zhu"/><br /><sub><b>Dawei Zhu</b></sub></a><br /><a href="https://github.com/dwzhu-pku/PaperBanana/commits?author=dwzhu-pku" title="코드">💻</a> <a href="#ideas-dwzhu-pku" title="아이디어, 계획 및 피드백">🤔</a> <a href="https://github.com/dwzhu-pku/PaperBanana/commits?author=dwzhu-pku" title="문서">📖</a></td>
      <td align="center" valign="top" width="16.66%"><a href="https://github.com/lemon-prog123"><img src="https://github.com/lemon-prog123.png?s=100" width="100px;" alt="lemon-prog123"/><br /><sub><b>lemon-prog123</b></sub></a><br /><a href="https://github.com/dwzhu-pku/PaperBanana/commits?author=lemon-prog123" title="코드">💻</a></td>
      <td align="center" valign="top" width="16.66%"><a href="https://github.com/memray"><img src="https://github.com/memray.png?s=100" width="100px;" alt="memray"/><br /><sub><b>memray</b></sub></a><br /><a href="https://github.com/dwzhu-pku/PaperBanana/commits?author=memray" title="코드">💻</a></td>
      <td align="center" valign="top" width="16.66%"><a href="https://github.com/elpsykongloo"><img src="https://github.com/elpsykongloo.png?s=100" width="100px;" alt="elpsykongloo"/><br /><sub><b>elpsykongloo</b></sub></a><br /><a href="https://github.com/dwzhu-pku/PaperBanana/issues?q=author%3Aelpsykongloo" title="버그 보고">🐛</a> <a href="#ideas-elpsykongloo" title="아이디어, 계획 및 피드백">🤔</a></td>
      <td align="center" valign="top" width="16.66%"><a href="https://github.com/weathon"><img src="https://github.com/weathon.png?s=100" width="100px;" alt="weathon"/><br /><sub><b>weathon</b></sub></a><br /><a href="https://github.com/dwzhu-pku/PaperBanana/issues?q=author%3Aweathon" title="버그 보고">🐛</a></td>
      <td align="center" valign="top" width="16.66%"><a href="https://github.com/arashabadi"><img src="https://github.com/arashabadi.png?s=100" width="100px;" alt="arashabadi"/><br /><sub><b>arashabadi</b></sub></a><br /><a href="https://github.com/dwzhu-pku/PaperBanana/commits?author=arashabadi" title="코드">💻</a></td>
    </tr>
    <tr>
      <td align="center" valign="top" width="16.66%"><a href="https://github.com/Ludobico"><img src="https://github.com/Ludobico.png?s=100" width="100px;" alt="Ludobico"/><br /><sub><b>Ludobico</b></sub></a><br /><a href="https://github.com/dwzhu-pku/PaperBanana/commits?author=Ludobico" title="코드">💻</a></td>
      <td align="center" valign="top" width="16.66%"><a href="https://github.com/haosenwang1018"><img src="https://github.com/haosenwang1018.png?s=100" width="100px;" alt="haosenwang1018"/><br /><sub><b>haosenwang1018</b></sub></a><br /><a href="https://github.com/dwzhu-pku/PaperBanana/commits?author=haosenwang1018" title="코드">💻</a> <a href="https://github.com/dwzhu-pku/PaperBanana/issues?q=author%3Ahaosenwang1018" title="버그 보고">🐛</a></td>
      <td align="center" valign="top" width="16.66%"><a href="https://github.com/stuinfla"><img src="https://github.com/stuinfla.png?s=100" width="100px;" alt="stuinfla"/><br /><sub><b>stuinfla</b></sub></a><br /><a href="https://github.com/dwzhu-pku/PaperBanana/commits?author=stuinfla" title="코드">💻</a> <a href="#ideas-stuinfla" title="아이디어, 계획 및 피드백">🤔</a></td>
      <td align="center" valign="top" width="16.66%"><a href="https://github.com/ReturnYG"><img src="https://github.com/ReturnYG.png?s=100" width="100px;" alt="ReturnYG"/><br /><sub><b>ReturnYG</b></sub></a><br /><a href="#ideas-ReturnYG" title="아이디어, 계획 및 피드백">🤔</a></td>
      <td align="center" valign="top" width="16.66%"><a href="https://github.com/Mylszd"><img src="https://github.com/Mylszd.png?s=100" width="100px;" alt="Mylszd"/><br /><sub><b>Mylszd</b></sub></a><br /><a href="#ideas-Mylszd" title="아이디어, 계획 및 피드백">🤔</a> <a href="#tool-Mylszd" title="도구">🔧</a></td>
      <td align="center" valign="top" width="16.66%"><a href="https://github.com/NielsRogge"><img src="https://github.com/NielsRogge.png?s=100" width="100px;" alt="NielsRogge"/><br /><sub><b>NielsRogge</b></sub></a><br /><a href="#ideas-NielsRogge" title="아이디어, 계획 및 피드백">🤔</a></td>
    </tr>
    <tr>
      <td align="center" valign="top" width="16.66%"><a href="https://github.com/MinyuChan-vem"><img src="https://github.com/MinyuChan-vem.png?s=100" width="100px;" alt="MinyuChan-vem"/><br /><sub><b>MinyuChan-vem</b></sub></a><br /><a href="https://github.com/dwzhu-pku/PaperBanana/issues?q=author%3AMinyuChan-vem" title="버그 보고">🐛</a></td>
      <td align="center" valign="top" width="16.66%"><a href="https://github.com/catallactics"><img src="https://avatars.githubusercontent.com/u/223395626?v=4?s=100" width="100px;" alt="catallactics"/><br /><sub><b>catallactics</b></sub></a><br /><a href="#ideas-catallactics" title="아이디어, 계획 및 피드백">🤔</a></td>
      <td align="center" valign="top" width="16.66%"><a href="https://github.com/ruiguo-bio"><img src="https://avatars.githubusercontent.com/u/20548903?v=4?s=100" width="100px;" alt="Rui Guo"/><br /><sub><b>Rui Guo</b></sub></a><br /><a href="https://github.com/dwzhu-pku/PaperBanana/issues?q=author%3Aruiguo-bio" title="버그 보고">🐛</a></td>
      <td align="center" valign="top" width="16.66%"><a href="https://github.com/YXDBright"><img src="https://avatars.githubusercontent.com/u/144319486?v=4?s=100" width="100px;" alt="YXDBright"/><br /><sub><b>YXDBright</b></sub></a><br /><a href="#ideas-YXDBright" title="아이디어, 계획 및 피드백">🤔</a></td>
      <td align="center" valign="top" width="16.66%"><a href="http://sites.google.com/view/yiming-shen"><img src="https://avatars.githubusercontent.com/u/89332218?v=4?s=100" width="100px;" alt="Yiming Shen"/><br /><sub><b>Yiming Shen</b></sub></a><br /><a href="https://github.com/dwzhu-pku/PaperBanana/commits?author=shenyimings" title="코드">💻</a></td>
      <td align="center" valign="top" width="16.66%"><a href="http://blog.sukisq.me"><img src="https://avatars.githubusercontent.com/u/87158944?v=4?s=100" width="100px;" alt="Edom"/><br /><sub><b>Edom</b></sub></a><br /><a href="https://github.com/dwzhu-pku/PaperBanana/commits?author=blessonism" title="코드">💻</a></td>
    </tr>
    <tr>
      <td align="center" valign="top" width="16.66%"><a href="https://github.com/issyuNaN"><img src="https://avatars.githubusercontent.com/u/167730146?v=4?s=100" width="100px;" alt="issyuNaN"/><br /><sub><b>issyuNaN</b></sub></a><br /><a href="https://github.com/dwzhu-pku/PaperBanana/issues?q=author%3AissyuNaN" title="버그 보고">🐛</a></td>
    </tr>
  </tbody>
</table>

<!-- markdownlint-restore -->
<!-- prettier-ignore-end -->

<!-- ALL-CONTRIBUTORS-LIST:END -->

## 라이선스
Apache-2.0

## 인용
이 저장소가 도움이 되셨다면, 다음과 같이 논문을 인용해 주세요:
```bibtex
@article{zhu2026paperbanana,
  title={PaperBanana: Automating Academic Illustration for AI Scientists},
  author={Zhu, Dawei and Meng, Rui and Song, Yale and Wei, Xiyu and Li, Sujian and Pfister, Tomas and Yoon, Jinsung},
  journal={arXiv preprint arXiv:2601.23265},
  year={2026}
}
```

## 면책 조항
이 프로젝트는 Google의 공식 지원 제품이 아닙니다. 이 프로젝트는 [Google 오픈소스 소프트웨어 취약점 보상 프로그램](https://bughunters.google.com/open-source-security)의 대상이 아닙니다.

커뮤니티에 기여하는 것이 목표이므로 현재 상업적 목적으로 사용할 계획이 없습니다. 핵심 방법론은 Google 인턴십 중 개발되었으며, 이러한 특정 워크플로우에 대한 특허가 Google에 의해 출원되었습니다. 이는 오픈소스 연구 노력에는 영향을 미치지 않지만, 유사한 로직을 사용하는 제3자 상업적 응용 프로그램은 제한됩니다.
