VS Code: mLua Extension

# 학습 과정 소개
LocalWorkspace와 ExtendedScriptFormat을 사용하는 크리에이터는 메이플스토리 월드에서 제공하는 mLua VS Code Extension을 활용하여 스크립트를 효율적으로 작성할 수 있습니다.

##### 참고 가이드
[LocalWorkspace]
[ExtendedScriptFormat]
[mLua]
[공동 제작]

# mLua VS Code Extension
mLua Extension은 VS Code에서 mLua를 편리하게 사용할 수 있도록 지원하는 확장 프로그램입니다. 

> <span style="color: #585858">더 알아보기
> mLua Extension은 글로벌 변수 선언이 가능하며, MSW 메이커의 스크립트 에디터와 달리 경고가 표시되지 않습니다.</span>

> <span style="color: #585858">더 알아보기
> 디버깅 기능이 추후 추가될 예정입니다.</span>
# mLua VS Code Extension 사용
mLua Extension을 사용하기 위해서는 [VS Code](https://code.visualstudio.com/{"target":"_blank"})를 설치해야 합니다. VS Code는 소스 코드 편집기로 구문 강조 기능을 제공해 코드 작성의 편리성을 높여줍니다. 

1. [VS Code](https://code.visualstudio.com/{"target":"_blank"})를 설치합니다.
2. **Extensions**를 선택하고 mLua를 검색합니다.
![[object Object]](https://mod-file.dn.nexoncdn.co.kr/bbs/1739257075188099842c84703431fae9f1dfe782c5b3b.png{"width":"430px"} "1")
3. 설치가 완료되면 Extension은 자동으로 활성화됩니다. 
![[object Object]](https://mod-file.dn.nexoncdn.co.kr/bbs/17392575481988bb9dee8ad324ddea22bef398b4421c7.png{"width":"430px"} "2")
4. **VS Code - File - Open Folder**를 선택한 뒤, 로컬에 내려 받은 월드 폴더를 선택해 전체 파일을 불러옵니다.
![[object Object]](https://mod-file.dn.nexoncdn.co.kr/bbs/1739257564610278a1b7b1ff143d0a498a01e8a3c6134.png{"width":"430px"} "3")
5. 작성한 코드를 메이플스토리 월드의 메이커에 동기화해 출시할 수 있습니다. 동기화 방법은 [LocalWorkspace]을 참고합니다.

# mLua VS Code Extension 기능
## Hover
심볼에 마우스를 올리면, 해당 심볼의 정보가 나타납니다.
![hover](https://mod-file.dn.nexoncdn.co.kr/bbs/1739258236378407788513cc141c2af1b550be5154b6d.png{"width":"430px"} "hover")
## Inlay Hint
멤버의 오버라이딩 여부, Entity Id에 해당하는 Entity Name와 같이 코드 작성을 돕는 정보를 표시합니다.
![Inlay](https://mod-file.dn.nexoncdn.co.kr/bbs/1739258296092c63e04f5db6a443585eb781342cc51b3.png{"width":"630px"} "Inlay")

## Document Symbol
문서에 존재하는 심볼을 표시합니다.
![[object Object]](https://mod-file.dn.nexoncdn.co.kr/bbs/1739259663469d2a0a54c39c44d87942b78154ea70269.png{"width":"430px"} "documentsymbol")

##  Rename Symbol
참조된 심볼들의 이름을 한 번에 변경할 수 있습니다.

##  Signature Helper
함수 호출 식을 작성할 때 함수 시그니처를 표시하여 파라미터 작성에 도움을 줍니다.  오버로딩된 경우 방향키를 사용하여 원하는 시그니처를 찾아 함수 시그니처를 표시할 수 있습니다.
![signature helper](https://mod-file.dn.nexoncdn.co.kr/bbs/1739258358762a37066f5d9284e27a5d031f086aedb78.png{"width":"430px"} "signature helper")

## Go to Reference
선택한 심볼의 참조로 이동합니다.

## Find All Reference
선택한 심볼의 모든 참조를 찾습니다.

## Go to Definition
선택한 심볼이 정의된 위치로 이동합니다.

## Go to Type Definition
선택한 심볼의 타입이 정의된 위치로 이동합니다.
![gototype](https://mod-file.dn.nexoncdn.co.kr/bbs/1739259625673c71ae233e5f2446daee2749b11cfbbd4.png{"width":"430px"} "gototype")

## Completion
#### Code Completion
스크립트 작성 시 특정 스크립트에 사용 가능한 심볼을 추론하고, 자동 완성을 제공합니다. 인덱싱할 때도 해당 심볼로부터 접근 가능한 멤버를 추론하고, 자동 완성을 제공합니다.

![[object Object]](https://mod-file.dn.nexoncdn.co.kr/bbs/173925981885800cc64e8e22a4c6bab618d241f270495.png{"width":"430px"} "123")

#### Entity Id Completion
엔티티 경로를 이용해서 엔티티의 Id를 쉽게 찾을 수 있습니다. Completion에서 제공한 엔티티 후보 중 하나를 선택하면, Entity Id로 자동으로 치환됩니다. 엔티티 Id를 쉽게 식별할 수 있도록 인레이 힌트로 엔티티의 이름을 확인할 수 있습니다.
예를 들어 프로퍼티 타입이 BackgroundComponent인 경우, Completion은 Background가 있는 엔티티의 Id만 추려서 보여줍니다. 
![entityId](https://mod-file.dn.nexoncdn.co.kr/bbs/173926657423897107b00ec1c4dc7a66a7c5907914b2c.png{"width":"540px"} "entityId")

##  Code Snippet
사용자가 자주 사용하는 코드를 템플릿으로 제공합니다. Snippet을 선택하여 코드 조각을 자동으로 완성하고, Tab 키를 눌러 템플릿이 의도하는 다음 위치로 이동할 수 있습니다.

## Diagnostic
크리에이터가 작성한 코드를 진단해 문제가 될 수 있는 부분을 알려주는 기능입니다. 문제 해결의 중요도에 따라 분류되고, 진단 결과는 PROBLEMS에서 확인할 수 있습니다.
![Problems](https://mod-file.dn.nexoncdn.co.kr/bbs/17392600764315daec26eb5b148fe90c9134258b65d7d.png{"width":"430px"} "Problems")

## Commit Character Support
`.(dot)`, `,(comma)`, `:(colon)`을 입력하는 시점에 자동으로 제안된 항목을 선택해주는 기능입니다.
![CommitCharacterSupport](https://mod-file.dn.nexoncdn.co.kr/bbs/1739259976957319c89b93a604bba81366707a91b93f2.gif{"width":"430px"} "CommitCharacterSupport")

# Code Folding
`--region`, `--endregion`을 사용해 작성한 코드의 특정 구간을 접는 기능입니다. 코드를 접고 싶은 시작 구간에 `--region`을, 끝 구간에 `--endregion`을 입력합니다.

![CodeFolding](https://mod-file.dn.nexoncdn.co.kr/bbs/17483232121567e4c7bf533d5448d9a8a8b143410a237.png{"width":"520px"} "CodeFolding")

# Symbol Color Customization
**Inspect Editor Tokens and Scopes**의 **foreground scope**값을 변경해 특정 심볼의 색상을 변경할 수 있습니다.
로컬 워크스페이스를 통해 로컬로 **다운 받은 프로젝트 폴더 - .vscode폴더** 하위에 **settings.json**파일을 생성해야만 색상 변경 기능을 사용할 수 있습니다. **settings.json** 파일 생성 방법은 [LocalWorkspace]가이드를 참고합니다. 

1. VS Code에서 **Ctrl + Shift + P**를 입력합니다.
2. **Developer:Inspect Editor Tokens and Scopes**를 선택합니다.
3. 색상을 변경할 심볼을 마우스로 클릭해 **foreground scope** 값을 확인합니다.
4. 생성한 **settings.json** 파일에 아래와 같은 형식으로 작성합니다.

    ```lua
    {
        "editor.tokenColorCustomizations": {
            "textMateRules": [
                {
                    "scope": "entity.name.class",
                    "settings": {
                        "foreground": "#daed30", 
                        "fontStyle": "bold"
                    }
                },
                {
                    "scope": "keyword.control",
                    "settings": {
                        "foreground": "#daed35",
                        "fontStyle": "bold"
                    }
                }
            ]
        }
    }
    ```
    
4. **settings.json** 파일을 저장하고, 색상이 변경되었는지 확인합니다.

><span style="color: #7cafc2">**Tip.**
>.**vscode** 폴더 내부의 파일 이름이 반드시 **settings.json**이어야 합니다. 이름이 정확하지 않으면 기능이 적용되지 않습니다.</span>

# 단축키

| 단축키 | 액션 |설명 |
| --- | --- | --- |
| Ctrl + Space|  Completion  | Completion을 활성화합니다. | 
| Ctrl + Shift + Space | Signature Helper  |Signature Helper를 활성화 합니다.| 
| Shift + F12 | Go to Reference  | 선택한 심볼의 참조로 이동합니다. | 
| Shift + Alt + F12 | Find All Reference  |선택한 심볼의 모든 참조를 찾습니다. | 
| F2   |  Rename  |이름을 변경합니다.| 
| F12 |   Go to Definition  | Definition으로 이동합니다.|