# pi-code-analysis-html

**코드 / 아키텍처 분석 결과**를 다이어그램 중심의 시각화 대시보드 HTML 로 만들어 주는 [pi](https://pi.dev) 스킬 패키지.

`make-html` 이 긴 글을 읽히는 *산문 리포트*(커버 → 섹션 → 콜아웃)라면, 이 스킬은 분석 결과를 **도해 중심으로 보여주는 페이지**를 만든다. 파이프라인 플로우 · 모듈 의존성 스택 · **프로그램 관여도 표** · 스탯 타일 · CSS 바 차트 · 카탈로그 그리드 · 개선사항 카드를 따뜻한 에디토리얼 톤(아이보리 캔버스 + 클레이 액센트 + 세리프 디스플레이)으로 렌더한다.

## 구성

```
skills/code-analysis-html/
├── SKILL.md                              # 스킬 진입점 (적용 조건 + 사용 방법)
├── references/design-system.md           # 전체 디자인 규칙 (토큰·컴포넌트·모션·체크리스트)
└── assets/example-influxdb-analysis.html # 완성 산출물 예시 (InfluxDB 백엔드 사용 분석)
```

## make-html vs code-analysis-html

| | `make-html` | `code-analysis-html` |
|---|---|---|
| 콘텐츠 | PRD·가이드·정책 등 장문 문서 | 아키텍처·사용 패턴·코드베이스 분석 |
| 오프닝 | 커버 (eyebrow + 제목 + 메타) | 마스트헤드 + 주제 구조(파이프라인/스파크라인/스탯) |
| 본문 | 산문 섹션 + 콜아웃 + 테이블 | 다이어그램 우선 섹션 + **프로그램 관여도 표(필수)** |
| 본문 폭 | 800px (가독 중심) | 1080px (대시보드) |
| 트리거 | "HTML 보고서로", "리포트 형식으로" | "시각화해줘", "이미지화", "다이어그램으로" |

단일 HTML · 외부 의존성 없음(폰트 CDN 제외) 원칙을 지킨다. Mermaid/Chart.js 같은 차트 라이브러리는 쓰지 않고 순수 CSS + 인라인 SVG + vanilla JS 로 그린다.

## 설치

```bash
# git 저장소에서 설치 (권장)
pi install git:github.com/preinpost/pi-code-analysis-html

# 로컬 클론에서 개발용으로 설치
pi install ~/dev/pi-code-analysis-html
```

특정 버전 고정: `pi install git:github.com/preinpost/pi-code-analysis-html@v0.1.0`

### 업데이트 / 제거

```bash
pi update --extensions     # 최신 커밋으로 갱신 (ref 미지정 시 main 추적)
pi remove git:github.com/preinpost/pi-code-analysis-html
```

## 사용

설치하면 `code-analysis-html` 스킬이 로드된다. 사용자가 분석 결과의 **시각화/다이어그램 렌더링을 명시적으로** 요청할 때만 적용된다.

- "이 분석 시각화해서 보여줘"
- "이미지화해서 정리해줘"
- "다이어그램/플로우차트로 그려줘"
- "이 분석 HTML 대시보드로 만들어줘"

강제 실행:

```bash
/skill:code-analysis-html
```

산문 리포트가 필요하면 `make-html` 을 쓴다. 단순 질문·요약·읽기·표만 필요한 경우에는 적용되지 않는다.

## 라이선스

MIT
