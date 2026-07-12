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
| 로비 허브 | `docs/specs/lobby_hub.md` | 큐브 스크래치·대장 기술·재정 관리 3갈래 진입 허브, 상태 확인·전략 선택, 기능 간 연결·구현 우선순위 |
| 큐브 스크래치 시스템 | `docs/specs/cube_scratch_system.md` | 큐브 아이템 사용 메커니즘, 스크래치 UI/UX 및 조작 흐름 |
| 잠재능력 판정 | `docs/specs/potential_determination.md` | 등급(레어~레전더리), 옵션 재설정 로직, 등급 상승·옵션 등장 확률 테이블 |
| 대장 기술 업그레이드 | `docs/specs/blacksmith_upgrade.md` | 대장 기술 획득·선택·중복(단계 상승)·종류(더 좋은 물품/대장장이의 눈/가격 협상/위험 거래)·적용 우선순위 |
| 판매가 산정 | `docs/specs/pricing_calculation.md` | 아이템·재화 가치 기준, 상점 판매/구매 가격 밸런싱 공식 |
| 무기 상점 | `docs/specs/weapon_shop.md` | 상점 슬롯·무기 종류/등급·구매·갱신·판매가 티어(as-built 정합화) |
| 재정 관리 | `docs/specs/finance_management.md` | 메소 대출 서비스, 골드리치 은행 저축, 상환 압박 연동, 재정 표시 정보 |
| 게임 흐름 | `docs/specs/game_flow_charts.md` | 로그인~인게임 루프, 콘텐츠 전환 등 전체 상태 머신(FSM)·시퀀스 |

## 작업 → 필독 기획서 매핑
아래 키워드가 작업에 등장하면, 구현 전 해당 기획서를 반드시 Read 한다.

| 작업 키워드 | 필독 기획서 |
|---|---|
| 로비 / 허브 / 메인 화면 / 메뉴 선택 | `lobby_hub.md` |
| 큐브 / 스크래치 / 큐브 UI / 큐브 아이템 | `cube_scratch_system.md` + `potential_determination.md` |
| 잠재능력 / 등급 / 옵션 재설정 / 확률 / 등급 상승 | `potential_determination.md` |
| 대장 기술 / 업그레이드 / 증강 / 기술 선택 / 더 좋은 물품 / 대장장이의 눈 / 가격 협상 / 위험 거래 | `blacksmith_upgrade.md` + `pricing_calculation.md` |
| 가격 / 판매가 / 구매가 / 상점 / 재화 밸런싱 | `pricing_calculation.md` |
| 무기 상점 / 상점 슬롯 / 무기 구매 / 무기 등급 / 새로고침 | `weapon_shop.md` + `pricing_calculation.md` |
| 재정 / 대출 / 저축 / 이자 / 상환 / 골드리치 은행 | `finance_management.md` + `pricing_calculation.md` |
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
- `TitleMap`(패키지 시작 맵) / `InGameMap`(무기 상점) / `YoungMinMap`(복권 긁기) / `DongHeeMap` / `JiWonMap`

## 구현된 기능
- **대장장이 코어(Blacksmith) — 서버 권위 게임 로직** — `RootDesk/MyDesk/Blacksmith/`.
  - `GameManager.mlua` (`@Logic`, **유저별 독립 세션** — 싱글게임) — 플레이어 상태 + 모든 규칙의 권위.
    **서버 진실 = `Sessions[userId]` 테이블**(스칼라 32개 + 중첩 `ShopSlots`/`CurrentPotential`/`OwnedAugments` + `UserId`).
    클라 노출 = 같은 이름의 **plain property 미러**(@Sync 아님): 각 Server RPC 말미 `PushState(userId)`가 32개 스칼라를
    `ApplySessionState(state, targetUserId)`(targeted Client RPC, call-site 마지막 인자=UserId)로 **해당 유저에게만** 전송.
    미러 property: `Meso` / `CurrentItemId` / `CubeUseCount` / `HasPotential` / `CurrentGrade` / `CurrentPrice` / `IsCurrentDestroyed` /
    `CubesUntilRepayment` / `IsRepaymentDue` / `PendingRepayment` / `DebtStage` / `IsGameOver` /
    `TotalSellCount` / `MaxSellPrice`(게임오버 기록용) / `AugmentsData`·`AugmentChoicesData`·`IsAugmentPending`(증강, CSV) /
    `LoanUseCount`·`CanLoan`(대출) / `CurrentActivity`("lobby"/"cube"/"augment"/"finance", 로비 전이).
    (이 문서의 다른 항목에서 "@Sync"라 적힌 GameManager 상태 서술은 전부 이 **미러 property**를 뜻한다 — 클라 폴링 패턴은 동일.)
    **세션 라이프사이클**: `UserEnterEvent` → `ResetSessionFor(userId)` / `UserLeaveEvent` → 세션 삭제(**재접속 = 새 게임**, 유저 확정) /
    클라 `OnUpdate` 최초 1회 `RequestInitialSync()` 핸드셰이크로 초기 미러 수신(입장 레이스 차단).
    **RPC 패턴**: 각 `@ExecSpace("Server")` RPC = `GetSession(senderUserId)` → `Do*(s)`(ServerOnly 본문, early return 가능) → `PushState` 3줄 래퍼.
    **헬퍼 이원화**: 클라 미러 판독용(`IsSessionEnded`/`GetCurrentDay`/`GetCurrentRepayment`/`GetSyncedAugmentStage`/`GetEffectiveBuyCost`, ExecSpace 없음)
    vs 서버 세션용 `...Of(s)` 변형(`IsSessionEndedOf`/`GetCurrentRepaymentOf`/`GetEffectiveBuyCostOf`/`GetAugmentStageOf`/`GetAugmentStageValueOf`/`ComputeRepaymentOf`).
    ⚠ 서버 코드에서 클라 판독용 헬퍼 호출 금지(서버 property 사본은 진실 아님).
  - UI→서버 RPC 엔트리포인트(전부 `@ExecSpace("Server")`, 클라 입력 불신·서버 전량 검증):
    `_GameManager:RequestBuyItem(slotIndex)`(상점 슬롯 인덱스로 구매) / `RequestUseCube()` / **`RequestSellItem()`(판매 **확정만** — 즉시 지급 안 함, `IsSalePending`으로 전환·아이템 유지) / `RequestCollectSale()`(수령 — 이때 메소 지급 + 아이템 정리)** /
    `RequestConfirmDestroy()`(파괴 아이템 정리) / `RequestSettleDebt()`(상환·부족 시 게임오버) /
    `RequestSelectAugment(key)`(증강 3택 선택) / `RequestLoan()`(메소 대출) / `RequestRestart()`(게임오버 후 전체 초기화) /
    `EnterActivity(name)`·`ReturnToLobby()`(로비 상태 전이). UI 버튼을 여기에 연결한다.
    (`Debt`: 대출로 누적된 갚아야 할 빚 @Sync. 대출 1회당 **현재 상환 필요 메소(`GetCurrentRepayment()`)만큼** 증가. `GetCurrentRepayment()`=상환 필요 메소 단일 진실값(미납 시 `PendingRepayment`, 아니면 `GetDebtAmount(DebtStage+1)` 미리보기) — HUD·재정·대출 공용 SSOT.)
  - `PotentialService.mlua` — 잠재 판정(`RollPotential(allowDestroy, equipId, validForceChance)` → 등급+옵션 3줄. equipId/validForceChance는 증강 '대장장이의 눈'용). `PricingCalculator.mlua` — 판매가 산정(`Calculate(equipId, pr)` → `{destroyed, price, ...}`) + **`GetOptionBonus(equipId, o)`(줄 1개의 판매가 보너스 순수함수 — Calculate와 스크래치 UI 실시간 상승이 공유하는 SSOT)**.
  - **2단계 판매(판매 확정 → 수령하기)** @Sync: `IsSalePending`/`PendingSalePrice`/`PendingSaleItemId`. `RequestSellItem`은 검증 후 이 3개만 세팅(메소·아이템 불변). `RequestCollectSale`이 `Meso += PendingSalePrice` + 통계 갱신 + `ClearItem`. pending 중 `RequestUseCube`·`RequestSellItem` 거부, `IsSessionEnded()` 공용 가드. `ResetSession`이 pending 초기화. (스크래치 판매 결과 창은 아래 복권 긁기 UI 참조.)
  - `BlacksmithConfig.mlua` (`@Logic`) — 구현된 밸런스 상수의 SSOT. 시작메소/큐브비용/등급·옵션 확률/장비 5종/빚 상환표 + **증강 4종(`augments`)·대출(`loan`)·저축(`savings`)** 등 모든 수치는 이 파일에서만 조정한다(기획서 값을 옮긴 코드 측 SSOT).
  - **로비 허브 서버 로직(증강·재정·게임오버·재시작) — UI 없이 로직/API만** (`lobby_hub`·`blacksmith_upgrade`·`finance_management` 기획서). 세부:
    - **대장 기술(증강) — 골드 구매형(구현 완료)**: `ResetSession`에서 `RollAugmentChoices` 1회 롤(세션 시작부터 3택 상시 준비) → 로비 '대장 기술' 버튼 → `_AugmentManager:Open()` → `RequestSelectAugment(key)`가 **도달 단계 기준 비용 검증·차감**(`BlacksmithConfig.augmentCosts={7500,12500,20000}` → `GetAugmentCost(stage)`) + 중복 시 단계 상승(최대 3) + **구매 성공 시 `RollAugmentChoices` 재추첨**(§4-4 새로고침). 무료 3택 폐지(`RequestSettleDebt`의 `RollAugmentChoices` 호출 제거). 효과 배선(불변): 더좋은물품→`RollShop` 등급확률(`betterGoodsGradeProb`) / 대장장이의눈→`RollLine` 유효옵션(`smithEye` 10/20/45%) / 가격협상→`GetEffectiveBuyCost` 구매가(`priceNego` 10/18/40%) / 위험거래→판매가·`ComputeRepayment` 이자(`riskyDeal` 10/20/30%). 클라 가격 표시는 `GetSyncedAugmentStage`(AugmentsData 파싱)로 단계 읽어 `GetAugmentCost(stage+1)`. 증강 아이콘은 `d.augments.*.icon`(plain 스프라이트 RUID). UI는 아래 **대장 기술 업그레이드 UI** 항목 참조.
    - **메소 대출**: `RequestLoan()` — 조건(`UpdateCanLoan`: 아이템 미보유면 상점 무기 전부 구매불가 / 보유면 큐브 구매불가) 충족 시 **지급 메소 = 현재 상환 필요 메소(`GetCurrentRepayment()`), 빚 `Debt += 같은 금액`** + **1회 제한**(`loan.maxUses`). 즉 상환 필요 메소만큼 받고 그만큼 빚져서, 상환 시 `상환금+빚`을 한 번에 청구(예: 상환 45,000 → 45,000 받고 빚 45,000 → 정산 90,000). ⚠ 기존 "고정 지급 100,000 / 빚 원금×2" 및 "상환금 2배(`LoanPenaltyPending`)" 방식은 모두 폐기. `d.loan`은 `maxUses`만 남김(금액은 동적).
    - **상환+빚 청산**: `RequestSettleDebt()` — 상환 시점(5/5) 도달 시 `PendingRepayment + Debt`를 전부 갚아야 통과(부족 시 게임오버). 성공 시 `Debt=0`·`DebtStage+1`. (⚠ 증강은 상환 보상이 아니라 골드 구매형 — 여기서 `RollAugmentChoices` 호출하지 않는다.) 다음 라운드 전환 시스템은 미구현 → `log("NEXT WEEK: day=N주차 …")`로만 알림.
    - **게임오버 기록**: `RecordGameOver(s)` — 게임오버 시 5개 항목(최종메소/큐브수/판매수/최고판매가/상환단계)을 `_DataStorageService:GetGlobalDataStorage("BlacksmithRecords")` **유저별 키 `lastRun:<userId>`**에 저장(Credit 절약 위해 1회성).
    - **재시작**: `RequestRestart()` — 게임오버 상태에서만, `ResetSessionFor(senderUserId)`로 **요청한 유저의 세션만** 전체 리셋(증강·대출·통계 포함, DataStorage 누적은 보존).
    - **일차 규약**: 별도 시간 시스템 없이 **일차 = `DebtStage + 1`**(A안). 서버 `GetCurrentDay()`, 클라는 직접 계산.
    - ✔ **상환금 표시(해결됨)**: 위험거래·대출 적용 후 실제 상환금은 `PendingRepayment`(=`ComputeRepayment`). 공용 상단 HUD(`HudManager`)가 상환 필요 메소를 `PendingRepayment` 우선(미보류 시 `GetDebtAmount(DebtStage+1)` 폴백)으로 표시하도록 이미 정합화됨.
- **무기 구매 상점(Weapon shop) UI** — `RootDesk/MyDesk/Blacksmith/WeaponShopManager.mlua`(`@Logic`, 클라), `ui/WeaponShopGroup.ui`.
  **상점 보드/슬롯 전용**(좌 시계·우 메소 상단 HUD는 아래 공용 HudManager 소관 — WeaponShopManager는 더 이상 HUD를 그리지 않는다).
  InGameMap+상점 화면(`ActiveScreen=="shop"`) 게이팅(`CurrentMapName` 폴링 + 보드 `Enable` 토글). 서버 권위로 N슬롯(config `shopSlotCount`) 추첨:
  `GameManager`의 `RollShop()` / `RequestRefreshShop()`(무료 재추첨) / `RequestBuyItem(slotIndex)`.
  슬롯은 `@Sync string ShopSlotsData`("equipId:grade,…" CSV)로 클라 전달(중첩테이블 @Sync 불가 회피). 한 상점 내 동일 (무기+등급) 조합 중복 없음.
  - **등급 확률(무기 상점 기획서 §9·§10).** 0단계(증강 미보유) 기본표 = 커먼60/레어30/에픽9/유니크1/레전더리0(`weaponGrades.prob`). '더 좋은 물품' 증강 보유 시 단계별 절대 확률표(`betterGoodsGradeProb`)로 통째 교체(1~3단계). 추첨은 `RollWeaponGradeDetail(stage)`(→`{grade, roll, total}`), 등급만 필요하면 래퍼 `RollWeaponGrade(stage)`. 표 조회 `GetShopGradeProbTable(stage)`. `RollShop`은 추첨마다 하단 콘솔에 확률표(누적 구간 포함)+슬롯별 `등급 무기(확률%, roll=값)` 로그를 남긴다(디버그 상시).
  - **무기 등급 ≠ 잠재능력 등급.** 상점 등급(`BlacksmithConfig.weaponGrades`: Common/Rare/Epic/Unique/Legendary)은 판매공식의 **"기본 판매가(basePrice)" 티어**다. 같은 등급이면 무기 종류가 달라도 동일 가격, 무기 종류는 유효 스탯·아이콘만 결정. 구매 시 `CurrentBasePrice` 저장 → `RequestUseCube`에서 `pr.baseOverride`로 넘겨 판매가 계산에 반영(PricingCalculator 시그니처 불변). ⚠ 등급 key(`Rare/Epic/Unique/Legendary`)는 잠재능력 등급과 문자열이 겹치나 별개 테이블(`weaponGrades` vs `gradeProb`)이다.
  - 슬롯/상세 아이콘은 아바타 무기 아이템 RUID → `SpriteGUIRendererComponent.ImageRUID`에 `"thumbnail://" .. ruid`로 설정.
  - 상점 보드 **표시/재추첨 시점**에 `_GameManager:RequestShopStuckCheck()`를 호출해 weapon 스턱 게임오버를 서버 판정(로비에서는 판정 안 함 — 아래 GameFlow 항목 참조).
  - **슬롯머신 스핀 연출(클라 전용)**: 새 `ShopSlotsData` 도착(`lastShopData` 변화) 시 즉시 렌더 대신 `StartSpin` — 슬롯별 `ReelMask`(마스크+ReelIcon 2장 leapfrog 세로 스크롤, 랩마다 무작위 무기 썸네일+슬롯 무작위 등급색 틴트)가 돌다가 **위→아래 순차 정지**(`SpinStopBase`/`SpinStopGap`, 정지 직전 감속). 정지 시 `RenderSlot` 복원+정지음+아이콘 펀치. 스핀 중 슬롯 클릭/구매 가드(`spinning`), 재입장(데이터 불변)은 스핀 없음, 화면 이탈 시 `FinishSpinInstant`. 증강 변화 재렌더는 스핀 중 보류→종료 시 일괄.
  - **등급 반짝 이펙트(`GetSparkleLevel`)**: 현재 확률표(`GetShopGradeProbTable(GetSyncedAugmentStage("betterGoods"))`)에 **레전더리>0이면(2~3단계) 유니크=L2/레전더리=L3(에픽은 일반)**, 닫혀 있으면(0~1단계) **에픽=L1/유니크=L2**. L1=별 팝 클립(`5c7fcd92…`)+플래시, L2=별 링 버스트(`58a7d489…`, 강화성공풍)+파티클+성공음(`b060035a…`), L3=링 대형+글리터 필드(`c4705494…`)+팡파레(`cb270655…`). SparkleFX는 animationclip RUID를 **접두사 없이** ImageRUID에 직접 설정(재생). FX 엔티티는 .ui에서 **켜진 채 저작**(꺼진 자식은 GetChildByName 캐시 누락 위험) → OnBeginPlay `ResetFXEntities`로 끔. 연출 튜닝은 전부 인스펙터 property(SSOT).
- **클릭 전용 캐릭터 처리** — `RootDesk/MyDesk/Player/ClickOnlyController.mlua`(`@Logic`, 클라). **모든 유저 엔티티** `Visible=false`(싱글게임 몰입 — `_UserService.UserEntities.Values` 순회, 레벨 구동이라 늦게 입장한 유저도 자동 커버) + 로컬 플레이어만 `PlayerControllerComponent.Enable=false`(이동 입력 차단). Global 모델은 읽기전용이라 런타임에서 처리.
- **복권 긁기(Scratch-ticket) UI** — `RootDesk/MyDesk/ScratchTicket/`, `ui/ScratchTicketGroup.ui`.
  `ScratchTicketManager.mlua`(`@Component`)가 3장의 은박 픽셀캔버스(`PixelGUIRendererComponent`)를 숨겨진 "당첨" 레이어 위에 깔고,
  클릭/드래그로 알파를 깎아 긁음(70% 도달 시 완료 이벤트). 격자해상도/브러시반경/강도/완료기준/대상맵은
  데이터테이블 `ScratchTicketConfig.userdataset`+`.csv`(`_DataService:GetTable`)로 구동, 없으면 인스펙터 기본값 폴백.
  (좌 시계·우 메소 상단 HUD는 아래 공용 HudManager 소관 — 스크래치 매니저에서 제거됨.)
  - **실시간 판매가 상승(우측 `PriceWindow`)**: 큐브 사용 즉시 판매가 창을 **기본 판매가(`CurrentBasePrice`)부터** 표시(`ResetPriceDisplay`) → 줄 공개마다 그 줄의 서버 보너스(`SerializePotential` 8번째 필드 `bonus`=`GetOptionBonus`)만큼 `priceTarget` 상승 → 3줄 완료 시 **서버 최종가 `CurrentPrice`로 점프**(등급·이탈 배수·라운딩 반영, 클라 계산 신뢰 안 함). `UpdatePriceScroll`이 ease-out 카운트업. 직렬화 포맷은 `"cubeCount;grade;line×3"`, line = `label|value|unit|optionType|isBonus|isDestroy|isValid|bonus|optionGroup`(9필드 — 9번째 `optionGroup`은 "MainStat"/"Attack"/"Junk"/"Destroy"로 결과음 5분기용).
  - **공개 타입별 연출(`PlayRevealFX`)**: 잡옵/비유효=회색 델타·소폭 펀치 / 유효=노랑 플래시·펀치+코인 소량 / 이탈(`isValid&&isBonus`)=보라 플래시·큰 펀치+코인 다량 / **파괴 줄=카운트업 정지(`priceFrozen`)+판매가 자리 "파괴" 빨강**(기존 파괴 시퀀스와 병행). 3줄 완료 최종 점프도 이탈 유무로 강조 차등.
  - **사운드(`docs/specs/soundlist.md` SSOT)**: 옵션 공개 결과음은 `PlayOptionSound(o)`가 `optionGroup`+`isValid`+`isBonus`로 5분기(Bad/Good/Jackpot/SuperJackpot/Legendary), 파괴는 `OnLineRevealed`에서 `Option_Destroy` 별도 재생. 긁는 동안 `Scratch_Loop` 루프음(`SetScratchLoop` 플래그 디듀프, `UpdateCursor` 에지+`SetContainersActive(false)`/`OnLineRevealed`/`OnEndPlay`에서 정지). 판매 결과창은 유효옵션 개수(0~3)로 `Sale_Low/Normal/High/Jackpot`. 클릭음: 큐브 사용·수령=Default, 판매 확정=Confirm. 무기 상점/증강 스핀은 `Weapon_Reveal_Loop` 루프음(`SetReelLoop`), BGM은 `Sound/BgmManager`(@Logic, 맵 에지에서 `PlayBGM`).
  - **동전 낙하 연출**: `CoinLayer`(루트 자식) + `Coin_1..20` 스프라이트 풀(코인 RUID `02a489cccff24a139a6c3582a5871f58`), `CrumbLayer` 패턴 복제. 유효/이탈/최종 점프 시 판매가 창 상단에서 pop-up 후 중력 낙하+회전(`SpawnCoins`/`UpdateCoins`), 짤랑음(`SfxCoin` `acb0f70275b7422dbcc8a395cbbd9d28`, 쿨다운). 상승 델타 "+n" 팝업은 `PriceWindow/PriceDelta`(`TextGUIRendererComponent`). 튜닝값 전부 인스펙터 property.
  - **큐브 구매창 타이틀 전환 + 구매 유도 화살표**: `CubeSelect/Title/UIText`(`cubeTitleText` 바인딩) 타이틀을 `SetCubeSelectShown(shown=true)` 단일 게이트에서 `_GameManager.HasPotential`로 분기 — 미사용(첫 구매)="큐브 구매" / 긁은 뒤 재노출="재구매?"(판매→새 무기 구매 시 서버 `ResetPotentialState`로 자동 복귀). 구매창 위 `CubeSelect/GuideArrow`(노란 아래 화살표+'클릭' 5프레임 애니메이션클립, `GuideArrowRUID` 인스펙터 SSOT `d34354e0…`)가 수직 사인 부유(`ArrowFloatAmp/Period`, `UpdateGuideArrow`) — CubeSelect 자식이라 슬라이드 인/아웃·렌더순서·상환 잠금 숨김을 자동 상속(별도 게이팅 없음).
  - **2단계 판매(판매 결과 창 `SaleResult`)**: 판매 버튼 → `RequestSellItem`(확정만, 버튼 비활성) → `IsSalePending` @Sync 에지 감지 → `ShowSaleResult`(무기 아이콘 `thumbnail://`+판매가+"수령하기" 버튼, 모달·긁기 차단) → `collectButton` → `RequestCollectSale`(메소 지급). 취소 없음. 아이템은 수령 전까지 유지되어 UI가 열린 채 있고, 수령 후 `CurrentItemId==""`로 게이팅이 UI를 닫아 상점 복귀(+정산 흐름은 수령 후 진행). `SaleResult`/`CoinLayer`는 `FixRenderOrder`의 `BringToFront`로 최상단 확정. (⚠ 신규 UIBuilder 텍스트=`TextGUIRendererComponent`. `ui/ScratchTicketGroup.ui`의 legacy `ConfirmSell`에 nested `UIGroupComponent`(lint L029)가 남아있어 UIBuilder write는 `strict:false` 필요 — 이번 작업이 만든 것 아님, 건드리지 말 것.)
- **상단 HUD(로비/상점/스크래치 공용) — `RootDesk/MyDesk/Hud/HudManager.mlua`(`@Logic`, 클라) + `ui/HudGroup.ui`.**
  InGameMap에서 상시 표시(맵 게이팅: `CurrentMapName=="InGameMap"`으로 `hudRoot.Enable`). 값은 전부 서버 권위 `_GameManager` @Sync.
  - 좌상단 라디얼 시계('현재 시간') = `used = WeekCubeCount`(이번 주 큐브 사용, 미러) → **n/N(올라가는 방식**, N = 주차별 상환 주기 `BlacksmithConfig.debtCycles = {5,6,6,7,7}` → `GetDebtCycle(DebtStage+1)`, 상환 시점 N/N. 상환 성공 시 서버가 `WeekCubeCount=0` 리셋). `TimerFill.FillAmount`로 채움.
  - 우상단 박스(`Hud` 컨테이너, 슬롯 간격 84px): `MesoBox`(y0) '현재 소유 메소'(충분/부족 색; 충분 판정 = `Meso >= required + Debt`) → `RepayBox`(y-84) '상환 필요 메소: {required}'(`required = PendingRepayment>0 ? PendingRepayment : GetDebtAmount(DebtStage+1)`) → **`DebtBox`(y-168) '현재 빚 : {Debt}'(`Debt>0`일 때만 Enable)**.
  - 임박 연출: 3/5부터 시계 흔들림, 4/5부터 중앙 라벨 빨강 깜빡 + 메소 부족(`required+Debt` 기준) 시 상환필요 텍스트 빨강↔골드(`UpdateWarningFX`).
  - **대출 유도 슬라이드 팝업(`LoanPopup`, 버튼+CanvasGroup)**: 상단 박스 뒤에서 스르륵 내려오는 '대출하러 가기!' CTA. `UpdateLoanPopup`이 OnUpdate에서 목표 y(빚 없으면 -168=RepayBox 아래 / 빚 있으면 -252=DebtBox 아래)로 선형 슬라이드, 숨김 y = 목표+84(위 박스 뒤). `WantLoanPopup`: 상점 화면(`ActiveScreen=="shop"`+미보유)이면 3슬롯 전부 실효 구매가 초과 / 스크래치(아이템 보유)면 이상한 큐브 구매불가 시 표시. `ActiveScreen=="finance"`·게임오버면 숨김. 도착 전/퇴장 중 `CanvasGroup.Interactable=false`(클릭 차단). 팝업은 `OnBeginPlay`에서 `_UILogic:SetSiblingIndex(…,1)`로 박스 뒤에 배치. 클릭 → `_FinanceManager:Open()`. `LoanPopup` 자식으로 게임오버 잠금 오버레이 `LockOverlay`(X사슬 2줄+자물쇠, 기본 숨김, 팝업 위치 자동 상속)가 있고 GameOverManager가 구동.
  - `HudGroup`은 `GroupType=1/GroupOrder=0`(다른 화면 그룹이 위에 얹힘). in-group `@Component`가 없어 루트 `Enable` 게이팅이 안전(LobbyGroup과 동일).
  - ⚠ 과거 이 HUD가 `WeaponShopManager`·`ScratchTicketManager`·`LobbyGroup`에 3벌 중복돼 있었으나 제거·통합함(UI Layer도 단일 `HudGroup`).
- **재정 관리(대출 서비스 센터) UI** — `RootDesk/MyDesk/Finance/FinanceManager.mlua`(`@Logic`, 클라), `ui/FinanceGroup.ui`(GroupType=1/GroupOrder=5, 최상위 모달).
  진입: HUD 대출 유도 팝업 클릭 또는 로비 '재정 관리' 버튼(`LobbyManager.OnFinanceClicked` → `_FinanceManager:Open()`). 종료: X 버튼 / 대출 성공(자동). 화면 상태는 `LobbyManager.ActiveScreen`을 `"finance"`로 전환(+`EnterActivity("finance")` 서버 정합), 복귀 시 `prevScreen`으로 되돌림(+`ReturnToLobby()`). InGameMap+`ActiveScreen=="finance"` 게이팅으로 `financeRoot.Enable`.
  - 보드: 노인 NPC(`thumbnail://` 런타임 설정)·말풍선·정보 3줄(지급액/늘어나는 빚/남은 횟수, 돈주머니 아이콘)·초록 '대출하기' 버튼. 지급액·빚은 `GetCurrentRepayment()`에 동적 연동(`RefreshInfo` 매 프레임 갱신).
  - '대출하기' → 경고 확인 팝업(`ConfirmPopup`, **'대출받기'(→`RequestLoan`) + '취소'(→`OnCancelConfirm`, 팝업만 닫음)**) → 대출 성공(`LoanUseCount` 증가) 감지 시 `OnUpdate`가 자동 `Close()`.
  - **잠금 오버레이(`LockOverlay`)**: `LoanUseCount >= maxUses`면 대출하기 버튼 위를 반투명+자물쇠로 덮고, 그 위 투명 `LockBtn`이 클릭을 가로챔 → 보드 흔들림(`shakeTime`) + 에러음(`_SoundService:PlaySound(errorSoundRUID)`), 대출 실행 안 함. (소진+구매불가 스턱은 이제 서버 `CheckStuckGameOver`가 게임오버로 처리 — GameFlow 참조.)
  - 사운드: 클릭 `972843e759204d3e9ad84e7d3fa94f83` / 에러 `174d501eccd04eadbd6c6411d4ade7e7`(msw-search UI 오류음).
- **대장 기술 업그레이드 UI(증강 뽑기)** — `RootDesk/MyDesk/Augment/AugmentManager.mlua`(`@Logic`, 클라), `ui/AugmentGroup.ui`(GroupType=1/GroupOrder=6, 모달).
  진입: 로비 '대장 기술' 버튼(`LobbyManager.OnSkillClicked` → `_AugmentManager:Open()`). 종료: X(`Close`). 화면 상태 `ActiveScreen="augment"`(+`EnterActivity`/`ReturnToLobby` 정합), 복귀 시 `prevScreen`. InGameMap+`ActiveScreen=="augment"` 게이팅으로 `augmentRoot.Enable`. FinanceManager 모달 패턴 복제.
  - **뷰 2개**(한 그룹 안 컨테이너, Enable 토글): `ChoiceView`(3택 창, 기본) / `OwnedView`(보유 목록, '보유 증강' 버튼으로 오픈).
  - **슬롯머신 스핀**(WeaponShopManager `StartSpin`/`UpdateSpin`/`StopSlot`/`FinishSpinInstant` 이식 — ReelMask+ReelIcon1/2 leapfrog 100px 세로 스크롤, `SpinStopBase`/`SpinStopGap` 위→아래 순차 정지, 정지 직전 감속). 트리거: `AugmentChoicesData`(첫 열람) 또는 `AugmentsData`(구매) 에지 변화 → 재스핀. **미구매 재진입은 두 데이터 불변이라 스핀 없음**(`lastChoicesData`/`lastOwnedData`). 스핀 중 하단 내용(이름/설명/단계/가격/구매버튼) 숨김 → **정지한 슬롯부터** `RenderSlotAt`으로 표시.
  - 각 슬롯: 아이콘(`d.augments.*.icon` plain RUID 직접 설정, thumbnail:// 없음) + 이름 + 도달단계("N단계") + 효과 설명(config `desc`, 수치 미표기) + 가격(`GetAugmentCost(GetSyncedAugmentStage+1)`) + 초록 '구매' 버튼(`OnBuyClicked`→메소 부족이면 클라 선차단 에러음+흔들림, 충분하면 `RequestSelectAugment`). **잔여 <3종이면 `AdjustSlotLayout`이 칸 수 축소+가운데 정렬**(빈 칸 아님). 전 소진 시 "모든 대장 기술을 마스터했습니다" 안내.
  - `OwnedView`: `AugmentsData` 파싱(`GetSyncedAugmentStage`)으로 보유 증강 행별(아이콘+이름 N단계+한 줄 효과) 렌더, 미보유 시 안내.
  - ⚠ **텍스트 컴포넌트: 최신 UIBuilder는 `TextGUIRendererComponent` 생성**(구버전 상점 UI는 `TextComponent`). `SetChildText`·`emptyChoiceText`/`emptyOwnedText` property는 `TextGUIRendererComponent`(`.Text`)로 접근. ⚠ **슬롯 구매버튼(`buyButtons[i]`)은 `GetChildByName`이 돌려준 엔티티**(컴포넌트 아님) — `.Entity` 붙이지 말고 엔티티로 다룸(`.Enable`/`.ButtonComponent`/`.SpriteGUIRendererComponent`). 릴/슬롯 자식은 GetChildByName 캐시(CacheSlots, 루트 켜진 상태), 상단 버튼만 UUID 바인딩.
- **게임 흐름 UI 3종(다음주 정산·게임오버·승리)** — `RootDesk/MyDesk/GameFlow/` + `ui/{NextWeekGroup,GameOverGroup,VictoryGroup}.ui` (기획: `finance_management.md` §5-1~§5-3, `game_flow_charts.md` §16).
  - **서버 확장(GameManager)**: `GameOverReason`("repayment"/"debt"/"weapon"/"cube") + `IsVictory` @Sync 신설. `IsSessionEnded()`(게임오버∨승리)가 전 액션 RPC 공용 가드. `RequestSettleDebt` 실패 시 원인 분기(메소 차감 없음 — 기록 보존), 성공 시 `DebtStage>=winStage(5)`면 승리(`RollAugmentChoices` 생략). `CheckStuckGameOver()`: 대출 소진 + (미보유→현재 3슬롯 전부 구매불가="weapon" / 보유→큐브 불가+판매로도 회생불가="cube") — **UpdateCanLoan 안에 넣지 말 것**(ResetSession 경유 시작 시 오발동). **호출 시점(유저 확정)**: weapon = `RequestShopStuckCheck`(클라 `WeaponShopManager`가 상점 화면 표시/재추첨 시점에 호출 — 로비/정산 직후 판정 금지) / cube = `RequestUseCube`·`RequestBuyItem` 말미. `RequestRestart`는 승리 상태도 수락. `RequestBuyItem`에 `IsRepaymentDue` 거부 추가.
  - **상환액 스케줄**: `debtTable{45k,70k,100k,140k}` + 표 밖 **매주 ×1.1**(`debtGrowth`, 1000단위 반올림). `debtStep` 폐기. `winStage=5`(상환 5회 = 승리).
  - **NextWeekManager**(@Logic 클라, GroupOrder 10): 오픈 조건 = InGameMap + `IsRepaymentDue` + `CurrentItemId==""`(**에지 래치** — 정산 실패 시 IsRepaymentDue가 true로 남아도 재오픈 방지). 오픈 시 `ActiveScreen="lobby"` 강제(판매 직후 상점 보드 노출 방지) + `ReturnToLobby()`. 1초 후 `RequestSettleDebt` → sync 폴링(성공=미납 해제 / 실패=IsGameOver, 2초 타임아웃) → 숫자 스크롤(ease-out): 메소 차감 → 빚 0 스크롤+CanvasGroup 페이드아웃 → 상환액 다음주 금액으로 상승 → 닫힘. 실패 시 부족 항목(상환액/빚) 2초 깜빡 → `_GameOverManager:Show(reason)`. `IsBusy()`로 게임오버/승리 UI가 연출 종료를 대기.
  - **GameOverManager**(GroupOrder 60 — ScratchTicketGroup 내부 ConfirmSell(50)보다 위 필수): 박스 낙하+바운스(중력 적분, `vy=-vy×0.45`). weapon/cube는 `IsGameOver` 에지 자동 감지 → `_HudManager:SetForceLoanPopup(true)`(`WantLoanPopup`이 게임오버 시 이 플래그를 반환) → **HudGroup `LoanPopup` 자식 `LockOverlay`(320×74, 팝업과 동일 크기·슬라이드 자동 상속)** 위에서 X사슬 2줄 낙하(사슬 `c6a2a498935647f485c8b013be53a84f` Tiled ±77°) + 자물쇠(`13122cb6462d4124b69434c90d9a92fd`)가 위에서 떨어져 착지(미니 중력+바운스) + 철컹 SFX(무거운 `8f610e071e87408a8326e849a88d0613` / 짧은 `ab731794c2e14e399b475767edd5c407`) → 그 후에야 게임오버 루트(딤+박스) 켜고 박스 낙하. (⚠ 유저 피드백으로 풀스크린 X사슬(GameOverGroup 소속)에서 이 방식으로 변경됨.) repayment/debt는 NextWeekManager가 `Show(reason)` 호출(5초 무응답 폴백). 재시작 → `ActiveScreen="lobby"` + `RequestRestart`, 리셋은 IsGameOver 하강 에지.
  - **VictoryManager**(GroupOrder 55): IsVictory 에지 + `_NextWeekManager:IsBusy()` 대기 후 스케일/페이드 인. 최종 메소/큐브/판매/최고가 + 다시 하기.
  - **스크래치 5/5 잠금**: `ScratchTicketManager.RefreshCubeLock()` — `IsRepaymentDue` 레벨 구동(변화 감지)으로 큐브 아이콘/구매 버튼 4개+재사용 비활성(판매만 가능), `UseCube` 클라 차단 병행, 잠금 중 호버 연출 오프.
- **시즌 패스(표시 중심 메커톤판)** — `RootDesk/MyDesk/SeasonPass/SeasonPassManager.mlua`(`@Logic` 클라), `ui/SeasonPassGroup.ui`(GroupType=1/**GroupOrder=6 — Augment와 공유**, ActiveScreen 상호배타라 무해), 기획 `docs/specs/seasonpass.md`, 애셋 RUID `RootDesk/MyDesk/images/Quest/Quest_RUID.txt`.
  - **서버(GameManager)**: 미러 2개 신설 `SeasonScore` + `SeasonMissionsData`("key:progress:cleared,…" — missionOrder 순 **6토큰 고정**, progress는 target 클램프·클리어 시 target 고정). 판정 통계는 서버 전용 세션 스칼라 `WeirdCubeUseCount`(이상한 큐브만, 재활용 발동 무관 — CubeUseCount와 의미 다름)/`TotalSellAmount`/`DestroyCount` + 중첩 `SeasonMissions`(cleared 맵). **`CheckSeasonMissions(s)` = config statKey 주도 범용 엔진(자동 클리어+점수, 클레임 RPC 없음)** — 훅 3곳: `DoUseCube` 말미(cube3/destroy1) / `DoCollectSale`(sell1/bigSale=MaxSellPrice/totalSale) / `DoSettleDebt` 성공 분기(debt1, **승리 early return보다 앞**). `ResetSessionFor` 말미 `SerializeSeasonMissions`로 초기 CSV 보장. **`RequestRestart`는 점수·클리어 미션만 보존**(미클리어 진행도는 새 판 통계 기준 리셋, 유저 확정 — "season kept" 로그). `EnterActivity` 화이트리스트에 `"seasonpass"` 추가됨(활동명 검증 있음 — 누락 시 warning 거부).
  - **Config SSOT**: `BlacksmithConfig d.seasonPass` = maxScore 4000 / missions 6종(cube3 300·sell1 200·bigSale 500·totalSale 700·destroy1 300·debt1 400, statKey=세션 필드명) / rewards **5지점(0점 '견습 대장장이 증표' 포함 — 컨셉 아트 5열, 유저 확정)**. reward.icon은 **완성 ImageRUID 문자열**(아바타 아이템은 thumbnail:// 접두 포함 저장, 클라 그대로 대입). `GetSeasonPass()`.
  - **클라 모달**: FinanceManager 패턴 복제(Open/Close + `ActiveScreen="seasonpass"` + prevScreen 복귀 + InGameMap 게이팅). 진입 = HUD 상단 중앙 **선물상자 버튼**(`HudGroup.ui BtnGift`, top-center [72,-18] 96px — HudManager가 배선 `OnGiftClicked`). 좌측 = 보상 카드 5열(카드 틀+아이콘+보상명+**'받기' 버튼: 도달 시만 클릭 가능, 클릭음+펀치 피드백 전용·지급 없음**) + 수평 진척도 바(`GaugeFill` sprite_type 3/fill_method 0, FillAmount=score/4000) + 톱니 노드 5개(달성/미달성 **ImageRUID 스왑**, 0점 항상 골드) / 우측 = 미션 정보 **스크롤 리스트**(`MissionPanel/MissionScroll` — OScroll 패턴 복제: 한 엔티티에 ScrollLayoutGroup+Mask+투명 raycast 스프라이트, `Type=1/vdir=3(0.0=상단)/ChildAlignment=1/Spacing=10`, 뷰포트 340×550 = **한 화면 4행**, 행 340×130 정적 자식 6개 — Vertical에선 CellSize 무시·자식 rect가 행 높이). 행 = 체크박스 44 + Name 24 bold(BestFit 16~24) + Reward 20 + Desc 17(BestFit) + Progress 15(BestFit). 휠 스크롤은 AugmentManager 3종 세트(`OnMouseScroll` 목표 누적 + `UpdateWheelScroll` 감속 보간 + Open 시 상단 리셋) 복제. 미션/보상 텍스트·아이콘은 .ui 하드코딩 없이 config 주입. 반복 요소는 `GetChildByName` 캐시(루트 켜진 채 저작→OnBeginPlay 캐싱 후 끔, Row는 재귀 탐색이라 스크롤 아래로 이동해도 동작), 텍스트는 `TextGUIRendererComponent`.
  - 연동 수정: `HudManager.WantLoanPopup`·`ScratchTicketManager` 게이팅에 `"seasonpass"` 숨김 추가(모달 아래 CTA/커서 잔상 방지). LobbyManager는 무수정(허브는 lobby만 표시라 자동 숨김).

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
