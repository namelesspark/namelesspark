# namelesspark

- 국립금오공과대학교 컴퓨터공학부 컴퓨터공학전공 4학년
- 백오더 예측(180만 건 중 0.67%)에서 시작해, 현재는 **엣지에서 클라우드까지 이어지는 데이터 파이프라인**과 **제조 설비 고장 예측**으로 관심을 넓히는 중임
- 임베디드에 가까운 제어부터 이를 서비스로 연결하는 백엔드까지, 한 흐름으로 다루는 엔지니어를 지향함

---

## 🛠 Tech

**주력** &nbsp;`C` `C++` `Python`
**개발** &nbsp;`FastAPI` `Flask` `React` `Firebase`
**클라우드 / 인프라** &nbsp;`AWS EC2` `AWS S3` `boto3` `GitHub Actions`
**환경** &nbsp;`Linux` `Raspberry Pi` `ESP32` `Git` `ffmpeg`

---

## 🚧 진행 중인 프로젝트

### 대피 취약계층 실내 잔류 판단 시스템 — Wi-Fi CSI 기반
> 2026 한이음 드림업 (IITP) · 팀 이게외않되 (4인) · 2026.04 ~ 10 · **AWS 클라우드 / 데이터 파이프라인 담당**

카메라·웨어러블 없이 Wi-Fi CSI만으로 실내 재실·움직임을 판단하고, 재난 경보 이후 잔류 가능성을 산정하는 시스템임.
`ESP32-S3 → Raspberry Pi Gateway → FastAPI Model Server(EC2) → Web Dashboard` 파이프라인 중 **클라우드 계층**을 맡음.

- **추론 경로와 저장 경로를 분리**하여 S3 업로드 실패가 실시간 추론을 중단시키지 않도록 설계하였음
- **로컬 Spool 기반 장애 허용 구조**를 적용하여 HTTP/S3 전송 실패 payload를 보존 후 재전송, CSI 데이터 손실을 방지하였음
- **저장 단위를 이원화**하였음 — Raw CSI 60초 `JSONL.gz` / 전처리 payload 5초 `JSON.gz`, 날짜 파티셔닝 적용
- status snapshot에서 Raw matrix와 credential을 제외하여 민감 데이터 노출을 차단하였음
- **GitHub Actions 기반 EC2 자동 배포 파이프라인**을 직접 구성하였음 — main 브랜치 push 시 SSH 배포, 의존성 설치 및 systemd 서비스 재시작까지 자동화 (Model Server / Dashboard 2개 워크플로)
- 5개 공간 · 11개 세션 · **CSI 패킷 105만 개** 규모 데이터셋의 저장 구조를 설계하였음

`AWS EC2` `Amazon S3` `boto3` `FastAPI` `Python` `GitHub Actions`

---

### LLM 기반 학사 행정 통합 질의응답 챗봇 (RAG)
> 2026학년도 AX 기반 역량 강화 프로젝트 (금오공대 AX교수학습센터) · **풀스택 개발 담당** · 착수 예정

학칙·장학·전과·졸업 규정이 공지사항과 부서별 PDF/HWP에 파편화되어 있어, 학생은 문서를 직접 뒤지고 행정실은 반복 문의에 시달리는 문제를 다룸.
규칙 기반 챗봇을 건너뛰고 **RAG로 환각을 통제하면서 학적 데이터와 연동된 개인화 답변**을 생성하는 것이 목표임.
데이터 파이프라인 · 검색 · Agent · 백엔드 · 프론트엔드 전 영역을 단독 개발함.

- **데이터 엔지니어링** — 학칙의 조·항·목 구조와 표를 보존하는 계층적 파싱 후 Vector DB 구축
- **하이브리드 검색** — BM25 키워드 매칭 + 밀집 검색 결합, Re-ranker로 컨텍스트 우선순위 정렬
- **학적 연동** — 학생 프로필을 자동 호출해 검색 결과와 대조·연산하는 Agentic 루프
- **경량화** — 학내 인프라 구동을 위한 7B~13B sLLM 검토, 소스 문서 외 답변 금지 제약으로 환각률 통제
- 검증 지표 — 빈출 질문 50~100개 기준 문서 참조율(Retrieval Recall) 및 답변 정확도 정량 측정

`Python` `RAG` `Vector DB (Chroma / FAISS)` `BM25` `sLLM`

---

### 설비 고장 예측 AI 데이터 분석 — POSCO K-뉴딜
> 2026.07 ~ 진행 중 · [posco-pdm-academy](https://github.com/namelesspark/posco-pdm-academy)

예측 정비(Predictive Maintenance) 관점의 설비 데이터 분석 과정임.
사후 정비·예방 정비의 한계를 넘어, **데이터로 고장 신호를 미리 잡아 정비를 계획하는 방식**을 다룸.

- `수집 → 정리 → 분석 → 탐지 → 판단` 파이프라인을 Python으로 구현하는 과정을 따라가는 중임
- 진행: Python 환경·자료형·연산자 → 데이터 처리 → 이상 탐지 → 고장 예측

`Python` `Pandas` `설비 데이터`

---

## 📌 Projects

### [BISKIT POINT](https://github.com/namelesspark/biskit-point-platform) — AI 기반 통합 학습 플랫폼
> DX/AX 역량 강화 교내대회 **우수상** (15팀 중) · 2025.11 ~ 2026.01 (11주) · 풀스택 리드 (기여도 90%)

YouTube / 파일 업로드 / 오프라인 강의를 하나의 학습 세션 구조로 통합하고, 타임스탬프 기반 AI 퀴즈와 맥락 인식 튜터 챗봇을 붙인 플랫폼임.

- **실시간 STT 누적 비용을 1/10로 절감**하였음 — 오디오 청크 분할 전사 및 호출 구조 재설계
- 서로 다른 3개 학습 모드의 세션 스키마를 단일 구조로 통일해 기능 확장 비용을 낮추었음
- 시스템 설계 · 프론트/백엔드 · AI 연동 · 배포까지 단독 수행하였음

`React 18` `Flask` `Firebase` `OpenAI GPT / Whisper` `YouTube API`

---

### [백오더 예측](https://github.com/namelesspark/deep-learning-term-project) — 극단적 불균형 데이터 분류
> 딥러닝응용 (산업공학전공) · 모델 설계 담당

180만 건 중 **0.67%만이 백오더 레이블**인 극심한 불균형 데이터를 다루었음.
정상 케이스가 99% 이상이라 단순 정확도가 무의미하여, **PR-AUC와 Recall 중심으로 평가**하며 희귀하지만 놓치면 손실이 큰 사건을 잡는 방법을 다루었음.

- 전처리부터 파생변수 설계, 모델 비교(XGBoost / MLP 등), threshold 조정까지 전 과정을 수행하였음
- 설비 고장 예측 또한 정상 가동이 대부분이고 고장은 드문, 구조적으로 동일한 문제라고 보고 있음

`Python` `XGBoost` `PyTorch` `scikit-learn`

---

### FireDroneCar — 자율주행 화재 진압 로봇
> 임베디드 시스템 (컴퓨터공학전공) 수업 프로젝트

Raspberry Pi 기반 Ackermann 조향 차량으로 화재 지점을 탐지·접근해 진압하는 프로젝트임.
센서 입력 → 제어 로직 → 액추에이터로 이어지는 실시간 제어 루프를 직접 구현하였음.

`C` `Python` `Raspberry Pi`

---

### 인센티브 브레인 — STT 기반 강의 분석 플랫폼
> POSTECH Mini-TeX-Corps **우수상** · 3인팀

린 스타트업 방법론 기반 창업 경진대회임.
**고객 인터뷰 14명**(교수 9 / 학생 4 / 센터 팀장 1)을 직접 수행하고, **3차 피봇**을 거쳐 B2B 모델로 최종 발표하였음.
기술이 아니라 문제부터 검증하는 과정을 배운 경험임.

`Value Proposition Canvas` `Customer Discovery` `Business Thesis`

---

## 📫 Contact

- Email: jade.lake8852@gmail.com
