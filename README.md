# propensity-score
propensity score matching/weighting 수행 시 주의사항

iptw
→ 전체 인구를 가상의 무작위 실험 환경처럼 만들어서
   “모든 사람이 대사질환이 있다고 가정하면” vs “없다고 가정하면”
   위염 발생률이 얼마나 차이날까? 를 보는 것
 = 전체 모집단 평균 효과 (marginal ATE)

 “실제로는 나이 많고 대사질환 있는 사람이 많은데, 이걸 대사질환 있는 사람과 없는 사람으로만 나눠서
인위적으로 공정하게 비교한 다음 전체적인 경향을 보는 거야.”

---
### Propensity Score Matching

🧠 1:1 matching을 수행했을 경우, 데이터가 '짝지어졌다'는 특성이 있으므로,  
GEE(Generalized Estimating Equations)를 병행하는 것이 권장된다.  

👉 데이터 내 쌍(pair) 간 상관성이 존재하기 때문이다.  
(1:1 Matching에서 GEE 병행은 필수에 가깝고, 1:n matching에서도 권장. IPTW는 불필요)  

---
### Propensity Score Weighting

🧠 IPTW를 수행할 경우, 극단적 가중치는 잘라내거나 다듬어 분석 결과의 왜곡을 막을 필요가 있다.  
극단적 가중치는 전체 추정값을 소수의 샘플에 의존하게 만들고, 표준 오차를 폭발적으로 키운다.  

👉 0.01 < ps <0.99 Trimming  
👉 weight > 10 인 경우 10으로 Truncation  
👉 Stabilized weights + trimming (평균 수준에 맞춰 안정화 후 trimming)  
