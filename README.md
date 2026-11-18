# foothold-go2: 트랙 A 시뮬레이션 · 트랙 B 실기 자율주행

FOOTHOLD 의 실개발 레포. 시뮬 학습(Isaac Lab)부터 실기 배포까지의 코드가 산다.
문서·회의록은 [foothold-lab](https://github.com/foothold-project/foothold-lab),
공개 사이트는 https://foothold-project.vercel.app

## 지금 이 저장소의 일 (2026-11-18 갱신)

이 저장소는 이제 **실기 Go2 X · 저수준 제어 · 최종 배포**를 맡는다. 시뮬 학습 실험은 `foothold-lab`,
앱 코드는 `foothold-app` 에서 한다. 세 저장소의 역할과 실기 안전선은 [AGENTS.md](AGENTS.md) 1a · 5a · 5b 절.

| 무엇 | 어디 |
|---|---|
| 실기 장비 | Go2 X · Jetson Orin NX 16GB · Intel RealSense D435i · Unitree L2 |
| 정책 정본 (시뮬) | `foothold-lab` 의 정본 3 (`start-m15500`) 묶음 · 여기에는 sha 대조한 사본만 |
| 앱 연결 | `foothold-app` 의 `contracts/` 약속 · 이 저장소는 앱 태그를 가리키기만 함 |
| 실기 측정 원본 · 인수인계 | `foothold-lab` (비공개) |

아래 환경 고정값과 성능 기준선은 8 월 시뮬 트랙 시절의 기록이다. 지금 학습 환경은 `foothold-lab` 기준이다.

## 환경 고정값: 바꾸려면 팀 합의

| 항목 | 값 | 왜 고정인가 |
|---|---|---|
| Isaac Sim | **5.1.0.0** | pip 설치 기준 |
| Isaac Lab | **v2.3.2 태그** (`37ddf62`) | 최신 안정판. 기본 브랜치는 베타이고 험지 미지원 |
| Python | **3.11** | Isaac Sim 이 cp311 wheel 만 제공 |
| PyTorch | **2.7.0+cu128** | 기본 wheel 은 CPU 전용 · cu128 인덱스에서 먼저 설치 |
| rsl_rl | **3.1.2** | Isaac Lab v2.3.2 가 핀 고정 |
| 물리 백엔드 | **PhysX** | Newton 백엔드는 험지 로코모션 미지원 |

전체 설치 절차·함정 6종: [setup 가이드](https://foothold-project.vercel.app/setup.html)
접속·운영(TensorBoard·프로세스 정리): [팀원 가이드](https://foothold-project.vercel.app/team-access.html)

## 실측 성능 기준선 (AI-WS01 · RTX 5080 ×1 · 2026-08-05)

| 항목 | 값 |
|---|---|
| 험지 4096 env 학습 속도 | **21,248 ~ 21,385 steps/s** |
| iteration 1회 | 4.4~4.6초 (수집 4.5s + 학습 0.12s) |
| 표준 1,500 iteration | **약 113분** |

## 브랜치 전략

`feature/작업명-이름` → PR → `dev` → PR → `main`. **main 직접 push 금지.**
상세와 도식 = [협업 규칙 §5](https://foothold-project.vercel.app/collab.html) · 팀 전 규칙의 단일 진실.

## 실험 규율: 두 가지 (팀 공통 규칙 · [COLLAB §13](https://foothold-project.vercel.app/collab.html))

**① 한 번에 하나만 바꾼다.** 두 개 바꾸면 어느 쪽 효과인지 모른다.
**② 실험은 [EXPERIMENTS.md](EXPERIMENTS.md) 에 남긴다.** 기준 곡선과 대조 없는 실험은 안 한 것과 같다.

## ⚠️ 체크포인트(.pt)를 커밋하지 마라

`.gitignore` 가 막고 있다. 한 번 커밋되면 히스토리에서 지울 수 없고 레포가 GB 단위로 부푼다.
체크포인트는 워크스테이션 `logs/` 와 팀 스토리지(추후 확정)에 보관한다.
