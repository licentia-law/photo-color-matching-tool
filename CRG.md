# CRG – Coding Rules & Guidelines  
Photo Color Matching Tool (MVP)

---

## 1. 목적 (Purpose)

본 문서는 Photo Color Matching Tool 개발 시 적용할 **코딩 규칙, 구조 원칙, 구현 가이드라인**을 정의한다.  
PRD에 정의된 요구사항을 **일관성 있고 확장 가능하게 구현**하는 것을 목표로 한다.

---

## 2. 기본 원칙 (General Principles)

- MVP 범위 외 기능은 구현하지 않는다
- 모든 코드는 **명확성 > 간결성 > 성능** 우선
- 하나의 함수는 **하나의 책임**만 가진다
- 이미지 처리 로직과 UI 로직을 명확히 분리한다
- 추후 로드맵 기능 추가를 고려한 구조를 유지한다

---

## 3. 디렉터리 구조 (Directory Structure)

```text
photo-color-matching/
├─ app.py                  # Streamlit 엔트리 포인트
├─ requirements.txt
├─ README.md
├─ output/                 # 결과 이미지 저장 폴더
├─ logs/
│  └─ error.log            # 에러 로그
├─ core/
│  ├─ __init__.py
│  ├─ io.py                # 이미지 로드 / 저장 / 색공간 변환
│  ├─ analysis.py          # 기준 이미지 색감 분석
│  ├─ transfer.py          # 색감 적용 로직
│  └─ utils.py             # 공통 유틸
└─ config/
   └─ constants.py         # 고정 파라미터 (컷 수치 등)
```

---

## 4. 파일별 책임 규칙 (File Responsibilities)

### app.py
- Streamlit UI 구성
- 사용자 입력 수집
- core 모듈 호출
- 결과 및 오류 메시지 표시
- **이미지 처리 로직 직접 포함 금지**

### core/io.py
- 이미지 파일 로드
- JPG / PNG 유효성 검사
- sRGB 색공간 강제 변환
- 이미지 저장(output 폴더)

### core/analysis.py
- 기준 사진 A 분석
- Lab 색공간 변환
- 전체 픽셀 기반 통계 계산
- 하이라이트/섀도우 컷 적용
- 색감 모델 반환

### core/transfer.py
- 사진 B에 색감 적용
- a, b 채널 매칭
- 색감 강도(Blend) 반영
- 값 클리핑 처리

### core/utils.py
- 공통 수학 함수
- 배열 클리핑
- 예외 메시지 포맷

### config/constants.py
- 하이라이트/섀도우 컷 수치
- 기본 색감 강도
- suffix 문자열
- **하드코딩 금지, 상수로만 관리**

---

## 5. 이미지 처리 규칙 (Image Processing Rules)

- 내부 처리 색공간은 **항상 sRGB**
- 색감 분석 및 적용은 **Lab 색공간 기준**
- MVP 기준:
  - a, b 채널만 매칭
  - L 채널은 보존
- 모든 픽셀 값은 최종적으로 0–255 범위로 클리핑

---

## 6. 색감 분석 규칙 (Analysis Rules)

- 기준 사진 A는 전체 픽셀 기준으로 분석
- 하이라이트 / 섀도우 컷 적용
- 분석 결과는 재사용 가능한 구조체로 반환

---

## 7. 색감 적용 규칙 (Transfer Rules)

- 색감 강도(Blend)는 0–100 범위
- Blend는 a, b 채널 적용 비율에만 영향
- L 채널 변경은 MVP에서 금지

---

## 8. UI / UX 규칙 (Streamlit)

- 실행은 명시적 버튼 클릭으로만 수행
- 업로드 순서가 UI 상 명확해야 함
- 변환 완료 시 저장 경로 안내
- MVP에서는 전/후 미리보기 구현 금지

---

## 9. 에러 처리 규칙 (Error Handling)

- 모든 예외는 try/except로 처리
- 에러 발생 시:
  - logs/error.log 기록
  - 사용자 메시지 표시
- 프로그램 강제 종료 금지

---

## 10. 네이밍 규칙 (Naming Conventions)

- 파일명/함수명/변수명: snake_case
- 상수: UPPER_SNAKE_CASE

---

## 11. 금지 사항 (Explicitly Forbidden)

- app.py에 이미지 처리 로직 작성
- 하드코딩 수치 사용
- UI와 알고리즘 코드 혼합
- MVP 범위 외 기능 구현

---

## 12. 완료 기준 (Definition of Done)

- PRD MVP 요구사항 충족
- CRG 위반 없음
- Streamlit 정상 실행 및 저장 확인

---
