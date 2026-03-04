# AIPOC
AIPOC TEAM REPO
# Documind (AIPOC) - 개인 기여 포트폴리오

> 이 문서는 팀 프로젝트 전체 소개가 아니라, **제가 직접 수행한 작업 범위**만 정리한 README입니다.

## 1. 프로젝트 맥락
- 원본 저장소: [Bjs-coder-kr/AIPOC](https://github.com/Bjs-coder-kr/AIPOC)
- 프로젝트: 문서 분석/최적화 POC (`Python + Streamlit`)
- 핵심 도메인: PDF 분석, 품질 평가, RAG 검색, 타깃 맞춤 리라이팅(Actor-Critic), 결과 내보내기

## 2. 내가 맡은 목표 (Why)
1. 벡터스토어 장애 시에도 사용자 플로우가 중단되지 않도록 복원력 강화
2. 멀티 임베딩 프로바이더 환경에서 데이터 혼합(오염) 리스크 제거
3. 운영자가 데이터 상태를 즉시 확인할 수 있도록 가시성 강화
4. 분석 결과를 바로 보고/공유 가능한 포맷으로 내보내기

## 3. 내가 구현한 내용 (What / How)

### A. ChromaDB 장애 대응용 Fallback 경로 구현
- 문제: ChromaDB 실패 시 분석/최적화 흐름 전체가 멈출 수 있는 단일 실패 지점 존재
- 구현:
  - `SimpleVectorStore` fallback 경로 추가
  - UI/오케스트레이션 흐름에서 장애 시 우회 처리
- 관련 커밋: `30f4f66`, `76aaf60`

### B. 멀티 프로바이더 데이터 분리
- 문제: 프로바이더별 임베딩 차원/특성이 다른데 같은 저장소를 공유하면 검색 품질 저하 가능
- 구현:
  - 프로바이더별 컬렉션/저장 흐름 분리
  - UI 탐색기에서 provider 선택 기반 조회 지원
- 관련 커밋: `4d1ba9a`

### C. 운영 가시성 기능(SQLite/ChromaDB 탐색기, 사용자 조회)
- 문제: 운영자가 사용자별 이력/DB 상태를 확인하기 어렵고 장애 원인 추적 시간이 길었음
- 구현:
  - 사이드바 메뉴 개편
  - SQLite Explorer, ChromaDB Explorer, 사용자 목록/조회 기능 추가
- 관련 커밋: `a3c48b1`, `b013cc3`

### D. 결과 내보내기 및 한글 호환성 개선
- 문제: 분석 결과를 외부 공유할 때 포맷 변환이 번거롭고 한글 깨짐 이슈 발생
- 구현:
  - 히스토리/베스트 프랙티스/SQLite 탐색기에서 TXT/PDF 내보내기 연결
  - TXT UTF-8 BOM 적용, PDF 폰트 로딩(TTFont) 보완
- 관련 커밋: `4d78fab`, `aaf29f8`, `5446707`, `58f7f41`, `8326a08`

### E. 저장소 구조 정리 및 협업 안정성 개선
- 문제: 중첩 레포/디렉터리 구조로 개발 흐름이 불안정
- 구현:
  - 소스 위치 정리(`documind` 패키지 중심)
  - `.gitignore` 정비 및 불필요 추적 파일 정리
- 관련 커밋: `f66c715`, `412d0e0`, `5ea4f6f`, `48b17d1`

## 4. 내가 주로 다룬 코드 위치 (Where)
- `documind/app/views/analy_app.py`
- `documind/utils/export.py`
- `documind/utils/best_practice_manager.py`
- `documind/target_optimizer/optimizer.py`
- `documind/llm/config.py`

## 5. 검증 방식 (Verify)
- 회귀 관점:
  - 벡터스토어 장애 시 fallback 동작 여부
  - provider 분리 저장/조회 일관성
  - 탐색기 데이터 조회 및 내보내기 무결성
  - 한글 TXT/PDF 출력 호환성

- 참고 테스트 명령:
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

---

필요 시 이 README를 기반으로 이력서/포트폴리오 형식(한 페이지 요약, STAR 사례형, 직무별 버전)으로 바로 변환할 수 있습니다.
