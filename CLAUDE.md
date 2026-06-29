<!-- >>> managed by mswai >>> -->
@AGENTS.md
<!-- <<< managed by mswai <<< -->

# 메이플스토리 월드 기획서 참조 규칙

> 이 섹션은 위 `@AGENTS.md`(MSW Foundation 규칙) 위에 **이 프로젝트에만 해당하는 규칙**을 더한 것이다.
> AGENTS.md의 Skill/Reference 로딩 절차를 먼저 따른 뒤, 아래 기획서 규칙을 적용한다.

이 프로젝트의 모든 세부 기획서는 `docs/specs/` 폴더에 마크다운으로 관리된다.
관련 기능의 `.mlua` 스크립트·컴포넌트·UI를 작성/수정하기 전에, 해당 기획서를 **`Read` 도구로 전문(全文)을 먼저 읽고** 구현한다. (`offset`/`limit` 없이, shell `cat`/`type` 금지 — AGENTS.md 규칙과 동일)

## 📁 기획서 인덱스 및 경로
| 기획서 | 경로 | 다루는 내용 |
|---|---|---|
| 💡 컨셉 | `docs/specs/concept.md` | 세계관, 핵심 재미 요소, 아트 방향성 |
| 🧊 큐브 스크래치 시스템 | `docs/specs/cube_scratch_system.md` | 큐브 아이템 사용 메커니즘, 스크래치 UI/UX 및 조작 흐름 |
| ❓ 잠재능력 판정 | `docs/specs/potential_determination.md` | 등급(레어~레전더리), 옵션 재설정 로직, 등급 상승·옵션 등장 확률 테이블 |
| 💵 판매가 산정 | `docs/specs/pricing_calculation.md` | 아이템·재화 가치 기준, 상점 판매/구매 가격 밸런싱 공식 |
| 🔀 게임 흐름 | `docs/specs/game_flow_charts.md` | 로그인~인게임 루프, 콘텐츠 전환 등 전체 상태 머신(FSM)·시퀀스 |

## 🔗 작업 → 필독 기획서 매핑
아래 키워드가 작업에 등장하면, 구현 전 해당 기획서를 반드시 Read 한다.

| 작업 키워드 | 필독 기획서 |
|---|---|
| 큐브 / 스크래치 / 큐브 UI / 큐브 아이템 | `cube_scratch_system.md` + `potential_determination.md` |
| 잠재능력 / 등급 / 옵션 재설정 / 확률 / 등급 상승 | `potential_determination.md` |
| 가격 / 판매가 / 구매가 / 상점 / 재화 밸런싱 | `pricing_calculation.md` |
| 로그인 / 씬 전환 / 게임 루프 / FSM / 시퀀스 | `game_flow_charts.md` |
| 신규 기능 / 컨셉 / 전반 방향성 | `concept.md` |

## ⚙️ 개발 시 주의사항 (MSW 특화)
1. **확률·수식은 반드시 서버에서 처리한다.** 잠재능력 판정, 판매가 산정 등 결과에 영향을 주는 연산은 `@ExecSpace("ServerOnly")`(또는 `Server`)에서 검증·확정하고, 클라이언트는 연출/표시만 담당한다. 클라이언트 입력값을 신뢰하지 않는다.
2. **기획서 흐름(Flow)을 컴포넌트 경계로 사용한다.** `game_flow_charts.md`의 상태/시퀀스 단위로 `@Logic`(월드 전역·맵 전환 유지) vs 맵 엔티티 `@Component`(맵 한정) 스코프를 나눈다. (AGENTS.md "스크립트 스코프" 규칙 준수)
3. **기획서가 단일 진실 공급원(SSOT)이다.** 확률 테이블·가격 공식 같은 수치는 코드에 흩뿌리지 말고, 기획서 값을 그대로 옮긴 뒤 출처를 주석으로 남긴다. 구현과 기획서가 충돌하면 임의로 결정하지 말고 사용자에게 확인한다.
4. **기획서 미존재/모호 시 중단한다.** 위 매핑에 해당하는 기획서가 없거나 내용이 부족하면, 추측해서 구현하지 말고 어떤 정보가 필요한지 사용자에게 먼저 묻는다.
