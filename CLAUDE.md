# CLAUDE.md

이 레포에서 작업하는 AI(Claude Code 등)가 따라야 할 규칙입니다. 사람용 전체 규칙은 `README.md` 4장(형상관리)과 5장(AI 활용)에 있고, 이 파일은 그중 AI가 반드시 지킬 것만 모았습니다.

## 프로젝트

- 부산 로컬페이 관광 소비 루트 (KB Bridge 파이널 프로젝트, 팀원 7명 중 개발자 1명)
- 기준 문서: `docs/KB_Bridge_PRD_부산로컬페이루트_v0.3.docx` (DB 스키마는 부록 B, CSV 템플릿은 부록 C)
- 스택: PostgreSQL · pandas · Streamlit · n8n. 수업에서 배운 도구 밖의 새 기술을 추가하지 않는다.
- 사용자에게는 한국어로 답한다.

## 절대 하지 않을 것

- `main`에 직접 commit·push 하지 않는다. 항상 브랜치 → PR.
- `git push --force`, `git reset --hard`, 히스토리 재작성을 하지 않는다.
- `.env`, API 키, Telegram 토큰, n8n webhook URL, DB 접속 정보를 커밋하지 않는다.
- `data/raw/` 원본 파일을 수정하거나 커밋하지 않는다 (`MANIFEST.csv`만 예외).
- 이미 머지된 `db/migrations/` 파일을 수정하지 않는다.
- `data/manual/benefit_policy.csv`, `scenario_assumption.csv`의 기존 행을 고치지 않는다. 새 버전 행을 추가한다.
- 요청받은 이슈 범위 밖의 파일, 다른 팀원의 파일을 수정하지 않는다.
- 실명, 연락처, 텔레그램 chat_id, 정밀 위치를 코드·데이터·로그에 넣지 않는다.

## 작업 순서

1. 이슈 번호를 확인한다. 없으면 사용자에게 묻는다.
2. `main`을 최신으로 받고 브랜치를 만든다: `<접두어>/<이슈번호>-<짧은-영문-설명>`
   - 접두어: `feat` `fix` `data` `db` `docs` `design` `chore`
3. 작업 후 **커밋 전에** `git status`와 diff 요약, 커밋 메시지 초안을 보여 주고 확인을 받는다.
4. 커밋 하나에는 한 가지 변경만 담는다. 종류가 섞이면 커밋을 나누자고 제안한다.
5. push와 PR 생성도 사용자가 확인한 뒤에만 한다.

## 커밋 메시지

```
<type>(<범위>): <무엇을 했는지 한 줄>

<필요하면 이유>

Refs #<이슈번호>
```

type: `feat` `fix` `data` `db` `docs` `design` `test` `refactor` `chore`

## PR

- PR 하나 = 이슈 하나. 본문에 무엇을 바꿨는지, 어떻게 확인했는지, 영향 범위, `Closes #<이슈번호>`를 넣는다.
- 라벨(`area:*`, `P0~P2`)과 마일스톤(`W1 기획 확정` / `W2 핵심 구현` / `W3 통합·마무리`)을 붙인다.
- 머지는 개발 리드가 한다. AI가 머지하지 않는다.

## 데이터와 DB

- `data/manual/` CSV를 커밋하기 전에 `python pipeline/validate.py`를 실행하고, 실패한 행은 고치지 말고 사용자에게 보고한다.
- 스키마 변경은 `README.md` 4.5 절차를 따른다: 새 마이그레이션 파일 → `db/schema.sql` 갱신 → PRD 9.3 변경 이력 한 줄.
- 결제 상태 규칙: `payment_capability`의 card·qr은 검증 기록(`payment_check_log`)이 있을 때만 `confirmed`·`unavailable`이다. 동백전 가맹 여부만으로 card·qr을 `confirmed`로 바꾸지 않는다.
- 화면·코드·문서에 "실시간", "확실히 결제 가능", 매출 증가를 단정하는 표현을 쓰지 않는다.

## 일정

- 11/2 기능 동결 이후에는 버그 수정만 한다. 새 기능 요청이 오면 동결 상태임을 알린다.
- 최종 제출 11/4 10:00, 발표 11/5.
