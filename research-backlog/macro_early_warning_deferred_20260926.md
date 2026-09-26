# DEFERRED RESEARCH — Macro / Market Crash Early-Warning Module

STATUS: DEFERRED_UNTIL_CURRENT_PROJECT_COMPLETE
RECORDED_AT_KST: 2026-09-26T23:24:00+09:00
PROJECT: ETF_STOCK_PROJECT
IMPLEMENTATION_STATUS: NOT_STARTED
WORKER_IMPACT: NONE
DEPLOYMENT_STATUS: NOT_DEPLOYED
RULE_ACTIVATION_STATUS: NOT_ACTIVE

## 목적
현재 주식·ETF 분석 프로그램 완성 후 별도 연구 모듈로 검토한다.
목표는 특정 급락 날짜를 단정적으로 예측하는 것이 아니라,
수개월~수일에 걸친 시장 위험 환경 악화를 조기에 감지하는 것이다.

## 설계 원칙
- 기존 주식/ETF 계산기, 기술적 매수점수, 재무계산을 직접 변경하지 않는다.
- 별도 sidecar/독립 모듈로 연구하고 마지막에 GPT 입력 패킷으로만 연결하는 방식을 우선 검토한다.
- 단일 지표를 매수/매도 신호로 사용하지 않는다.
- 고정 임계값은 검증 전 활성 규칙으로 사용하지 않는다.
- 과거 최종값이 아니라 당시 이용 가능했던 데이터와 발표시각/수정이력을 고려한 블라인드·PIT 검증을 우선한다.
- 거짓경보율, 놓친 급락, 선행기간, 최대낙폭, 비용 후 성과를 함께 평가한다.

## 후보 시간층

### 장기 선행 환경 (대략 수개월~18개월)
- 10Y-3M yield spread
- 10Y-2Y yield spread
- near-term forward yield information
- NFCI / ANFCI
- Initial Jobless Claims 및 4주 평균
- Sahm Rule은 선행보다 확인 성격으로 별도 취급

### 중기 금융/거시 스트레스 (대략 수주~수개월)
- High Yield credit spread
- BBB corporate credit spread
- WTI: 절대가격보다 5/20/60일 변화율 및 충격속도
- 원유 선물곡선 backwardation / contango
- DXY
- USDJPY
- 기대인플레이션
- 연준/시장 유동성 관련 지표

### 단기 시장 스트레스 확인 (대략 1~10거래일)
- VIX / VIX3M term structure
- MOVE index
- market breadth: 200DMA 상회비율 + A/D 또는 신저가 계열
- credit-spread change velocity
- VVIX / SKEW / Put-Call은 중복효과 검증 후 선택

## 초기 검증 대상 사건 정의 예시
- 향후 5거래일 -5% 이하
- 향후 10거래일 -7% 이하
- 향후 20~60거래일 최대낙폭 -10% 이하
- 6~24개월 내 경기침체/대형 약세 환경

위 임계값은 연구용 예시이며 활성 투자규칙이 아니다.

## 검증에서 반드시 구분할 것
- 경기수요 상승에 따른 유가 상승 vs 공급충격 유가 상승
- 경기둔화에 따른 유가 급락 vs 정상화
- 금리곡선 수준 vs 변화속도
- 실제 관측 0 vs MISSING
- 발표일/수정일/당시 이용가능시각
- 선행경보 vs 동시확인 vs 후행확인
- 연구 성공 vs 실제 투자성과 승인

## 기존 프로젝트와 통합 원칙
독립 출력 예시:
- long_horizon_macro_risk
- medium_horizon_financial_stress
- short_horizon_market_stress
- supporting_evidence
- contrary_evidence
- data_as_of
- verification_state

기존 매수점수에 자동 가산하지 않는다.
GPT가 시장 배경/위험강도 해석의 보조근거로 사용한다.

## 재개 조건
현재 주식·ETF 프로그램의 승인된 기능 범위가 완성되고,
최종 통합 검증 및 운영 구조가 정리된 뒤 별도 연구 작업으로 재개한다.

재개 시 먼저 수행:
1. 기존 프로그램/교과서에 이미 존재하는 거시·위험지표와 중복 대조
2. 공식/안정적 데이터 소스 확정
3. 지표별 데이터 빈도와 실제 공개시각 정의
4. PIT/블라인드 검증 설계
5. 지표별 단독 성능보다 시간층 조합의 추가효과 검증
6. 효과가 없는 중복 지표는 제거
7. 기존 프로그램 연결은 마지막에 최소 변경으로 수행

## 비고
이 문서는 보류 연구 기록이다.
코드 구현, Worker 병합, 운영 배포, 투자규칙 활성화 또는 수익성 검증 완료를 의미하지 않는다.
