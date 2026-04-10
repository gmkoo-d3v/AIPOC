# Documind (AIPOC) - 개인 기여 포트폴리오

> 이 문서는 팀 프로젝트 전체 소개가 아니라, 제가 직접 설계하고 구현한 범위만 정리한 README입니다.

## 1. 프로젝트 개요
- 원본 저장소: [Bjs-coder-kr/AIPOC](https://github.com/Bjs-coder-kr/AIPOC)
- 프로젝트 성격: 문서 분석 및 최적화 POC
- 팀 구성: 3인
- 기술 스택: Python, Streamlit
- 핵심 기능: PDF 분석, 품질 평가, RAG 검색, 
  타깃 맞춤 리라이팅(Actor-Critic), 결과 내보내기
- 기여 방식: 담당 기능을 독립 구현 후 팀 레포에 통합 및 안정화

## 2. 기여 목표
1. 벡터스토어 장애가 발생해도 사용자 흐름이 중단되지 않도록 복원력을 높인다.
2. 멀티 임베딩 프로바이더 환경에서 데이터 혼합으로 인한 검색 품질 저하를 방지한다.
3. 운영자가 데이터 상태와 사용자 이력을 빠르게 확인할 수 있도록 가시성을 강화한다.
4. 분석 결과를 즉시 검토하고 공유할 수 있도록 내보내기 기능과 한글 호환성을 개선한다.

## 3. 주요 기여 내용

### A. ChromaDB 장애 대응 Fallback 경로 구현
- 배경: ChromaDB 실패 시 분석 및 최적화 흐름 전체가 멈출 수 있는 단일 실패 지점이 존재했다.
- 기여:
  - `SimpleVectorStore` 기반 fallback 경로를 추가했다.
  - UI 및 오케스트레이션 흐름에서 장애 발생 시 우회 처리되도록 연결했다.
- 기대 효과:
  - 벡터스토어 장애 상황에서도 핵심 사용자 플로우가 완전히 중단되지 않도록 복원력을 확보했다.
- 관련 커밋: `30f4f66`, `76aaf60`

### B. 멀티 프로바이더 데이터 분리
- 배경: 임베딩 프로바이더마다 벡터 차원과 특성이 다른데 동일 저장소를 공유하면 데이터 오염과 검색 품질 저하 가능성이 있었다.
- 기여:
  - 프로바이더별 컬렉션 및 저장 흐름을 분리했다.
  - UI 탐색기에서 provider 선택 기반 조회를 지원하도록 개선했다.
- 기대 효과:
  - 프로바이더 간 데이터 혼합 리스크를 줄이고 검색 결과의 일관성을 높였다.
- 관련 커밋: `4d1ba9a`

### C. 운영 가시성 기능 강화
- 배경: 운영자가 사용자별 이력과 DB 상태를 직접 확인하기 어려워 장애 분석과 운영 대응에 시간이 오래 걸렸다.
- 기여:
  - 사이드바 메뉴 구조를 정리했다.
  - SQLite Explorer, ChromaDB Explorer, 사용자 목록 및 상세 조회 기능을 추가했다.
- 기대 효과:
  - 운영자가 데이터 상태를 빠르게 파악하고 원인 추적 시간을 줄일 수 있도록 지원했다.
- 관련 커밋: `a3c48b1`, `b013cc3`

### D. 결과 내보내기 및 한글 호환성 개선
- 배경: 분석 결과를 외부에 공유하려면 별도 가공이 필요했고, TXT/PDF 내보내기 시 한글 깨짐 이슈가 있었다.
- 기여:
  - 히스토리, 베스트 프랙티스, SQLite 탐색기 화면에서 TXT/PDF 내보내기를 연결했다.
  - TXT는 UTF-8 BOM을 적용하고, PDF는 `TTFont` 로딩을 보완해 한글 호환성을 개선했다.
- 기대 효과:
  - 분석 결과를 바로 보고서 형태로 공유할 수 있게 되었고, 한글 문서 품질도 안정화했다.
- 관련 커밋: `4d78fab`, `aaf29f8`, `5446707`, `58f7f41`, `8326a08`

### E. 저장소 구조 정리 및 협업 안정성 개선
- 배경: 중첩 저장소와 불안정한 디렉터리 구조로 인해 개발 및 협업 흐름이 매끄럽지 않았다.
- 기여:
  - 소스 구조를 `documind` 패키지 중심으로 정리했다.
  - `.gitignore`를 정비하고 불필요한 추적 파일을 정리했다.
- 기대 효과:
  - 개발 환경의 일관성을 높이고 협업 중 충돌 가능성을 줄였다.
- 관련 커밋: `f66c715`, `412d0e0`, `5ea4f6f`, `48b17d1`

## 4. 주로 기여한 코드 영역
- `documind/app/views/analy_app.py`
- `documind/utils/export.py`
- `documind/utils/best_practice_manager.py`
- `documind/target_optimizer/optimizer.py`
- `documind/llm/config.py`

## 5. 검증 관점
- 벡터스토어 장애 시 fallback 경로가 정상 동작하는지 확인
- 프로바이더 분리 저장 및 조회가 일관되게 유지되는지 확인
- 탐색기 기능에서 데이터 조회 및 내보내기 결과가 무결한지 확인
- 한글 TXT/PDF 출력이 깨지지 않고 정상 렌더링되는지 확인

### 참고 테스트 명령
```bash
pytest documind/tests/test_optim_rag.py
pytest documind/tests/test_db_embeddings.py
pytest documind/tests/test_eval_cases.py

```

## 6. 기여 확인 기준
- Git 작성자 필터 기준(`gmkoo-d3v`) 커밋 이력 중심으로 정리
- 대표 작업일: `2026-01-26`
- 참고 명령:
```bash
git log --author="gmkoo-d3v" --oneline
```

## 7. 기여 커밋 확인

원본 저장소에서 제 커밋을 직접 확인할 수 있습니다.

- 원본 레포: [Bjs-coder-kr/AIPOC](https://github.com/Bjs-coder-kr/AIPOC)
- 브랜치: `dev`
- 커밋 목록 바로가기: [gmkoo-d3v 커밋 보기](https://github.com/Bjs-coder-kr/AIPOC/commits/dev?author=gmkoo-d3v)

```bash
git clone https://github.com/Bjs-coder-kr/AIPOC
git log dev --author="gmkoo-d3v" --oneline
```
---
