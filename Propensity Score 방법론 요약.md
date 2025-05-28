✅ **Propensity Score 방법론 요약**

---

### ✅ Propensity Score (성향 점수)

: 공변량(covariates)을 바탕으로 각 개인이 treatment(처치)를 받을 확률을 수치화한 값

- 일반적으로 **로지스틱 회귀 (Multivariable Logistic Regression)** 로 추정
- 단순히 회귀모델에 공변량을 포함하여 조정하는 방식은 **모델의 형태에 따라 결과가 좌우될 수 있음**
- 반면 성향 점수는 개별 처치 확률이라는 **요약 통계량**을 통해, 이후 매칭 또는 가중치 부여 방식으로 **공변량 구조를 재조정**

---

### ✅ 언제 covariate adjustment (회귀) vs PS method (PSM/PSW)가 유리한가?

| 상황 | 추천 방법 | 이유 |
|------|------------|------|
| 표본 수가 충분하고, PS 분포에 overlap이 잘 되는 경우 | **PS 기반 방법** (PSM/PSW) | 공변량 불균형을 정량적으로 보정 가능하며, ATT/ATE 등 다양한 해석 가능 |
| 공변량 수가 적고 imbalance가 작을 경우 | **회귀모델 조정** | 구현이 간단하고, 통계적 검정과 해석이 명확함 |
| 특정 소집단의 효과를 추정하고 싶은 경우 | **PSM (ATT 또는 ATO)** | 유사한 특성을 가진 사람들 간의 비교가 가능 |
| 전체 모집단의 평균 효과를 추정하고 싶은 경우 | **IPTW (ATE)** | 가중치를 통해 전체 분포를 재구성하여 ATE 추정 가능 |

> **참고:** Overlap이 잘된다는 것과 covariate imbalance가 작다는 것은 유사하지만, 정확히 같지는 않음.  
> - *Covariate imbalance*는 공변량 자체의 분포 차이  
> - *Overlap*은 PS 분포에서의 공통 영역 존재 여부

---

### ✅ Propensity Score Matching (PSM)

: 유사한 성향 점수를 가진 treatment/control 그룹을 짝지어 outcome 차이를 추정

- 일반적으로 **처치군(treated)을 anchor로** 비처치군을 매칭
- 매칭되지 않은 샘플은 제외되므로 **표본 손실 발생 가능**
- 결과적으로 **ATT (Average Treatment effect on the Treated)** 를 추정
  > "처치를 받은 사람에게서, 처치를 받지 않았을 때 나타났을 효과"

- 만약 매칭된 표본이 covariate overlap 영역에 집중된다면, 해석은 **ATO (Average Treatment effect on the Overlap)** 에 가까움
  > "covariates가 유사한 사람들 간의 평균 효과"

---

### ✅ Propensity Score Weighting (IPTW)

: 추정된 성향 점수의 **역수를 가중치로 부여**하여, treatment/control 간 공변량 분포를 인위적으로 재구성

- 전체 모집단 평균 효과인 **ATE (Average Treatment Effect)** 추정에 적합  
  > "모든 사람이 처치를 받았다면 vs 받지 않았다면, 전체 outcome은 얼마나 달랐을까?"

- **모든 표본을 사용** → 표본 손실 없음
- 단, **성향 점수가 0 또는 1에 가까운 극단값을 가진 샘플**의 경우 inverse weight가 커져
  - **분산이 커지고**
  - **bias가 유입될 위험**이 존재

#### 👉 극단값에 대한 대처
- **Trimming**: PS < 0.01 또는 > 0.99 제거
- **Truncation**: IPTW 가중치의 상한/하한 설정
- **Stabilized Weight**: 분산 안정화를 위해 사용되는 보정 방법

---

### 📌 요약 비교표

| 방법       | 조정 방식      | 표본 손실 | 해석 대상      | 대표 효과      |
|------------|----------------|------------|----------------|----------------|
| 회귀 조정  | 모델 내에서 조정 | 없음       | 전체 모집단     | 조건부 ATE      |
| Matching   | 유사한 표본만 비교 | 있음       | 처치군 또는 overlap | ATT 또는 ATO |
| IPTW       | 가중치로 전체 재조정 | 없음       | 전체 모집단     | ATE             |

---

### 🛠 용어 설명

- **ATE** (Average Treatment Effect): 전체 모집단에서의 평균 처치 효과
- **ATT** (Average Treatment Effect on the Treated): 처치를 받은 사람들에서의 평균 효과
- **ATO** (Average Treatment Effect on the Overlap): 성향 점수가 유사한 샘플들 사이의 평균 효과

---

### 🚧 잘못된 해석 방지를 위한 주의사항

- **Propensity Score(PS)** 는 교란(confounding)을 제거하지 않음  
  → 단지 처치 확률을 요약한 수단일 뿐

- PS 기반 분석 후에도 **공변량 균형(balance) 진단 필수**
  - 주요 진단법:  
    - `SMD plot` (Standardized Mean Difference)  
    - `Overlap plot` (PS 분포 겹침 확인)  
    - `Love plot` (보정 전후 비교)

- **분석 설정값(caliper, trimming, matching ratio, stabilized weight)** 은 결과에 중대한 영향
  - caliper 좁으면 bias ↓, 표본 손실 ↑  
  - stabilized weight 사용 시 분산 안정화 가능

- **PSM은 matched pair 비교이므로**, outcome 분석 시 **GEE** 등 상관구조를 고려한 모델이 필요할 수 있음  
  → 특히 반복 측정 데이터나 군집 구조가 있을 경우 더욱 중요

- **IPTW에서 유의한 결과가 나와도**, 공변량 균형이 확보되지 않으면 해석 불가능  
  → 반드시 사후 진단 동반 필요

- **Sensitivity analysis**를 통해 **미측정 교란(unmeasured confounding)에 대한 강건성 평가** 필요
  - 예: `E-value`, `Rosenbaum bounds` 등

> 결과 해석의 신뢰도는 단순 통계적 유의성보다, **모델의 적절성과 사후 검증의 충실도**에 달려 있음

---

### ✅ 마무리 문장 제안

> Propensity Score 기반 방법은 관찰자료 내에서 **교란변수를 조정하고 인과효과를 추정**하는 유용한 도구이나,  
> 전제가 충족되지 않으면 **왜곡된 해석**을 초래할 수 있으므로, 모델 적합과 사후 진단을 철저히 수행해야 한다.
