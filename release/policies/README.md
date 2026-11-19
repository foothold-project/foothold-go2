> 분류: 배포 묶음 · 정책 (바이트 사본)
> 작성: Claude 세션 (오흥재 지시, go2-lx) · 2026-11-19
> 근거: 팀장 11/19 15:34 「m15500 정책 + AME-2 27000 으로 정책 정본 우선 채택」 · 16:35 「go2 정책 묶음 진행 · Runpod 버킷 경로는 빼고」 (둘 다 lead-lx 전달) · foothold-lab 원본 (아래 표의 커밋) · `AGENTS.md` 1a
> 요지: **m15500 = 실기 후보 걸음 정책** · **AME-2 m27001 = 앱 시뮬용 teacher (실기 아님)**. 둘 다 foothold-lab 원본을 바이트 그대로 옮겼고 (ame2 `contract.json` 의 저장소 경로 칸만 뺌, 아래), contract · 관절 순서 · action scale · PD 는 원본 그대로다
> 상태: 초안 · dev 로 PR (병합은 팀장)

# 정책 묶음

| 묶음 | 무엇 | 원본 (foothold-lab) | 원본 커밋 | 여기 크기 |
|---|---|---|---|---|
| `start-m15500/` | **실기 후보 걸음 정책** · Go2 기본 보행 정책 정본 3 (EX-42 E1F m250 · 누적 15,500 update) · ONNX 지도 켬 · 끔 | `inbox/jay/20261103-policy-handoff/start-m15500-app/` (main) | `29883b0c` | 27 MB |
| `ame2-m27001/` | **앱 시뮬용 teacher (실기 아님)** · AME-2 teacher (model_16999 · 두 학습 평균 · 누적 27,001) · ONNX 하나 | `inbox/lim/20261103-policy-handoff/ame2-m27001/` (가지 `inbox-lim-ame2-m16999` · **PR #878 아직 열림**) | `4f5db23a` | 5.5 MB |

## 대조

```
cd release/policies/start-m15500 && sha256sum -c --ignore-missing SHA256SUMS
cd release/policies/ame2-m27001 && sha256sum -c --ignore-missing SHA256SUMS
```

두 묶음 모든 파일 OK (11/19 15:5x). `SHA256SUMS` 는 원본 파일 그대로라, 여기 옮기지 않은 파일 줄은 `--ignore-missing` 으로 건너뛴다.

| 묶음 | 옮긴 것 | 옮기지 않은 것 (까닭) |
|---|---|---|
| `start-m15500/` | ONNX 둘 · `CONTRACT.md` · `contract.json` · `parity.json` · 시험 벡터 8 · `SHA256SUMS` | `start-m15500.pt` (학습 · 변환용 · lab LFS · 실기는 ONNX) · `CHECKPOINT.md` |
| `ame2-m27001/` | ONNX · `CONTRACT.md` · `contract.json` · `app_onnx.json` · `parity.json` · 시험 벡터 2 · `README.md` · `SHA256SUMS` | `tools/` (만든 도구 · GPL-3 · 배포에 안 씀) · `.gitattributes` (이 폴더 위 `.gitattributes` 가 같은 `* -text`) |

## 실기에 쓰기 전에 알아야 할 것

- **`ame2-m27001` 은 teacher 다.** 선속도 (lin_vel) 를 특권 입력으로 받고, 지도 입력은 시뮬 지형의 참값이다. 원본 문서도 「실기는 student, teacher 는 시뮬」 (팀장 11/03) 이라 적는다. 실기용 student · Mapper 는 아직 없다 (Runpod 백업에 teacher 만 있음 · 석헌 11/19 Mapper 학습 중).
- `start-m15500` 도 실기 송신 전에는 `AGENTS.md` 5a 저수준 관문 (업체 열 사건 판정 · E-stop · 2 인 매달기) 을 지난다.
- **원본과 다른 파일 하나 (팀장 16:35):** `ame2-m27001/contract.json` 에서 `checkpoint.s3_path` 칸을 빼고, 학습 저장본 경로 앞부분 (Runpod 볼륨의 개인 폴더) 을 `<runpod-volume>/` 로 바꿨다. 정책 계산에 쓰는 칸 (입출력 · 관측 · 관절 순서 · scale · PD · 모델) 은 한 글자도 안 바꿨다. 그래서 이 파일 sha256 만 원본과 다르다: 원본 `78f8a9afd3e2c6c5594e1d9c79abbf4d1a993d3c10b8a820cbc0eba19144c0ba` → 여기 `b00ee933ba58dcfbbf4cd12c9af9f8a7cdae6cec2d9a8b73a031b516bc61c3cd` (`SHA256SUMS` 의 그 줄도 바꿈). 원본 경로가 필요하면 foothold-lab 원본을 본다.
- `ame2-m27001/CONTRACT.md` · `parity.json` 의 본문에는 같은 저장본 경로가 남아 있다 (버킷 주소는 없음 · 지시는 contract.json).
- LFS 는 쓰지 않는다 (가장 큰 파일 5.8 MB · GitHub 100 MB 한도 아래).
