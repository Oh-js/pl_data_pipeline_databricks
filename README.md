# pl_data_pipeline_databricks
메달리온 아키텍처 기반 프리미어리그 데이터 파이프라인 구축
# ⚽ Premier League Data Pipeline & Analytics Platform

Databricks와 Apache Spark를 활용하여 프리미어리그(EPL) 경기 데이터를 정제하고, 가설 검증형 데이터 마트(Gold 레이어)를 구축하는 데이터 엔지니어링 프로젝트입니다. 

## 📂 Project Structure & Files

본 레포지토리는 데이터 파이프라인 구축을 위한 소스 코드와 분석용 데이터셋으로 구성되어 있습니다.

- `test pl data 2026-05-22...ipynb`: Databricks 가설 검증형 분석 메인 노트북 (16-17 vs 25-26 토트넘 종단 분석 SQL 포함)
- `pl_1617_data_fromFDuk.csv`: Football-Data.co.uk에서 추출한 토트넘의 찬란했던 16-17 시즌 원천 데이터 (Bronze)
- `test_PL_data.csv`: 위기의 25-26 시즌 현재 프리미어리그 경기 데이터셋
- `test_pl_data_2.csv`: 데이터 무결성 검증 및 백업용 프리미어리그 추가 데이터셋

## 🛠️ Tech Stack
- **Platform:** Databricks (Unity Catalog)
- **Engine:** Apache Spark (PySpark, Spark SQL)
- **Storage:** Delta Lake (Medallion Architecture)
- **Language:** Python, SQL

## 🏗️ Data Architecture (Medallion)
1. **Bronze Layer:** Raw CSV 파일 로드 및 스키마 유추 (`test_PL_data.csv`, `test_PL_data_1617.csv`)
2. **Silver Layer:** 데이터 정제, 결측치 처리, 일관된 날짜 포맷팅 및 24개 핵심 전술 컬럼 추출 (`premier_league_silver`)
3. **Gold Layer:** 특정 비즈니스 가설 검증 및 벤치마킹을 위한 다차원 분석 마트 구현

---

## 📊 분석 케이스 스터디 1탄: 토트넘 미스터리 종단 분석
> **가설:** "과거 화이트 하트 레인 시절(16-17)의 압도적인 홈 성적과 새 스타디움(25-26)에서의 홈 부진 원인은 경기장 규격 및 그에 따른 전술적 부작용에 기인할 것이다."

### 🔍 핵심 지표 대조 테이블
| 시즌 (구장) | 경기 수 | 경기당 코너킥 | 경기당 피슈팅 | 파울 10개당 카드 수 | 홈 승점 |
| :--- | :---: | :---: | :---: | :---: | :---: |
| **2016-17 (White Hart Lane)** | 19 | **7.79개** | **7.89개** | **1.29개** | **53점** |
| **2025-26 (Tottenham Hotspur Stadium)** | 19 | 6.16개 | 11.53개 | 2.29개 | 15점 |

### 💡 데이터 기반 인사이트 요약
1. **좁은 구장 전술의 지표 증명:** 16-17 시즌 화이트 하트 레인은 콤팩트한 공간 덕분에 전방 압박 효율이 극대화되어 상대 슛을 경기당 7점대로 완벽히 묶었으며, 치열한 박스 인근 전투로 코너킥 빈도가 매우 높았습니다.
2. **넓어진 구장과 배후 공간 노출:** 경기장이 대형화된 현재, 수비진이 감당해야 할 공간이 넓어지면서 배후 돌파를 허용 예상함. 이를 저지하기 위한 무리한 파울이 늘어나며 **파울 대비 카드 수집률이 약 2배 가까이 폭증(1.29 -> 2.29)**하는 부작용을 데이터로 입증했습니다.
