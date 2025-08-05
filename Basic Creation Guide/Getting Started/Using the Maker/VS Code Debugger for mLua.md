VS Code: Debugger for mLua

# 학습 과정 소개
mLua Debugger VS Code Extension을 설치해 제작 중인 월드를 VS Code에서 디버깅해봅시다.
##### 참고 가이드
* [스크립트 디버그]
* [LocalWorkspace]
* [mLua VS Code Extension]

# Debugger for mLua Extension
**Debugger for mLua**는 로컬워크스페이스를 사용하는 월드의 mLua 파일을 VS Code에서 디버그할 수 있도록 지원하는 확장 프로그램입니다. mLua Extension 설치와 메이플스토리 월드의 LocalWorkspace 연동 기능이 활성화되어 있어야 Debugger for mLua를 사용할 수 있습니다. 
# Debugger for mLua Extension 설치
1. Extensions를 선택하고 **Debugger for mLua**를 검색해 설치합니다.
2. 월드의 로컬 폴더에서 `.vscode` 폴더와 `launch.json` 파일이 있는지 확인합니다.
3. `launch.json` 파일을 열어 아래와 같이 입력합니다. configurations이 아래와 같이 작성되어야 VS Code의 **Run and Debug** 탭이 활성화됩니다.

    ```lua
    {
        "version": "0.2.0",
        "configurations": [
            {
                "name": "MSW Attach",
                "type": "msw",
                "request": "attach"
            }
        ]
    }
    ```
# 디버그하기
Debugger for mLua는 MSW에 VS Code Debugger를 연결하는 방식입니다.
그러므로 메이커에서 먼저 디버그 플레이를 실행한 뒤 VS Code Debugger를 연결해 사용해야 합니다. 디버그 연결 상태는 메이커의 Console에서 확인할 수 있습니다.

1. 디버깅할 월드를 메이커에서 엽니다.
2. 메이커의 디버깅 시작 버튼을 눌러 플레이합니다.
    ![\[object Object\]](https://mod-file.dn.nexoncdn.co.kr/bbs/1752493246769c727f80f4b834db49badc10dfa1d87ae.png)
3. VS Code에서 **Run and Debug**를 선택합니다.
    ![\[object Object\]](https://mod-file.dn.nexoncdn.co.kr/bbs/1752493465533de5e5b193b0a4b64918698ba153abef7.png)
4. 이전에 정의한 구성 정보의 이름을 선택한 뒤 실행 버튼을 누릅니다.
    ![\[object Object\]](https://mod-file.dn.nexoncdn.co.kr/bbs/1752491257751ada9a353952a4373a9a58c419aabec9f.png)
5. VS Code Debugger 연결이 완료되면 메이커의 Console에서 확인할 수 있습니다.
    ![\[object Object\]](https://mod-file.dn.nexoncdn.co.kr/bbs/175249314149585250a3fb9e242ae91545127d221d059.png)

# Run and Debug
## 중단점
중단점은 디버그 실행을 일시 중단할 지점입니다. 중단점은 여러 곳에 설정할 수 있고, 자유롭게 제거할 수도 있습니다. 중단점이 설정된 파일과 라인을 **BREAKPOINTS**에서 확인할 수 있습니다.

#### 중단점 지정, 해제
중단점을 지정하거나 해제하는 방법은 두 가지입니다.

1. 중단점을 지정할 라인을 선택하고 `F9` 키를 눌러 중단점을 추가하거나 해제합니다.
2. 코드 라인 영역을 눌러 중단점을 추가하거나 해제합니다.

![\[object Object\]](https://mod-file.dn.nexoncdn.co.kr/bbs/1752491279590042be7bd2233426b8df2b4ff76b99d7e.png)

#### VARIABLES
Scope에 따른 변수 정보를 확인할 수 있습니다.
![\[object Object\]](https://mod-file.dn.nexoncdn.co.kr/bbs/17524936153840d09d5ea89904d80ae2099bb38b014ec.png)

#### WATCH
직접 Expression을 작성하고 값을 확인할 수 있습니다.
![[object Object]](https://mod-file.dn.nexoncdn.co.kr/bbs/1752557620746239370f899b8440d963337db331828e9.png "watch")
#### CALL STACK
호출 스택을 확인할 수 있습니다.
![[object Object]](https://mod-file.dn.nexoncdn.co.kr/bbs/17525480355440cc7b232d4fa498881987f2cb470a7f5.png "callstack")
#### BREAKPOINTS
중단점이 설정된 파일의 경로와 라인을 확인할 수 있습니다. 목록에서 중단점을 선택하면 해당하는 중단점으로 이동합니다.
![\[object Object\]](https://mod-file.dn.nexoncdn.co.kr/bbs/1752491237685ab378074ca044f6e98ee50bc36320133.png)

#### ENVIRONMENT
현재 실행 공간을 확인할 수 있습니다.
![\[object Object\]](https://mod-file.dn.nexoncdn.co.kr/bbs/175249399198781822bddc5f947eca0d193fcc7905448.png)

#### 심볼에 할당된 값 확인
심볼에 할당된 값은 해당 심볼에 마우스를 올렸을 때 나타나는 툴팁으로 확인할 수 있습니다.

# 디버그 툴바
디버그를 시작하면, 툴바가 VS Code 상단에 나타납니다. 월드 디버그 실행 중 중단점에서 플레이가 멈추면 툴바의 실행 버튼들이 활성화됩니다. 디버그를 위해 코드를 계속 실행하고 싶다면 버튼을 눌러 코드를 계속 실행할 수 있습니다.
코드 실행을 통해 프로시저 단위로 코드의 상태나 흐름을 파악해 월드의 문제를 찾고 해결할 수 있습니다.

![\[object Object\]](https://mod-file.dn.nexoncdn.co.kr/bbs/17524912237671bfee3141dde43fb80b778e12dfef002.png)

| 번호 | 기능 | 설명 |
| --- | --- | --- |
| ![\[object Object\]](https://mod-file.dn.nexoncdn.co.kr/storage/numbers/NO_01.jpg) | 계속 실행 | 현재의 중단점에서 다음 중단점이 있는 코드 라인까지 실행합니다. |
| ![\[object Object\]](https://mod-file.dn.nexoncdn.co.kr/storage/numbers/NO_02.jpg) | 프로시저 단위 실행 | 코드를 한 줄씩 실행합니다. 포커싱된 라인에 함수가 있을 때, 그 함수를 실행한 것으로 간주하고 다음 줄로 넘어갑니다 |
| ![\[object Object\]](https://mod-file.dn.nexoncdn.co.kr/storage/numbers/NO_03.jpg) | 아래로 코드 실행 | 포커싱 된 라인에 함수가 있을 때 해당 함수 내부로 들어가 함수 내 코드를 한 줄씩 실행합니다. |
| ![\[object Object\]](https://mod-file.dn.nexoncdn.co.kr/storage/numbers/NO_04.jpg) | 위로 코드 실행 | 현재 실행 중인 함수 호출을 완료하고, 해당 함수를 호출한 위치로 이동합니다. |
| ![\[object Object\]](https://mod-file.dn.nexoncdn.co.kr/storage/numbers/NO_05.jpg) | 재시작 | 현재 디버그 실행을 중단하고, 디버그 시작점으로 돌아가 디버그를 다시 실행합니다. |
| ![\[object Object\]](https://mod-file.dn.nexoncdn.co.kr/storage/numbers/NO_05.jpg) | 중단 | 디버거 실행을 중단합니다. |