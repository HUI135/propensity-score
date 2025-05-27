# propensity-score
propensity score matching/weighting에 대한 고찰

---
### Propensity Score Matching

🧠 1:1 matching을 수행했을 경우, 데이터가 '짝지어졌다'는 특성이 있으므로,  
GEE(Generalized Estimating Equations)를 병행하는 것이 권장된다.  

👉 데이터 내 쌍(pair) 간 상관성이 존재하기 때문이다.  
(1:1 Matching에서 GEE 병행은 필수에 가깝고, 1:n matching에서도 권장. IPTW는 불필요)  

---
### Propensity Score Matching

🧠 IPTW를 수행할 경우, 극단적 가중치는 잘라내거나 다듬어 분석 결과의 왜곡을 막을 필요가 있다.  
극단적 가중치는 전체 추정값을 소수의 샘플에 의존하게 만들고, 표준 오차를 폭발적으로 키운다.  

👉 0.01 < ps <0.99 Trimming  
👉 weight > 10 인 경우 10으로 Truncation  
👉 Stabilized weights + trimming (평균 수준에 맞춰 안정화 후 trimming)  
