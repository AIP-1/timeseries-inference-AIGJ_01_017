# 시계열 데이터 추론 과제 (AIGJ_01_017)

<!-- 수정 기록
- 2026-11-03 처음 작성: 과제 1~3 제출 (과제 4~8은 진행 예정)
-->

교육장 실습실·회의실 센서(CO₂·온도·습도) 데이터를 Jetson Orin Nano(aip-1)에서 모으고, 이해하고, 예측한다.
과제별 문서는 spec-kit 순서(spec → plan → tasks → 구현 → 보고서)로 `specs/` 아래에 있다. 전체 계획과 진행 상태는 [ROADMAP.md](ROADMAP.md)에 있다.

## 제출 현황

| 과제 | 내용 | 상태 | 보고서 | 코드 |
|---|---|---|---|---|
| 1. 데이터 수집 체계 | 운영진 InfluxDB → aip-1 InfluxDB 버킷 `jiwon` 상시 수집 | 완료 | [report.md](specs/001-data-collection/report.md) | [collector/](specs/001-data-collection/collector/) |
| 2. 데이터 이해와 기준선 | 정제 규칙, 5분 칸 표, 기간 나누기, 기준선 성능표 | 완료 | [report.md](specs/002-baseline/report.md) | [analysis/](specs/002-baseline/analysis/) |
| 3. 60분 예측 (심화 B) | GRU/LSTM/TCN × 과거 1·2·4시간, 일정 정보, 공간 일반화 | 완료 (온도 통과, CO₂ 미달) | [report.md](specs/003-forecast/report.md) | [code/](specs/003-forecast/code/) |
| 4~8 | 이상 탐지, 상태 추정, 사전학습 모델, 엣지 최적화, 상시 서비스 | 진행 예정 | — | — |

## 과제 3 핵심 결과 (시험 기간 2026-10-01 ~ 10-21, 60분 뒤 MAE)

| | 최종 모델 (TCN, 과거 2시간) | 현재값 유지 | 결과 |
|---|---|---|---|
| 실습실 CO₂ | 38.24 ppm | 34.09 | 미달 |
| 회의실 CO₂ | 52.71 ppm | 63.03 | 미달 (+16.4%, 기준 30%) |
| 실습실 온도 | 0.103 ℃ | 0.14 | 통과 |
| 회의실 온도 | 0.140 ℃ | 0.15 | 통과 |

원인 분석(10월 수업 인원 17명으로 학습 때 본 적 없는 조건)과 실험 과정은 [report.md](specs/003-forecast/report.md), [실험-기록.md](specs/003-forecast/실험-기록.md), [troubleshooting.md](specs/003-forecast/troubleshooting.md)에 있다.

## 측정 조건 (공통)

- 데이터: 실데이터, 2025-09-03 ~ 2026-10-21 (한국 시각)
- 기간: 학습 2025-09-03 ~ 2026-08-31, 검증 2026-09, 시험 2026-10-01 ~ 10-21
- 장비: Jetson Orin Nano 8GB (aip-1). 과제 2 분석은 Mac, 과제 3 학습은 Jetson GPU

## 폴더 구조

```
README.md                   이 문서
ROADMAP.md                  과제 1~8 전체 계획과 상태
.specify/memory/constitution.md   공통 원칙
specs/001-data-collection/  과제 1 (spec, plan, tasks, report, collector/)
specs/002-baseline/         과제 2 (spec, plan, tasks, report, notes, troubleshooting, analysis/, figures/)
specs/003-forecast/         과제 3 (spec, plan, tasks, report, 실험-기록, troubleshooting, 결정-가이드, code/)
```

접속 토큰·비밀번호는 저장소에 넣지 않았다 (각 장비의 `.env`에만 있음).
