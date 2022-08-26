# 묵찌빠 게임

## 📖 목차
1. [소개](#-소개)
2. [고민한 점](#-고민한-점)
3. [타임라인](#-타임라인)
4. [실행 화면](#-실행-화면)
5. [트러블 슈팅](#-트러블-슈팅)
6. [참고 링크](#-참고-링크)

## 🌱 소개

`woong`과 `mene` 팀이 만든 콘솔 묵찌빠 게임입니다.
 
## 👀 고민한 점

`Optional 처리`  
`Control Flow` - `if` 🆚 `switch`  
`반복문` 🆚 `재귀함수`  
`Enumeration` - `CaseIterable`, `rawValue`  
`NameSpace`, `Magic Literal`  
`method 기능 분리 - 단일 책임`  

## ⏰ 타임라인

### Step 1

- **220823**

    - `Hand` 타입 enum 설정  ➡️ case `.paper` `.rock` `.paper`
    - 게임 전체를 실행해 줄 `gameStart` 함수 구현
    - 사용자 입력을 받아 `Hand` 타입으로 반환하는 `makeUserHand` 함수 구현
    - 랜덤 컴퓨터 핸드와 유저 핸드를 비교하는 함수 `checkResult` 구현
    - `checkResult` 함수에서 유저 입력이 0일 경우, 게임 종료 로직 추가
    - `Hand` Enum에서 사용하지 않는 불필요한 Int 타입 rawValue 삭제
    - **STEP 1 PR 작성** ➡️ [PR 보러가기](https://github.com/yagom-academy/ios-rock-paper-scissors/pull/160)
---
- **STEP 1 Refactoring**
    - `RockPaperScissors.swift` 파일에서 `Hand` enum을 파일로 분리
    - 전역변수를 사용하지 않기 위해 `gameStopCheck` 변수와 `randomComputerHand`  상수 함수 내부로 이동
    - 가위바위보 결과값을 연결해주는 `Result` enum 파일 생성 ➡️ case `.win` `.lose` `.draw` 
    - Result enum rawValue String 타입으로 설정하고`compareHand` 함수에서 결과 출력 시 rawValue 사용하도록 리팩토링
    - `compareHand` 함수에서 반환된 Result타입의 rawValue를 출력하는 `displayResult`함수 분리 (메서드별로 한가지 기능만 하도록 의도함.)
    - `makeUserHand` 함수 리팩토링
        - 원시값을 사용해서 매칭되는 열거형을 호출하여 사용 - [enum init 공식문서](https://developer.apple.com/documentation/swift/rawrepresentable/init(rawvalue:))
        ```swift=
        guard let inputNumberString = readLine(),
          let inputNumber = Int(inputNumberString),
          let result = Hand(rawValue: inputNumber) else {
        
        print(GameMessages.inputWrong)
        return startGame()
        }
        ```
    - 'startGame' 함수에서 재귀함수 이용하여 반복구현하도록 리팩토링
        - `while`구문을 통해 게임을 반복시키지 않고, `startGame()` 함수 내부에서 `startGame()`을 호출하는 재귀함수 활용

- **220824**

    - `compareHand` 함수 파라미터 수정 및 `Tuple`로 승패 처리하도록 리팩토링
        - `argument lable` 사용
            ```swift
            func compareHand(with computer: Hand, and user: Hand) -> Result {
            ```
    - `makeUserHand` 함수 삭제하고 `startGame` 함수 내부에서 그 역할을 동시에 수행하도록 수정
    - `compareHand` 함수 `.draw` 조건 `default` 에서 처리하도록 변경 및 `gameStart`함수명 `startGame`으로 변경

### Step 2

- **220824**
    - `MukJjiPpa` Type enum으로 구현 ➡️ case `.none` `.muk` `.jji` `.ppa`
    - `MukJjiPpaResult` Type enum으로 구현 ➡️ case `.win` `.lose` `.draw`
    - 묵찌빠 게임을 진행하는 `playMukJjiPpaGame` 함수 구현
    - 사용자와 컴퓨터의 묵찌빠를 비교하여 MukJjiPpaResult를 반환하는 `compareMukJjiPpa` 함수 구현
    - 비교된 함수를 받아서 게임을 실행하는 `playMukJjiPpaGame`함수 부분 구현
    - `startGame` 함수의 게임 재시작 조건 수정
    - MukJjiPpaResult enum rawValue 수정, 코딩 컨벤션 통일
    - **STEP 2 PR 작성** ➡️ [PR 보러가기](https://github.com/yagom-academy/ios-rock-paper-scissors/pull/168)
    
- **220825**
    - **STEP 2 Refactoring**
        - `while true` loop 사용하지 않기 위해, 사용자턴을 알려주는 `Player` enum 생성
        - 매직리터럴 제거를 위해 `GameMessages` enum 생성하고 String 이동
        - 삼항연산자를 사용하여 누구의 턴인지 출력하는 if문 삭제, `playMukJjiPpaGame` 함수에서 turn 매개변수 생성 및 Player enum으로 턴 주고받을 수 있게 변경
        - `playMukJjiPpaGame` 함수에서 묵찌빠 비교해서 결과를 출력하는 부분을`displayMukJjiPpaResult`함수로 분리
        - 새로 생성된 enum 파일의 주석 `(Created by woong, mene)`로 변경

## 💻 실행 화면

| 가위바위보 오류처리<br>draw & win| 가위바위보 lose<br>묵찌빠 오류처리 & 종료 | 가위바위보 win<br>묵찌빠 lose & draw(종료)
|:--:|:--:|:--:|
|<img src="https://i.imgur.com/zp4Icb8.gif" width="300">|<img src="https://i.imgur.com/Hk3668o.gif" width="300">|<img src="https://i.imgur.com/Rlo7MJ3.gif" width="300">|

## ❓ 트러블 슈팅

- **함께 작업하는 원격 Repository pull 했을 때 `main` 브랜치만 노출되는 문제**
    - 처음에는 Collaborator에 woong을 등록하지 않아서 생기는 문제인가 했는데 이후에도 step2 진행하면서 새로 생성한 step2 브랜치가 노출되지 않는 문제가 있었습니다.
    `git branch`명령으로 확인해보아도 main만 노출되어 `git branch -a`명령으로 확인해보니 step2 브랜치 네임이 빨간색으로 표시되어 `git checkout step2`로 브랜치 변경하고 계속해서 진행할 수 있었습니다.

- **readLine()으로 받아온 옵셔널 값 처리 guard에서 0,1,2,3의 Int값을 받아 사용 ➡️ 함수 내부에서 Hand 타입으로 바로 변경**

    ```swift
    guard let inputNumberString = readLine(), let inputNumber = Int(inputNumberString),
              inputNumber >= 0, inputNumber < 4 else {
            print("잘못된 입력입니다. 다시 시도해주세요.")
            continue
            }
    ```
    처음 작성했던 코드는 readLine에서 받아온 옵셔널 스트링값을 Int 타입으로 변환해서 0...3 사이인지 확인하고 Int로 반환된 값을 다시 사용하는 방식이었는데(이때는 Hand 타입에도 `.scissor` `.rock` `.paper`만 존재)
    
    ```swift
    guard let inputNumberString = readLine(),
  let inputNumber = Int(inputNumberString),
  let result = Hand(rawValue: inputNumber) else {
    ```
    rawValue를 사용해서 해당하는 Hand 타입을 매칭하는 방식으로 리팩토링하였습니다.(Hand 타입에도 0에 대응하는 `.none` 케이스 추가)    

- **Hand 타입에서 `.none` 케이스 처리**

    그에 따라 guard문에서 0,1,2,3을 받아서 처리할 때에는 임의의 컴퓨터 핸드를 만들어주는 `randomComputerHand` 상수도

    ```swift
    let randomComputerHand: Hand = Hand.allCases.randomElement().unsafelyUnwrapped
    ```
    enum의 CaseIterable 프로토콜을 이용해서 `allCases.randomElement()`로 정의해서 사용하다가 Hand 타입으로 직접 변환하게 되면서 0이 입력되면 가위/바위/보 가 아닌 `none`이 생성될 수 있는 문제가 있어서

    ```swift
    let randomComputerHand: Hand = Hand(rawValue: Int.random(in: 1...3)).unsafelyUnwrapped
    ```
    `.none`을 제외하고 1...3 사이의 Hand만 랜덤하게 생성되도록 수정하였습니다.

- **종료조건 턴 확인 ➡️ Bool 타입의 `isuserTurn` 전역변수 사용 대신 `Player` enum 활용**

    기존에는 승패 여부를 `isUserTurn`이라는 Bool 타입의 전역변수로 정의하고 관리하였습니다. 그것을 활용하여 승패를 조건으로 while문이 작동하도록 하였습니다.
    ```swift
    func gameStart() {
        while isUserTurn {
    ```
    
    변경 후, `playMukJjiPpaGame` 함수의 파라미터로 누구의 턴인지 Player 타입으로 받아서 턴과 결과를 튜플로 비교하여 승패 여부를 판단하도록 수정하였습니다.
    ```swift
    func playMukJjiPpaGame(turn: Player) {
        switch (turn, mukJjiPpaResult)
    }
    ```
- **매직 리터럴과 NameSpace 타입**

    - `MagicLiteral`: 따로 변수나 상수에 할당하지 않은 리터럴을 아무 설명 없이 코드에 집어 넣는 것.
    - `MagicNumber`: 나무위키에서는 이를 설명없이 무작정 등장하는 숫자라고 표현한다.
        - 이번 프로젝트에서는 매직넘버와 리터럴 사용을 하지 않기 위해 노력해 보았습니다.
    - `NameSpace`에 대해 새롭게 알게 되었습니다. 네임 스페이스는 관련있는 값들을 모아놓은 공간인데 struct 타입으로 설정하거나 enum으로 설정해 줄 수 있습니다.
    struct 타입으로 설정 시 협업을 하는 개발자들이 실수로 인스턴스를 생성할 수 있는 가능성이 있기 때문에 `private init()`을 통해 인스턴스화 할 수 없도록 하거나 enum으로 생성해야 합니다.
        - 게임 내에서 사용되는 모든 메세지들을 `GameMessages`라는 이름의 enum 타입의 NameSpace를 생성하여 호출하도록 리팩토링 하였습니다.
    
    ```swift
    enum GameMessages {
    static let mukJjiPpaGameMemu: String = "턴] 묵(1), 찌(2), 빠(3)! <종료 : 0> :"
    static let inputWrong: String = "잘못된 입력입니다. 다시 시도해주세요."
    static let endGame: String = "게임종료"
    static let victory: String = "의 승리!"
    static let rockPaperScissorGameMenu: String = "가위(1), 바위(2), 보(3)! <종료 : 0> :"
    }
    ```

## 🔗 참고 링크

[Swift API Design Guidelines - Naming](https://swift.org/documentation/api-design-guidelines/)  
[Swift Language Guide - Control Flow](https://docs.swift.org/swift-book/LanguageGuide/ControlFlow.html)  
[Swift Language Guide - Functions](https://docs.swift.org/swift-book/LanguageGuide/Functions.html)  
[Swift Language Guide - Enumerations](https://docs.swift.org/swift-book/LanguageGuide/Enumerations.html)  
[Swift Language Guide - Properties](https://docs.swift.org/swift-book/LanguageGuide/Properties.html)  
[Swift Language Guide - Methods](https://docs.swift.org/swift-book/LanguageGuide/Methods.html)  
[Swift에서의 namespace](https://zeddios.tistory.com/353)  
[매직넘버-나무위키](https://namu.wiki/w/%EB%A7%A4%EC%A7%81%EB%84%98%EB%B2%84#toc)  

---

[🔝 맨 위로 이동하기](#묵찌빠-게임)
