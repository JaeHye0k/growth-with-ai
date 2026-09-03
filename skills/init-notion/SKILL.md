---
name: init-notion
description: Notion MCP 연동을 확인하고 프로젝트 작업 DB에 '구현 방식'(직접/위임/혼합) 속성을 설정한다. "노션 연동", "Notion 설정", "구현 방식 속성"을 언급하거나, baseline 설정 직후, 또는 config.json에 Notion DB 정보가 없는 상태에서 티켓 기록을 시도할 때 사용한다.
---

# init-notion — Notion 연동 설정 (선택 사항)

## 경로 계약 (growth-with-ai 4개 스킬 공유 — 변경 금지)

- `.claude/growth-with-ai/baseline.md` — 위임 경계선. baseline 스킬이 생성/갱신
- `.claude/growth-with-ai/config.json` — Notion 연동 정보. **이 스킬이 생성**
- `.claude/growth-with-ai/exams/<slug>.md` — exam이 떨군 명세. 세션 간 인계용
- `.claude/growth-with-ai/overrides.log` — 규칙 우회 기록 (탭 구분, 한 줄 한 건)
- `.claude/growth-with-ai/unclassified.log` — 미분류 유형 누적 (탭 구분, 한 줄 한 건)

## 전제

**전부 선택 사항이다.** exam/review는 Notion 없이도 완전히 동작해야 한다.
Notion이 연결돼 있으면 exam이 티켓의 구현 방식을 갱신하고, 없으면 로컬 로그로만 남긴다.
이 스킬이 실패하거나 중단되어도 다른 스킬은 영향을 받지 않는다.

## 절차

### 1. MCP 확인

현재 세션에 Notion 도구(MCP)가 있는지 확인한다.
**없으면** Notion MCP 등록 방법을 안내하고 **여기서 중단한다.** 사용자가 붙인 뒤 재실행하게 한다.

### 2. DB 연결

`.claude/growth-with-ai/config.json`을 읽는다 (없으면 새로 만들 준비).

`notion.database_id`가 없으면 사용자에게 작업 DB 링크를 요구한다.

- URL에서 database id를 파싱한다.
- **실제 조회로 접근 가능 여부를 확인한다.** 조회가 실패하면 공유 설정을 안내하고 중단한다.

### 3. 속성 보정

DB 스키마를 조회한다:

- `구현 방식` 속성이 **없으면**: select 타입으로 추가하고 옵션 `직접` / `위임` / `혼합`을 생성한다.
- 속성은 있는데 옵션이 빠졌으면: 빠진 옵션만 보충한다. 기존 옵션은 건드리지 않는다.

### 4. 검증

아무 티켓 하나에 `구현 방식` 값을 실제로 써보고 **원래 값으로 되돌린다.**
쓰기가 실패하면 권한 문제를 안내하고 config를 저장하지 않은 채 중단한다.

### 5. config.json 저장

```json
{
  "notion": {
    "database_id": "...",
    "property_name": "구현 방식",
    "options": ["직접", "위임", "혼합"]
  }
}
```

기존 config.json에 다른 키가 있으면 보존하고 `notion` 키만 갱신한다.
저장 후 "이후 exam이 티켓의 구현 방식을 자동 기록합니다"를 안내하고 종료한다.
