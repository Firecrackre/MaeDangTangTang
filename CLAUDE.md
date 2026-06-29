<!-- >>> managed by mswai >>> -->
@AGENTS.md
<!-- <<< managed by mswai <<< -->

# 메이플스토리 월드 기획서 참조 규칙

> 이 섹션은 위 `@AGENTS.md`(MSW Foundation 규칙) 위에 **이 프로젝트에만 해당하는 규칙**을 더한 것이다.
> AGENTS.md의 Skill/Reference 로딩 절차를 먼저 따른 뒤, 아래 기획서 규칙을 적용한다.

이 프로젝트의 모든 세부 기획서는 `docs/specs/` 폴더에 마크다운으로 관리된다.
관련 기능의 `.mlua` 스크립트·컴포넌트·UI를 작성/수정하기 전에, 해당 기획서를 **`Read` 도구로 전문(全文)을 먼저 읽고** 구현한다. (`offset`/`limit` 없이, shell `cat`/`type` 금지 — AGENTS.md 규칙과 동일)

## 기획서 인덱스 및 경로
| 기획서 | 경로 | 다루는 내용 |
|---|---|---|
| 컨셉 | `docs/specs/concept.md` | 세계관, 핵심 재미 요소, 아트 방향성 |
| 큐브 스크래치 시스템 | `docs/specs/cube_scratch_system.md` | 큐브 아이템 사용 메커니즘, 스크래치 UI/UX 및 조작 흐름 |
| 잠재능력 판정 | `docs/specs/potential_determination.md` | 등급(레어~레전더리), 옵션 재설정 로직, 등급 상승·옵션 등장 확률 테이블 |
| 판매가 산정 | `docs/specs/pricing_calculation.md` | 아이템·재화 가치 기준, 상점 판매/구매 가격 밸런싱 공식 |
| 게임 흐름 | `docs/specs/game_flow_charts.md` | 로그인~인게임 루프, 콘텐츠 전환 등 전체 상태 머신(FSM)·시퀀스 |

## 작업 → 필독 기획서 매핑
아래 키워드가 작업에 등장하면, 구현 전 해당 기획서를 반드시 Read 한다.

| 작업 키워드 | 필독 기획서 |
|---|---|
| 큐브 / 스크래치 / 큐브 UI / 큐브 아이템 | `cube_scratch_system.md` + `potential_determination.md` |
| 잠재능력 / 등급 / 옵션 재설정 / 확률 / 등급 상승 | `potential_determination.md` |
| 가격 / 판매가 / 구매가 / 상점 / 재화 밸런싱 | `pricing_calculation.md` |
| 로그인 / 씬 전환 / 게임 루프 / FSM / 시퀀스 | `game_flow_charts.md` |
| 신규 기능 / 컨셉 / 전반 방향성 | `concept.md` |

## 개발 시 주의사항 (MSW 특화)
1. **확률·수식은 반드시 서버에서 처리한다.** 잠재능력 판정, 판매가 산정 등 결과에 영향을 주는 연산은 `@ExecSpace("ServerOnly")`(또는 `Server`)에서 검증·확정하고, 클라이언트는 연출/표시만 담당한다. 클라이언트 입력값을 신뢰하지 않는다.
2. **기획서 흐름(Flow)을 컴포넌트 경계로 사용한다.** `game_flow_charts.md`의 상태/시퀀스 단위로 `@Logic`(월드 전역·맵 전환 유지) vs 맵 엔티티 `@Component`(맵 한정) 스코프를 나눈다. (AGENTS.md "스크립트 스코프" 규칙 준수)
3. **기획서가 단일 진실 공급원(SSOT)이다.** 확률 테이블·가격 공식 같은 수치는 코드에 흩뿌리지 말고, 기획서 값을 그대로 옮긴 뒤 출처를 주석으로 남긴다. 구현과 기획서가 충돌하면 임의로 결정하지 말고 사용자에게 확인한다.
4. **기획서 미존재/모호 시 중단한다.** 위 매핑에 해당하는 기획서가 없거나 내용이 부족하면, 추측해서 구현하지 말고 어떤 정보가 필요한지 사용자에게 먼저 묻는다.


# PROJECT-SPECIFIC NOTES — MaeDangTangTang

> 일반 MSW 규칙은 위 `@AGENTS.md`(자동 생성) 참조. 아래는 이 프로젝트 고유 컨텍스트(사람 관리, 마커 바깥이라 mswai 재생성에 보존됨).

## Maps (`map/`)
- `MainMap`, `YoungMinMap`, `DongHeeMap`, `JiWonMap`

## 구현된 기능
- **대장장이 코어(Blacksmith) — 서버 권위 게임 로직** — `RootDesk/MyDesk/Blacksmith/`.
  - `GameManager.mlua` (`@Logic`, 전역 단일 세션) — 플레이어 상태 + 모든 규칙의 권위. 상태는 `@Sync` property로 HUD에 노출:
    `Meso` / `CurrentItemId` / `CubeUseCount` / `HasPotential` / `CurrentGrade` / `CurrentPrice` / `IsCurrentDestroyed` /
    `CubesUntilRepayment` / `IsRepaymentDue` / `PendingRepayment` / `DebtStage` / `IsGameOver`.
    (단, `CurrentPotential` 옵션 3줄은 중첩 테이블이라 `@Sync` 불가 → UI 단계에서 `@ExecSpace("Client")` RPC로 따로 전달.)
  - UI→서버 RPC 엔트리포인트(전부 `@ExecSpace("Server")`, 클라 입력 불신·서버 전량 검증):
    `_GameManager:RequestBuyItem(equipId)` / `RequestUseCube()` / `RequestSellItem()` /
    `RequestConfirmDestroy()`(파괴 아이템 정리) / `RequestSettleDebt()`(상환·부족 시 게임오버). UI 버튼을 여기에 연결한다.
  - `PotentialService.mlua` — 잠재 판정(`RollPotential()` → 등급+옵션 3줄). `PricingCalculator.mlua` — 판매가 산정(`Calculate(equipId, pr)` → `{destroyed, price, ...}`).
  - `BlacksmithConfig.mlua` (`@Logic`) — 구현된 밸런스 상수의 SSOT. 시작메소/큐브비용/등급·옵션 확률/장비 5종/빚 상환표 등 모든 수치는 이 파일에서만 조정한다(기획서 값을 옮긴 코드 측 SSOT).
- **복권 긁기(Scratch-ticket) UI** — `RootDesk/MyDesk/ScratchTicket/`, `ui/ScratchTicketGroup.ui`.
  `ScratchTicketManager.mlua`가 3장의 은박 픽셀캔버스(`PixelGUIRendererComponent`)를 숨겨진 "당첨" 레이어 위에 깔고,
  클릭/드래그로 알파를 깎아 긁음(70% 도달 시 완료 이벤트). 격자해상도/브러시반경/강도/완료기준/대상맵은
  데이터테이블 `ScratchTicketConfig.userdataset`+`.csv`(`_DataService:GetTable`)로 구동, 없으면 인스펙터 기본값 폴백.

## 검증된 gotcha (이 프로젝트에서 실제로 디버깅함)
- **UI 히트테스트 좌표공간이 두 개다.** `UITransformComponent:GetWorldCorners()`는 **UI-캔버스 월드**(수백 단위)를
  돌려주며, 이는 `_UILogic:ScreenToWorldPosition`/`LocalUIToWorldPosition`가 쓰는 **게임 월드(±~6)와 다르다.**
  화면 터치를 UI 사각형에 매핑하려면 `ScreenToWorldPosition(touch)`와
  `_UILogic:LocalUIToWorldPosition(Vector2(±RectSize/2), uiTransform)`를 짝지어라(둘 다 게임 월드).
- **`.ui`는 맵 소속이 아닌 전역 자원**(`/ui` 트리, 모든 맵에 렌더). 특정 맵에서만 보이려면
  `_UserService.LocalPlayer.CurrentMapName`을 폴링하고, **게이팅 엔티티의 `Enable`을 `OnBeginPlay`에서 명시적으로 초기화**하라.
  변화감지만 하는 가드(`if onMap ~= isActive`)는 초기 상태를 못 잡아 비대상 맵에서 UI가 그대로 보인다.
- **UIBuilder `panel`은 UITransform만 있고 렌더러가 없다.** 색 사각형은 `sprite`로, `PixelGUIRendererComponent`는
  `panel`에 `addComponent`로 부착. 유니티식 브러시 컴포넌트는 없음 — `PixelGUIRendererComponent`
  (`SetPixel`/`SetAlpha`/`ResetWithColor`)가 긁기/페인트 캔버스.
