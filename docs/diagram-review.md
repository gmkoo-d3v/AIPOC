# 기존 이미지 검토와 재작성 근거

2026-11-01, 현재 작업 트리의 소스와 Git 이력을 기준으로 확인했습니다. 기존 6개 이미지의 문구·구조를 검토한 뒤 기능 인포그래픽 PNG 6종과 상세 PlantUML 참조도를 다시 작성했습니다. 기능 그림은 이미지 생성 도구로 제작했으며, 앱의 생성·임베딩 API 호출이나 성능 재측정은 하지 않았습니다.

| 기존 이미지 주제 | 확인 결과·수정 | 새 원본 |
|---|---|---|
| Actor-Critic 루프 | 고정 최대 5회 표기를 제거. 기본 5회, 조건부 +3회, 첫 90점 이상 뒤 개선 보너스, 85~89점 사용자 확인, 최고 결과 복원·종료 경로 표시 | [actor-critic.puml](diagrams/actor-critic.puml) |
| 시스템 아키텍처 | UI와 Application 제어가 같은 파일임을 반영. 도메인·공통 지원·저장소 구분, 생성 모델과 임베딩 provider 구분, anti 직접 호출 반영 | [architecture.puml](diagrams/architecture.puml) |
| 대상 최적화 아키텍처 | 추상적인 AI Brain을 실제 UI·Optimizer·Actor-Critic·참고 예시·가드레일·결과/이력 경로로 교체 | [target-optimization.puml](diagrams/target-optimization.puml) |
| 타깃 페르소나 | 4종 구성은 일치. 대학생·실무자 정책과 전문가의 정밀성 유지 명시. 모두 쉬운 문서로 바뀐다는 해석 제거 | [personas.puml](diagrams/personas.puml) |
| Hybrid RAG 검색 | 0.6/0.4는 코드와 일치. Target Optimizer의 Best Practice 예시 검색으로 범위 한정. 정규화·후보 필터·기본 top 2 표시 | [hybrid-search.puml](diagrams/hybrid-search.puml) |
| Multi-provider Vector DB | provider별 컬렉션은 일치. 모델 의존적인 차원 고정값과 차원 문제 완전 해결 주장을 제거 | [provider-isolation.puml](diagrams/provider-isolation.puml) |

슬라이드용 [architecture-slide.puml](diagrams/architecture-slide.puml)은 상세 구조도와 같은 구현을 간추린 버전입니다. 기존 Mermaid 그림과 구조가 부정확한 인포그래픽은 README에 재사용하지 않았습니다.

## 코드 근거

- 진입·실행 제어: [`main.py`](../documind/app/main.py), [`analy_app.py`](../documind/app/views/analy_app.py). `main.py`가 `analy_app.py`를 직접 실행합니다.
- 루프 정책·상태: [`orchestrator.py`](../documind/actor_critic/orchestrator.py), [`config.py`](../documind/llm/config.py). 첫 통과 시 추가 개선, 이후 점수 하락 시 복원. 마지막 사용자 확인에서 남은 시도가 없는 경우의 동작도 원본에 표시했습니다.
- 최적화·검사: [`optimizer.py`](../documind/target_optimizer/optimizer.py), [`guardrail.py`](../documind/target_optimizer/guardrail.py). rollback에서 검사 생략과 fallback 재작성 후 재검사가 없는 경로는 현재 구현 동작입니다.
- 예시 검색: [`best_practice_manager.py`](../documind/utils/best_practice_manager.py). 정규화한 점수를 결합하며 같은 target 후보가 없으면 필터를 풀고 재조회합니다. 품질 필터 이후 빈 결과가 되면 다시 전체 조회하지는 않습니다.
- 컬렉션·임베딩: [`best_practice_manager.py`](../documind/utils/best_practice_manager.py), [`embeddings.py`](../documind/ai/embeddings.py). provider 이름으로 컬렉션을 분리하며 모델·차원은 컬렉션 키에 포함되지 않습니다.
- 페르소나: [`personas.py`](../documind/target_optimizer/personas.py). 일반인·대학생·실무자·전문가의 문체·어휘·구조·평가 기준을 제공합니다.
- 조회 범위: [`analy_app.py`](../documind/app/views/analy_app.py)의 탐색기와 [`db.py`](../documind/utils/db.py)의 이력 조회에 역할·소유자 필터가 있습니다. [`rag/index.py`](../documind/rag/index.py)의 인메모리 검색에는 소유자 없는 공개 문서를 허용하는 경로도 있으므로 전체 RAG 권한을 동일한 규칙으로 설명하지 않습니다.

## 기여 귀속

[423020e](https://github.com/gmkoo-d3v/AIPOC/commit/423020e)에 초기 Actor-Critic 루프가 존재합니다. [76aaf60](https://github.com/gmkoo-d3v/AIPOC/commit/76aaf60)에서 gmkoo-d3v가 상태 모델·generator·사용자 결정·복원·검색 연결을 확장했고, [30f4f66](https://github.com/gmkoo-d3v/AIPOC/commit/30f4f66)에서 자동 진행을 추가했습니다.

이 문서는 정적 코드·이미지 검토 결과입니다. 내부 Critic 점수나 고정 검색 가중치를 실측 성능 성과로 해석하지 않습니다.
