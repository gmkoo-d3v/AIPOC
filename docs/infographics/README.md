# 기능 설명용 인포그래픽

2026-11-01, 기존 6개 이미지의 설명을 현재 코드와 대조하고 이미지 생성 도구로 새로 제작했습니다. 기능을 쉽게 소개하는 개요이며 실제 화면·실행 결과·벤치마크를 대신하지 않습니다. 상세 호출·종료 분기는 [PlantUML 참조도](../diagrams/)를 기준으로 확인합니다.

| 이미지 | 설명 | 구현 참조 |
|---|---|---|
| [system-overview.png](system-overview.png) | UI·기능·모델·저장소의 역할 | [architecture.puml](../diagrams/architecture.puml) |
| [target-optimization.png](target-optimization.png) | 원문·대상·참고 예시·재작성·결과 흐름 | [target-optimization.puml](../diagrams/target-optimization.puml) |
| [actor-critic.png](actor-critic.png) | 생성·평가·개선과 사용자 결정 | [actor-critic-detail.puml](../diagrams/actor-critic-detail.puml) |
| [personas.png](personas.png) | 일반인·대학생·실무자·전문가별 표현 정책 | [personas.puml](../diagrams/personas.puml) |
| [hybrid-search.png](hybrid-search.png) | 최적화 우수사례의 의미·키워드 검색 결합 | [hybrid-search.puml](../diagrams/hybrid-search.puml) |
| [provider-isolation.png](provider-isolation.png) | Best Practice provider별 컬렉션 | [provider-isolation.puml](../diagrams/provider-isolation.puml) |

Actor-Critic에서 최초 통과 기준 도달은 첫 번째 평가에 한정되지 않습니다. 85~89점 사용자 확인은 자동 진행 여부와 남은 시도에 따라 달라집니다. 최고 결과 복원은 보너스 개선 이후의 분기입니다. 이미지에는 대표 정책을 요약했고 예외·종료 경로는 상세도에 있습니다.

Hybrid의 0.6/0.4와 품질 기준 92점은 현재 구현값입니다. 최적 가중치나 정확도 향상 측정값을 의미하지 않습니다. provider별 저장 분리는 모델·차원 변경의 자동 호환성을 보장하지 않습니다.
