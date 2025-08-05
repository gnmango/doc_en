UI Components/Logics

# ItemToastMessage Logic
**아이템 ?개 획득**과 같은 토스트 메시지를 띄우는 로직입니다. UI 엔티티를 배치한 상태로 사용하기 위해 SyncTable을 활용해 관리합니다.
`{MessageTable, RUIDTable, TimeTable}`이 한 쌍이며, 새로운 메시지를 추가할 때 반드시 3개를 한 쌍으로 추가해야 합니다. 또한 새로운 메시지는 반드시 테이블 맨 앞에 추가해야 합니다.

#### Property
| 실행제어 | 타입 | 이름 | 설명 |
| --- | --- | --- | --- |
| None | SyncTable < string > | MessageTable| 화면에 띄울 메시지입니다. |
| None | SyncTable < string > | RUIDTable | 화면에 띄울 아이템 아이콘입니다. |
|None  | SyncTable < number > | TimeTable | 남은 메시지가 표시될 시간입니다. |
| None | SyncTable < Entity > | messageEntity | 메시지를 표시할 엔티티입니다. |

#### Function
| 실행제어 | 타입 | 이름 | 설명 |
| --- | --- | --- | --- |
| client only | void | OnBeginPlay | 프로퍼티를 초기화하고 **self.messageEntity**에 넣을 엔티티를 찾아 테이블에 넣습니다. |
| client only | void | OnUpdate | 실시간으로 아이템 획득 로그를 관리합니다. 표시할 메시지와 RUID 정보를 UI에 반영합니다. 남은 시간에 따라 UI 투명도를 설정합니다. |
| client| void | AddMessage | 새로운 메시지를 추가합니다. `PlayerData:PlayGetItemDirection()` 함수에서 호출됩니다. <ul><li>message: 입력할 내용입니다.</li> <li>ruid: 해당 로그에 보여질 아이템 또는 재화의 RUID입니다.</li></ul> |

# UI_EquipmentShop
장비 상점 UI 관련 컴포넌트입니다.
#### Property
| 실행제어 | 타입 | 이름 | 설명 |
| --- | --- | --- | --- |
| None |  number| FilterIdx | 장비를 타입별로 분류해 상품 목록을 볼 수 있습니다. |
| None | number | ShowingEquipmentLevel | 상품을 눌러 상세 정보를 확인할 수 있습니다. 상세 정보를 확인 중인 장비의 레벨을 표시합니다. 기본값은 0입니다. |
| None | boolean | IsPlayingDirection | 장비 구매 연출이 재생 중인지 판단합니다. |

#### Function
| 실행제어 | 타입 | 이름 | 설명 |
| --- | --- | --- | --- |
| client only| void | OnBeginPlay | 월드에 진입할 때 UI를 끄는 함수입니다. |
| client |void  | RefreshUI | 보유 재화량, 필터 버튼, 상품 목록을 갱신하는 함수입니다. 장비를 구매하거나 장비 상점을 닫았다 다시 열었을 때 혹은 타입 버튼을 눌렀을 때 실행됩니다. |
| client | void | ShowDetailPopup | 플레이어가 상품 목록 중 하나를 눌렀을 때 실행하는 함수입니다. 상세 정보 팝업을 갱신하고 활성화 합니다. 상품의 구매 비용과 내 보유 금액을 비교해 구매 가능하다면 버튼을 활성화 합니다. |
| client | void | PlayPurchaseDirection | 장비 구매 완료 연출을 재생하는 함수입니다. |
#### Entity Event Handler
| 실행제어 | 이벤트 중계자 | 이름 | 설명 |
| --- | --- | --- | --- |
|  - | InputService| KeyDownEvent | Esc 키를 누르거나, 닫기 버튼을 눌러 UI를 닫습니다. |
|  -| [entity] /ui/EquipmentShopGroup/EquipmentShop<br>/DetailInfoPopup/Panel/Button_Buy | ButtonClick | 상세 정보 팝업에 있는 구매 버튼을 눌렀을 때 실행합니다. `self.FilterIdx`와 `self.ShowingEquipmentLevel` 값에 따라 장비 구매를 시도합니다. 구매 시도를 하면 상세 정보 팝업을 닫습니다.|

# UI_EquipmentShop_FilterButton
장비 상점 UI의 **장비 타입 필터** 버튼에 붙어있는 컴포넌트입니다.
#### Entity Event Handler
| 실행제어 | 타입 | 이름 | 설명 |
| --- | --- | --- | --- |
| - | self | ButtonClickEvent | 버튼을 누르면 UI_EquipmentShop의 `FilterIdx`를 변경하고, `RefreshUI()`를 실행합니다.  |
# UI_EquipmentShop_SlotButton
장비 상점 UI 내 상품 목록마다 붙어있는 컴포넌트입니다.
#### Entity Event Handler
| 실행제어 | 이벤트 중계자 | 이름 | 설명 |
| --- | --- | --- | --- |
| - | self | ButtonClickEvent | 각 상품 목록을 누르면 `UI_EquipmentShop:ShowDetailPopup()`을 실행합니다. |
# UI_EquipmentStorage Logic
장비 보관함 UI와 관련된 로직입니다.
#### Property
| 실행제어 | 타입 | 이름 | 설명 |
| --- | --- | --- | --- |
| None | number | FilterIdx | 장비 보관함 UI에 표시할 장비의 1차 유형 분류 번호힙니다. 기본값은 1입니다. |
| None | number | SecondFilterIdx | 장비 보관함 UI에 표시할 장비의 2차 유형 분류 번호입니다. 기본값은 1입니다. <ul><li>1: 곡괭이</li> <li>2: 작업복</li> <li>3: 가방</li> <li>4: 신발</li></ul> |
| None | number | ShowingItemIdx | 장비 상세 정보 팝업에 보이는 아이템의 **Idx**입니다. 기본값은 0입니다. |

#### Function
| 실행제어 | 타입 | 이름 | 설명 |
| --- | --- | --- | --- |
| client only| void |  OnBeginPlay | 월드에 진입할 때 장비 보관함 UI를 끄는 함수입니다. |
| client | void | SetFilterIdx | 장비 보관함 UI에서 1차 타입 필터 버튼을 누르면 장비 목록과 장착 정보를 **idx**에 맞게 변경합니다. |
| client | void |  SetSecondFilterIdx| 장비 보관함 UI에서 2차 타입 필터 버튼을 누르면 장비 목록과 장착 정보를 **idx**에 맞게 변경합니다. |
| client | void | RefreshUI| 전체 UI를 현재 데이터에 맞게 변경합니다. 클라이언트에서 데이터가 변경된 경우 수동으로 호출해야 합니다. |
| client | void | ShowDetailInfo | 장비 목록에서 장비를 눌렀을 때 상세 정보 팝업의 내용을 변경하고, UI를 변경합니다. |
| client | void | ShowMessageEquipmentChanged| 장비를 장착하거나 해제한 경우 토스트 메시지를 띄웁니다.|
| client | void | ApplyAvatarEntity | 장착한 장비가 변경되면 장비 보관함 내에 있는 아바타 표시를 변경합니다. |

#### Entity Event Handler

| 실행제어 | 이벤트 중계자 | 이름 | 설명 |
| --- | --- | --- | --- |
| - | [entity]  /ui/HUDGroup/TownHUD/Panel_menu3<br>/ScrollList/EquipmentButton | ButtonClickEvent | 마을 메뉴 UI에서 **장비 보관함** 버튼을 누르면 호출되는 핸들러입니다. 메뉴 목록을 닫고, 장비 보관함 UI를 새로고침 해 엽니다.  |
| - | service| KeyDownEvent | 장비 보관함이 열려 있을 때 Esc 키를 눌러 닫습니다. 상세 정보 창이 열려 있는 경우 상세 정보 창만 닫습니다. |
| - | [entity]  /ui/InventoryGroup/EquipmentStorage<br>/DetailInfoPopup/Button_Equip | ButtonClickEvent2| 상세 정보 팝업에서 '장착하기' 버튼을 눌렀을 때 호출됩니다. 선택한 장비로 장착 장비 **Idx**를 변경하고, 팝업창을 닫습니다.  |
| - | [entity] /ui/InventoryGroup/EquipmentStorage<br>/DetailInfoPopup/Button_Equip_dismount | ButtonClickEvent3 | 상세 정보 팝업에서 **장착 해제** 버튼을 눌렀을 때 호출되며 장비 장착을 해제하고 팝업창을 닫습니다.  |


# UI_EquipmentStorage_FilterButton
장비 보관함 UI의 1차 필터 버튼마다 붙어있는 컴포넌트입니다.
#### Entity Event Handler
| 실행제어 | 이벤트 중계자 | 이름 | 설명 |
| --- | --- | --- | --- |
| - | self | ButtonClickEvent| 버튼을 누르면 UI_EquipmentStorage의 `SetFilterIdx(idx)`를 호출합니다. |
# UI_EquipmentStorage_SecondFilterButton
장비 보관함 UI의 2차 필터 버튼마다 붙어있는 컴포넌트입니다.

#### Entity Event Handler
| 실행제어 | 이벤트 중계자 | 이름 | 설명 |
| --- | --- | --- | --- |
| - | self | ButtonClickEvent| 버튼을 누르면 UI_EquipmentStorage의 `SetSecondFilterIdx(idx)`를 호출합니다. |
# UI_EquipmentStorage_SlotButton
장비 보관함 UI의 보관 목록 아이템마다 붙어있는 컴포넌트입니다.

#### Entity Event Handler
| 실행제어 | 이벤트 중계자 | 이름 | 설명 |
| --- | --- | --- | --- |
| - | self | ButtonClickEvent| 버튼을 누르면 UI_EquipmentStorage의 `ShowDetailInfo(idx)`를 호출합니다. |

# UI_GameResult
광산에서 탐험 종료될 때 등장하는 결과 UI 관련 컴포넌트입니다.
#### Function
| 실행제어 | 타입 | 이름 | 설명 |
| --- | --- | --- | --- |
| client only| void |  OnBeginPlay | 월드에 진입할 때 UI를 끄는 함수입니다. |
| client | void | SetResultUI | 게임 결과 화면을 갱신하는 함수입니다. 광산 내에서 사망하면 `PlayerMineData:Die()`에서 호출합니다. 귀환 로봇을 사용하면 `Portal_MineToTown:HandleInteractionEvent2()`에서 함수를 호출합니다. <ul><li>isSuccess: 성공 여부를 의미합니다. 사망해 호출한 경우 false, 귀환 로봇으로 호출한 경우 true입니다.</li><li> depth: 최대 깊이입니다.</li><li>time: 머무른 시간입니다.</li> <li>items: 이번 탐험에서 얻은 또는 잃어버린 광물 목록입니다.</li></ul>|
#### Entity Event Handler
| 실행제어 | 이벤트 중계자 | 이름 | 설명 |
| --- | --- | --- | --- |
| - | InputService | KeyDownEvent | 게임 결과 UI가 열려 있을 때 **탐험 종료** 버튼이 활성화 되어 있다면, E 키를 눌러 UI를 닫고 마을로 텔레포트 시킵니다.|

# UI_GameResult_ItemSlotButton
광산 결과 UI 화면에 있는 아이템 목록마다 붙어있는 아이템 정보를 보여주는 컴포넌트입니다.
각 아이템에 마우스를 올리거나 화면을 꾹 누르면 `ButtonStateChangeEvent`를 호출합니다.

#### Entity Event Handler
| 실행제어 | 이벤트 중계자 | 이름 | 설명 |
| --- | --- | --- | --- |
| - | self | ButtonStateChangeEvent | 아이템을 누르면 아이템 정보를 보여줍니다. PC에서는 목록에 마우스를 올리면 보이고, Mobile에서는 목록을 눌러 볼 수 있습니다.  |
# UI_HUD
HUD와 관련된 컴포넌트입니다.
#### Property
| 실행제어 | 타입 | 이름 | 설명 |
| --- | --- | --- | --- |
| None | Entity | Power | 채광력을 표시하는 엔티티입니다. |
| None | Entity | Money | 보유한 골드를 표시하는 엔티티입니다. |
| None | Entity | HPFill | 체력을 표시하는 UI 엔티티입니다. |
| None | Entity | HPText | 체력을 텍스트로 표시하는 엔티티입니다. |
| None | Entity | MPFill | 기력을 표시하는 UI 엔티티입니다. |
| None | Entity | MPText | 기력을 텍스트로 표시하는 엔티티입니다. |
| None | Number | UpdateTime | 다음 UI 갱신까지 남은 시간을 의미합니다. |

#### Function
| 실행제어 | 타입 | 이름 | 설명 |
| --- | --- | --- | --- |
| client | void | RefreshUI | HUD를 현재 데이터에 맞게 갱신하는 함수입니다.|
| client only | void | OnUpdate | HUD를 갱신하기 위한 함수입니다. 0.1초마다 RefreshUI를 호출합니다. |
| client | void | RefreshToolUI | 보유한 탐험 도구 수량을 갱신하고, 수량에 따라 이미지를 다르게 표시합니다. 탐험 도구를 획득하거나 소모할 때 호출됩니다. |
| client | void | PlayToolEffect | HUD 탐험 도구 아이콘에서 이펙트를 재생하는 함수입니다. 탐험 도구를 획득하거나 소모할 때 호출합니다. <li>idx: 획득, 소모한 탐험 도구 인덱스 </li>|
#### Entity Event Handler
| 실행제어 | 이벤트 중계자 | 이름 | 설명 |
| --- | --- | --- | --- |
| - | InputService | KeyDownEvent | 플레이어가 마을에 있을 때 `F3`을 눌러 메뉴를 열고 닫습니다. |
# UI_Inventory
인벤토리 관련 컴포넌트입니다.
#### Property
| 실행제어 | 타입 | 이름 | 설명 |
| --- | --- | --- | --- |
| None |number  | ShowingFilterIdx | 표시할 아이템 타입을 분류하는 인덱스입니다.|
| None | number | SelectedSlotIdx | 상세 정보를 보여주고 있는 아이템의 인덱스입니다. |

#### Function
| 실행제어 | 타입 | 이름 | 설명 |
| --- | --- | --- | --- |
| client only | void |OnBeginPlay | 프로퍼티를 초기화하고, 인벤토리 UI를 닫습니다. | 
| client | void  |RefreshUI | 인벤토리 UI를 새로고침합니다. `self.ShowingFilterIdx`에 해당하는 아이템들을 보여줍니다. 새로운 아이템을 얻거나, UI를 열거나, 필터 버튼을 누를 때 새로운 정보로 갱신합니다. |
| client | void |ShowDetailPopup | 보유 중인 아이템 목록에서 아이템을 누르면 상세 정보 팝업창을 엽니다. | 
#### Entity Event Handler
 실행제어 | 이벤트 중계자 | 이름 | 설명 |
| --- | --- | --- | --- |
| - | self  | KeyDownEvent | `I` 키를 눌러 UI를 열고, 닫습니다. `Esc` 키를 눌러 UI를 닫습니다. 이때 상세 정보 팝업창이 열려 있다면, 팝업창만 닫습니다. |
| - | [entity] ExitButton (/ui/InventoryGroup/Inventory<br>/Panel/TitlePanel/ExitButton)  | ButtonClickEvent | 닫기 버튼을 눌러 UI를 비활성화 합니다. |
# UI_Inventory_FilterButton
인벤토리의 필터 버튼에 달려 있는 컴포넌트입니다.
#### Entity Event Handler
| 실행제어 | 타입 | 이름 | 설명 |
| --- | --- | --- | --- |
| - | self | ButtonClickEvent | `UI_Inventory.ShowingFilterIdx`를 변경하고 `UI_Inventory:RefreshUI()`를 실행합니다. |
# UI_Inventory_SlotButton
인벤토리의 각 아이템에 달려 있는 컴포넌트입니다.
#### Entity Event Handler
| 실행제어 | 타입 | 이름 | 설명 |
| --- | --- | --- | --- |
| - | self | ButtonClickEvent | `UI_Inventory:ShowDetailPopup(idx)`를 실행합니다. |
# UI_Loading Logic
월드에 처음 접속하거나, 마을에서 마을로 이동하거나, 광산에서 마을로 돌아올 때 보여지는 로딩 화면과 관련된 로직입니다.
#### Property
| 실행제어 | 타입 | 이름 | 설명 |
| --- | --- | --- | --- |
| None | boolean | IsLoading | 로딩 창이 열려 있는 상태인지 확인합니다. `SetEnableLoading()`함수에서 값을 변경합니다.|

#### Function
| 실행제어 | 타입 | 이름 | 설명 |
| --- | --- | --- | --- |
| client | void | SetEnableLoadingUI | 로딩 화면을 활성, 비활성하는 함수입니다. 활성화하면 로딩 화면의 모든 UI 투명도를 1로 만들고, 플레이어의 이동을 막습니다. 비활성화 시 0.5초 동안 점진적으로 로딩 UI를 투명하게 만들고, 플레이어 엔티티를 이동할 수 있게 합니다.|
| client only | void | OnBeginPlay | 월드에 처음 접속했을 때  `self:SetEnableLoadingUI(true)`를 호출하고, `TimerService`를 사용해 `self:SetEnableLoadingUI(false)`를 3초 뒤 호출합니다. |

# UI_MapEnteredManager
UI를 비활성화하는 컴포넌트입니다. 마을이나 광산을 이동할 때 사용합니다.

#### Property
| 실행제어 | 타입 | 이름 | 설명 |
| --- | --- | --- | --- |
| None | boolean | AutoDisableOnTown | LocalPlayer가 마을에 입장할 때 불필요한 UI인 경우 true로 설정합니다.|
| None | boolean | AutoDisableOnMine | LocalPlayer가 광산에 진입할 때 불필요한 UI인 경우 true로 설정합니다.|

#### Entity Event Handler
| 실행제어 | 이벤트 중계자 | 이름 | 설명 |
| --- | --- | --- | --- |
| -  | LocalPlayer | LocalPlayerMapEntered | 플레이어가 다른 맵에 진입할 때마다 `PlayerData:OnMapEnter()`에서 `LocalPlayerMapEntered` 이벤트를 플레이어 엔티티에 보내고, UI를 비활성화 합니다. |
# UI_MineralExchange
광물 상점 UI와 관련된 컴포넌트입니다.
#### Property
| 실행제어 | 타입 | 이름 | 설명 |
| --- | --- | --- | --- |
| client only | void  | OnBeginPlay | 월드에 처음 진입할 때 엔티티를 `disable`로 변경합니다. |
| client | void | RefreshUI | 광물 상점 UI를 갱신하는 함수입니다. 광물을 모두 판매하거나, 광물 상점 UI를 열 때 호출합니다. |

#### Entity Event Handler
| 실행제어 | 이벤트 중계자 | 이름 | 설명 |
| --- | --- | --- | --- |
| - | InputService | KeyDownEvent | `E` 키를 눌러 광물 상점에서 보유한 모든 광물을 판매합니다. `Esc` 키를 눌러 광물 상점 UI를 닫습니다. 광물 판매는 서버에서 이뤄집니다.|
# UI_MiningGame
채광 시 실행되는 미니 게임 관련 컴포넌트입니다.
#### Property
| 실행제어 | 타입 | 이름 | 설명 |
| --- | --- | --- | --- |
| None | boolean | IsKeyBox |  현재 진행 중인 미니 게임이 열쇠 상자를 획득할 수 있는 게임인지를 확인합니다. |
| None | boolean | IsPlaying| 미니 게임 진행 여부를 판단합니다. 값이 true라면 `OnUpdate()`에서 화살표를 움직입니다.  |
| None | number | LeftHitChance | 남은 타격 기회입니다. 일반 광맥 타격 기회는 5회, 열쇠 상자는 10회입니다. <br> Hit()에서 남은 광맥 체력은 0 이상이나, `LeftHitChance` 프로퍼티가 0 이하인 경우 미니 게임을 종료합니다. |
| None | number | OreHPMax | 채광 중인 광맥의 최대 체력입니다. |
| None | number | OreHP | 채광 중인 광맥의 남은 체력입니다. |
| None | boolean | ContainBonusGame | 보너스 게임이 포함된 미니 게임인지 판단합니다. 광맥과 처음 상호 작용 했을 때 보너스 게임 활성화 여부를 결정합니다. 보너스 게임이 있을 경우 값을 true로 변경합니다. |
| None | boolean | IsPlayingBonusGame | 보너스 게임이 진행 중인지 판단합니다. 보너스게임은 `self.ContainBonusGame`이 **true** 이고, 일반 타격을 모두 성공했을 때 진행합니다. `Hit()`에서 채광을 성공했을 때 `self.ContainBonusGame` 이 **true**면 `IsPlayingBounesGame` 값을 true로 변경합니다. |
| None | Entity | TimingArrow | `OnUpdate()`에서 움직일 화살표 UI 엔티티입니다. |
| None | Entity | CurrentTarget | 채광 중인 광맥 엔티티입니다. |
| None | number | PlayingTime | `OnUpdate()`에서 화살표 UI의 위치를 결정하기 위한 시간 값입니다.|

#### Function
| 실행제어 | 타입 | 이름 | 설명 |
| --- | --- | --- | --- |
| client | void | Initialize | 채광 게임을 시작하기 위해 각종 프로퍼티를 초기화하는 함수입니다. 광맥과 상호 작용 시 호출됩니다 <ul><li>VeinLevel: 상호 작용한 광맥의 레벨</li> <li>VeinEntity: 상호 작용한 광맥 엔티티</li> <li>IsKeyBox: 광맥의 열쇠 보유 여부</li></ul> |
| client | void | SetTimingBarPosition | 미니 게임에서 타이밍 영역 위치를 무작위로 조절하는 함수입니다. 보너스 게임이라면 기본 게임 UI를 끄고, 보너스 게임 UI를 활성화합니다.  |
| client only | void | OnBeginPlay | `self.TimingArrow`에 엔티티를 할당합니다. |
| client only | void | OnUpdate | 미니 게임 중 화살표 위치를 왕복해 움직이게 합니다. `self.PlayingTime += delta`를 더한 값의 위치로 화살표를 이동시킵니다.|
| client | void | Hit | 타격 적중을 처리하는 함수입니다. 플레이어가 `채광하기` 버튼, `E` 키를 눌렀을 때 호출됩니다.|
| client | void | ExitGame | 미니 게임을 종료시키는 함수입니다. 게임 도중 `Esc`를 누르거나, `self:Hit()`에서 성공/실패 조건에 맞으면 호출됩니다.|
| client | void | ForcedExitGame | 미니 게임을 강제로 종료시키는 함수입니다. 게임 중에 플레이어가 사망하면 호출됩니다. |
#### Entity Event Handler
| 실행제어 | 이벤트 중계자 | 이름 | 설명 |
| --- | --- | --- | --- |
| - | InputService | KeyDownEvent | 처음 미니 게임 UI가 활성화 되고, 플레이어가 `E`키를 누르면 미니 게임을 시작합니다. 미니 게임 중엔 `E`키를 눌러 타격합니다. |
| - | InputService | KeyDownEvent2 | 미니 게임 UI를 `Esc` 키를 눌러 닫습니다. 게임이 진행 중이었다면 강제 종료하고 UI를 닫습니다. |
# UI_RainbowText
`TextComponent` 텍스트 컬러가 일정 주기로 변경됩니다.
#### Property
| 실행제어 | 타입 | 이름 | 설명 |
| --- | --- | --- | --- |
| None | Number | Time | 색상을 일정 주기로 변경하기 위해 사용하는 프로퍼티입니다. |

#### Function
| 실행제어 | 타입 | 이름 | 설명 |
| --- | --- | --- | --- |
| client only | void | OnUpdate |  3초 주기로 색상을 변경합니다. |

# UI_Status Logic
내 능력치 UI와 관련된 로직입니다.

| 실행제어 | 타입 | 이름 | 설명 |
| --- | --- | --- | --- |
| client only | void | OnBeginPlay | 월드 처음 진입 시 능력치 UI를 닫습니다. |
| client | void | RefreshUI | 능력치 UI를 새로 고침합니다. 서버에서 능력치를 갱신할 때마다 함수를 호출합니다. <ul><li>base: 능력치의 기본값입니다.</li><li>increase: % 증가량입니다.</li><li>final: base에 increase 값을 계산해 적용한 최종값입니다.</li></ul>
#### Entity Event Handler
| 실행제어 | 이벤트 중계자 | 이름 | 설명 |
| --- | --- | --- | --- |
| - | InputService | KeyDownEvent | `Esc`를 눌러 UI를 닫습니다. |
| - | [entity] /ui/HUDGroup/TownHUD/Panel_menu3<br>/ScrollList/StatusButton | ButtonClickEvent | **능력치** 버튼을 누르면 이 핸들러를 실행합니다. 메뉴 UI를 닫고, 능력치 UI를 엽니다.|