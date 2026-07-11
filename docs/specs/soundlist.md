# 사운드 리소스 리스트

## 1. BGM

| 구분       | 사용 위치          | RUID                               |
| -------- | -------------- | ---------------------------------- |
| 타이틀 화면   | 게임 타이틀 화면에서 재생 | `3def13e49e10434282979764c612440a` |
| 메인 게임플레이 | 메인 게임 진행 중 재생  | `bbbf2616f2064d658f4d53af5faf9d3e` |

---

## 2. SFX

## UI 클릭음

### UI_Click_Light

* 역할: 도움말, 창 닫기, 탭 이동 등 결과에 영향을 주지 않는 가벼운 버튼 클릭
* RUID: `0868b0f43f1949c1ace06ae4fa33ee55`

### UI_Click_Default

* 역할: 일반 메뉴 선택, 무기 선택, 옵션 선택 등 가장 자주 사용하는 기본 클릭
* RUID: `b2b0b752bf914fb5abd885470c84c8fc`

### UI_Click_Confirm

* 역할: 판매 확정, 대출 실행, 증강 구매, 빚 상환 등 중요한 선택 확정
* RUID: `69f85d65004541db82615459032e1d4a`

---

## 스크래치

### Scratch_Loop

* 역할: 잠재옵션 은박을 긁고 있는 동안 재생되는 스크래치 효과음
* 재생 방식: 긁는 동안 재생하고, 마우스를 떼거나 스크래치가 끝나면 정지
* RUID: `5ffcdd8177a943638529749d26627f67`

---

## 잠재옵션 공개 결과음

### Option_Bad

* 역할: 최종 판매 가치가 낮거나 별로인 옵션 결과
* RUID: `a6f91022d4ca4238bcda3bc9fcafabc3`

### Option_Good

* 역할: 꽤 좋은 수준의 옵션 결과
* RUID: `ce686c2a652744cdbf1aa9f66399c843`

### Option_Jackpot

* 역할: 대박 수준의 옵션 결과
* RUID: `756bec092ef6470c98a30dd564219259`

### Option_SuperJackpot

* 역할: 초대박 수준의 옵션 결과
* RUID: `d048171b80bc497292892b37229ca9dc`

### Option_Legendary

* 역할: 최상위 전설급 옵션 결과
* RUID: `f5fb4f53733a4327a6c4ba1445a6fced`

### Option_Destroy

* 역할: 장비가 파괴된 결과
* RUID: `4d3d9de1b72d48eeb9f76f3a144a9bee`

### 재생 규칙

* 옵션 결과음은 옵션 3줄이 모두 공개되고 최종 가치가 판정된 순간 한 번만 재생한다.
* 옵션 1줄마다 개별로 재생하지 않는다.
* 파괴 결과가 발생한 경우 `Option_Destroy`를 우선 재생한다.

---

## 무기 판매 결과음

### Sale_Low

* 역할: 적은 금액으로 판매됐을 때
* RUID: `66f8bbbc1d6a4166973774960d7b1ea1`

### Sale_Normal

* 역할: 보통 수준의 금액으로 판매됐을 때
* RUID: `d879b6be959b4586b3a428b626605605`

### Sale_High

* 역할: 좋은 금액으로 판매됐을 때
* RUID: `9b6e608ee81043f8bb045b07509f443d`

### Sale_Jackpot

* 역할: 매우 높은 금액으로 판매됐을 때
* RUID: `2ba4a50df8df4b919b5670d48b72159f`

### 재생 규칙

* 판매 버튼을 누를 때 `UI_Click_Confirm`을 먼저 재생한다.
* 판매 결과 판정 후 판매 금액 구간에 맞는 결과음을 재생한다.
* 두 소리가 겹쳐 답답하게 들릴 경우 `UI_Click_Confirm`의 볼륨을 낮추거나 생략한다.

---

## 무기 상점

### Weapon_Reveal_Loop

* 역할: 무기 상점에서 무기가 공개되기 전 슬롯머신처럼 드르르륵 돌아가는 대기음
* 재생 방식: 무기 공개 애니메이션 동안 재생하고, 무기가 등장하는 순간 정지
* RUID: `7c73a090bd1d4bfe8fb860b2ad44031d`

---

## 게임 종료

### Game_Defeat

* 역할: 게임 패배 결과 화면이 표시될 때
* RUID: `6e462a4ae44b4a518ea68fde456ea5c3`

### Game_Victory

* 역할: 게임 승리 결과 화면이 표시될 때
* RUID: `56cbe200a9c7477fb54b0a87e531b98c`

---

## 공용 클릭음 사용 기능

아래 기능은 별도 전용 효과음 없이 `UI_Click_Confirm`을 사용한다.

### 대출 실행

* 사용 효과음: `UI_Click_Confirm`
* RUID: `69f85d65004541db82615459032e1d4a`

### 증강 구매

* 사용 효과음: `UI_Click_Confirm`
* RUID: `69f85d65004541db82615459032e1d4a`

### 빚 상환

* 사용 효과음: `UI_Click_Confirm`
* RUID: `69f85d65004541db82615459032e1d4a`

---

## 3. 사운드 적용 기준 요약

| 상황                 | 사용 사운드                |
| ------------------ | --------------------- |
| 가벼운 UI 클릭          | `UI_Click_Light`      |
| 일반 메뉴 / 무기 / 옵션 선택 | `UI_Click_Default`    |
| 중요한 선택 확정          | `UI_Click_Confirm`    |
| 잠재옵션 스크래치 중        | `Scratch_Loop`        |
| 낮은 가치 옵션 결과        | `Option_Bad`          |
| 좋은 옵션 결과           | `Option_Good`         |
| 대박 옵션 결과           | `Option_Jackpot`      |
| 초대박 옵션 결과          | `Option_SuperJackpot` |
| 전설급 옵션 결과          | `Option_Legendary`    |
| 장비 파괴              | `Option_Destroy`      |
| 낮은 금액 판매           | `Sale_Low`            |
| 보통 금액 판매           | `Sale_Normal`         |
| 높은 금액 판매           | `Sale_High`           |
| 매우 높은 금액 판매        | `Sale_Jackpot`        |
| 무기 상점 공개 대기        | `Weapon_Reveal_Loop`  |
| 게임 패배              | `Game_Defeat`         |
| 게임 승리              | `Game_Victory`        |
