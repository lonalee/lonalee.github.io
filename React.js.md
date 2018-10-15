---
title: React
subtitle: what a new world!
layout: post
icon: fa-book
order: 3
---


## Tic-Tac-Toe game

- Board Component -> Square Component (Parent => Child)
  - Iteration Value를 Parent -> Child (data 공유는 props-properties)
  - **여러 자식 컴퍼넌트와 부모 컴퍼넌트 또는 자식 요소끼리** 정보 공유를 위해서는 부모 컴퍼넌트에 state를 선언해주어야 한다.
  - Onclick 이벤트 처리

- Square Component의 상태 관리
  - 상태 관리를 위해서 Component에서 constructor(props) {} 형태로 정의
  - this.state = { value: null }
  - click 시 state의 변화
    - 각 square의 state는 (X로 변한 상태) 부모 컴퍼넌트(Board)에서 관리.

- lifting state up (상태를 부모 컴퍼넌트에게 전달?)
  - 9 개의 null로 채워진 배열 생성 (in the Board component)
  - 초반부에 value prop을 Board에서 자식 컴퍼넌트로 전달했다. 이를 통해서 각 네모 상자에서 0~8까지 표시할 수 있게 되었다.
  - 각 숫자를 Square 컴퍼넌트의 상태에 따라(클릭 여부에 따라) X 표시로 교체했다.
    이로 인해 Square 컴퍼넌트는 현재 Board로부터 전달되는 value prop을 무시한다.
- 메커니즘을 재 전달하는 prop 값을 사용한다. 그리고 Board는 각 Square에 현재 값('X', 'O', or null)을 알려주기 위해서 수정이 필요하다.
- Board의 constructor에 배열을 정의하였고, Board의 renderSquare 메소드가 그 배열을 참조할 수 있도록 수정

  ```
  renderSquare(i) {
    return <Square value={this.state.squares[i]} />;
  }
  ```

- 각 Square는 'X', 'O', or null 값을 가질 수 있는 prop 값을 받을 수 있다.

다음으로, Square가 클릭될 때 발생하는 일을 araboza.
Board는 어떤 square가 null 아닌 값으로 채워져 있는지 알 수 있다.
이제 Square가 Board의 상태를 업데이트하기 위한 방법을 마련해야 한다.
상태란 그것을 정의하는 컴퍼넌트에게는 private한 것으로 간주되기 때문에 Square로부터 직접적으로 Board의 상태를 업데이트할 수는 없다.

Now we’re passing down two props from Board to Square: value and onClick.
이제 Board -> Square로 2 종류(value, onClick)의 props를 전달할 수 있다.

onClick prop은 함수이다. 이 함수는 클릭 발생 시 Square에 의해서 호출되는 함수이다. 그리고 Square 컴퍼넌트에 대한 다음과 같은 수정사항을 적용한다.

```
this.state.value -> this.props.value  (in Square’s render method)
this.setState() -> this.props.onClick() (in Square’s render method)
constructor 삭제 (더 이상 game 컴퍼넌트의 상태를 추적하지 않기 때문)
```

Square가 클릭되면 Board에 의해 onClick 함수가 호출되어 진다.
세부 단계는 다음과 같다.

1. button 컴퍼넌트에 있는 onClick prop이 React에 클릭 이벤트 리스너를 준비하라고 전달한다.
2. 버튼이 클릭되면, React는 Square의 render() 메소드에 정의된 onClick 이벤트 핸들러를 호출한다.
3. 이벤트 핸들러는 this.props.onClick() 함수를 호출한다. Square의 onClick prop은 Board에 의해 구체화되었다.
4. Board에서 Square로 onClick={ () => this.handleClick(i) } 이벤트 핸들러를 전달하기 때문에, Square는 클릭 이벤트 발생 시 this.handleClick(i)을 호출한다.
5. handleClick() 메소드는 아직 정의하지 않았기 때문에 코드는 실행되지는 않는다.

handleClick 메소드 정의 후,

Square를 클릭하여 X 마크를 할 수 있게 되었다. 그러나 현재는 상태를 개별 Square 컴퍼넌트가 아닌 **Board 컴퍼넌트 안**에 저장하는 것이다.
Board 컴퍼넌트의 상태가 변화할 때, Square 컴퍼넌트가 자동적으로 다시 rendering을 수행한다.
Board 컴퍼넌트 내의 모든 square (상자들)의 상태를 유지해야 승자를 결정할 수 있을 것이다.

Square 컴퍼넌트는 더 이상 상태를 유지하지 않기 때문에 그것들은 Board 컴퍼넌트로부터 값을 받고 그것들이 클릭 될 때 그 사실을 Board에게 통보한다. Square 컴퍼넌트는 Board의 통제를 받는 컴퍼넌트이다.

handleClick 메소드에서 slice라는 배열 메소드를 사용해서 squares array의 복사본을 생성했다. 원본 배열에 대한 직접적인 수정을 피하기 위함인데 **복사본을 생성한 보다 자세한 이유는 다음 섹션에서!!!**

### 변경불가성이 중요한 이유?

바로 이전 섹션에서 slice 메소드를 사용한 이유가 언급되었다. 이것을 설명하기 위해서 변경불가성에 대해서 논의하고 이것이 중요한 이유를 알아본다.
There are generally two approaches to changing data. The first approach is to mutate the data by directly changing the data’s values. The second approach is to replace the data with a new copy which has the desired changes.

일반적으로 데이터를 변경하는데에는 2가지 방법이 있다. 첫번째 방법은 데이터의 값을 직접 수정하는 것이고, 두번째는 새로운 값을 갖는 복사본으로 데이터를 바꾸는 방법이다.

The end result is the same but by not mutating (or changing the underlying data) directly, we gain several benefits described below.

두 가지 방법은 결과는 같을 것이다. 그러나 새로운 복사본으로 대체하는 방법이 더욱 유용하다.

1. 기능의 단순화
    1. 불변성은 복잡한 특성들의 실행을 쉽게 만든다. 이 튜토리얼의 후반부에서 "time travel"이라는 feature를 생성할 것이다. 그것은 tic-tac-toe 게임의 진행 history 확인, 이전 순서로 복귀를 가능하게 만드는 기능이다. 이 기능은 일반적으로 어플리케이션에서 사용되는 기능이다. 직접적인 데이터 수정을 지양해야만 게임의 history를 확인할 수 있을 것이며 재사용할 수도 있을 것이다.

2. 변화 감지
    1. 변경 가능한 대상에 대한 변화 감지는 어려움이 따를 수 있다. 왜냐하면 그것들은 직접적으로 수정이 가능한 것들이기 때문이다. 이러한 변화 감지는 변경 가능 대상이 그것의 이전 상태와 비교를 통해서 가능한 것인데, 이것은 객체의 전체적인 구조를 대상으로도 비교를 하려고 한다.
    2. 불변 객체에 대한 변화 감지는 상당히 수월하다. 만약 불변 객체(어떤 것으로부터 참조되는)가 이전의 상태와 다르다면 그 객체는 변경된 것이다.


3. re-rendering 시점 결정
    1. 불변성의 주된 이점은 순수한 컴퍼넌트를 만들 수 있게 해준다는 점이다. 불변성질의 데이터는 변화의 발생 여부를 쉽게 판단할 수 있다. 그 변화는 re-rendering 타이밍을 결정하는데 중요한 역할을 한다.
    2. shouldComponentUpdate() 메소드에 대해 학습하고 순수 컴퍼넌트를 작성하는 방법 또한 학습할 수 있다. (Optimizing Performance 참조)

### Functional Components

Square 컴퍼넌트를 함수형(functional) component로 바꾸어본다.
리액트에서는 함수형 컴퍼넌트는 컴퍼넌트를 작성하기 위한 보다 쉬운 방법이다. 그 컴퍼넌트들은 rendering을 위한 메소드만을 담고 있으며 그 자신들의 상태와 관련된 것들은 갖고 있지 않다.
React.Component를 확장하는 하나의 클래스를 정의하는 것 대신에, props를 인자로 받고 화면에 rendering될 내용을 return하는 함수를 작성할 수 있다.
함수형 컴퍼넌트는 클래스 형태로 작성하는 것 보다 간결하다. 많은 컴퍼넌트들이 이러한 형태로 표현될 수 있다. (class -> function)

> Square 클래스에 함수 삽입

### Taking Turns

Tic-Tac-Toe게임의 순서를 명확히 하기 위해서 X,O가 번갈아서 표시되어야 한다. 첫번째는 default로 X로 시작한다. Board 컴퍼넌트의 constructor에서 초기값을 지정할 수 있다.

>xIsNext: true

플레이어가 움직일 때마다, 다음 플레이어가 누군지 결정하기 위해 xIsNext 프로퍼티는 반대가 될 것이다. 그리고 game 컴퍼넌트의 상태는 저장될 것이다.
Board 컴퍼넌트의 handleClick 함수를 xIsNext의 값을 반전하기 위해서 수정해보자.

xIsNext는 boolean 값으로 X,O를 구분하기 위한 state 객체의 프로퍼티이다. 클릭 발생 시 handleClick 메소드가 호출되고 클릭된 square를 알 수 있다. 이 때 삼항연산으로 현재 xIsNext가 true이면 squaresV의 해당 index에 X를, 반대의 경우는 O를 할당한다. 할당 후에는 xIsNext가 반전되어야 할 것이다. 따라서 setState 메소드로 반전 시킨 xIsNext 프로퍼티 값을 재 할당한다.

> this. state는 무엇?
> 상태 관리에 필요한 객체 (state 자체는 property)
> 9개의 인덱스를 갖는 배열(O,X,null 관리), 순서를 보장하기 위한 boolean value를 담고 있다.

### 승자 판정

Now that we show which player’s turn is next, 순서가 유지되어 플레이할 수 있으므로 게임이 끝나는 순간을 결정할 수 있어야 한다. 즉, 승자를 판정하기 위해서 함수를 정의하도록 한다.

### 시간 이동 기능 추가

#### 자리 이동 history의 저장

squares 원본 배열을 조작했다면 시간 이동 기능을 추가하는 것을 매우 어려운 일이 될 것이다.

그러나, slice 메소드를 사용해서 매 움직임마다 squares의 복사본을 생성하고 squares는 변경불가능한 것으로 취급했다.
이로 인해서 이동 발생 후에도 지속적으로 이전의 움직임을 배열로 보존할 수 있고 이전의 차례로 이동하게 만들 수도 있다.

이전 버전의 squares 배열을 history라는 다른 배열에 저장하도록 한다. history 배열은 처음부터 끝까지의 모든 board states를 표현할 수 있다.

### Lifting State Up, Again

최상위의 Game 컴퍼넌트가 이전의 움직임들을 표시하도록 하자. 그러려면 history에 접근할 수 있어야 하므로 history state를 Game 컴퍼넌트에 위치 시킨다.

history state를 Game 컴퍼넌트에 위치시키면 squares state를 Board 컴퍼넌트(child)에서 제거해도 된다.
Square -> Board 컴퍼넌트로 상태 정보를 이동(lift up) 시킨 것 처럼 Board -> Game (child -> parent)으로 lift up 한다.
이렇게 하면 Game 컴퍼넌트가 Board의 데이터에 대한 완전한 제어권을 갖게 되고, **Game은 Board에게 이전 차례의 움직임(history array에 담겨 있는)을 표시하도록 지시할 수 있다.**

먼저, Game 컴퍼넌트의 초기값을 constructor에 정의한다.

그 다음으로 Board 컴퍼넌트가 Game 컴퍼넌트로부터 squares와 onClick props를 전달 받도록 만들어야 한다. **현재 상태는 Board 컴퍼넌트가 다수의 square들의 이벤트 처리를 하나의 핸들러로만 처리하기 때문에, 어느 square가 클릭됐는지 index number를 통해 알려줘야 한다**. Board 컴퍼넌트의 형태를 바꾸기 위해 아래 단계를 수행한다.

1. constructor 삭제 ( in Board )
2. this.state.squares[i] -> **this.props.squares[i]** (in Board’s renderSquare)
3. this.handleClick(i) -> **this.props.onClick(i)** (in Board’s renderSquare)

The Board component now looks like this:
<생략>

>현 game의 상황을 판단하고 결정하기 위한 최신의 history 진입점으로 사용하기 위해서 Game 컴퍼넌트의 render 함수를 수정한다.
<생략>

>Game 컴퍼넌트는 이제 game 진행상황을 렌더링 할 수 있으므로 Board 컴퍼넌트의 렌더링 메소드에서 이와 상응하는 코드는 필요 없을 것이다.
<생략>


마지막으로, HandleClick 메소드를 Board -> Game 컴퍼넌트로 이동하자.
HandleClick을 수정할 필요도 있다. Game 컴퍼넌트의 state가 다른 구조로 정의되었기 때문이다.
**Game의 HandleClick 메소드 내부에서 새로운 history 엔트리를 history(배열)로 한데 모은다(concat 메소드 사용).**

<생략>

이 시점에서 Board 컴퍼넌트는 renderSquare와 render 메소드만이 필요하다. 게임의 state와 handleClick 메소드는 Game 컴퍼넌트에 있으면 된다.

### 이전 움직임들(history) 보여주기

게임의 history를 기록하고 있으므로 리스트 형태로 화면에 표시할 수 있다.
**React의 요소(element)들은 JS의 일급객체이다.** 값으로 전달할 수도 있고 return으로 사용할 수도 있다. 여러 대상을 render하기 위해서 React에서는 **요소의 배열**을 사용할 수 있다.

map 메소드를 사용하여 움직임 history를 React 요소에 매핑할 수 있다. 이 요소들은 **화면의 버튼**을 표현하는 것이다. 그래서 이제 이전 움직임으로 ***jump*** 하게 만드는 버튼들을 하나의 리스트로서 표시할 수 있다.

Let’s map over the history in the Game’s render method:
<생략>

게임 history의 매 turn마다 button 요소를 갖는 li를 생성한다.
button 요소는 this.jumpTo 메소드를 호출하는 onClick 핸들러를 갖는다. 아직 이 메소드를 정의하지는 않았기 때문에, 현재는 다음과 같은 에러가 발생한다.

 >a warning in the developer tools console that says:
 >Each child in an array or iterator should have a unique “key” prop. Check the render method of “Game”.

### Key 획득

리스트를 render할 때, React는 rendered li요소에 대한 정보를 저장한다. 이 리스트를 업데이트할 때 React는 변경된 것이 무엇인지 알 필요가 있다.
```
Imagine transitioning from
  <li>Alexa: 7 tasks left</li>
  <li>Ben: 5 tasks left</li>
to

  <li>Ben: 9 tasks left</li>
  <li>Claudia: 8 tasks left</li>
  <li>Alexa: 5 tasks left</li>
```

위의 예시에서 React가 1번 list -> 2번 list로의 변화를 인식하기 위해서는 개별 li가 고유값 (key property)를 가져야 한다.

예시의 strings (alexa, ben, claudia)를 사용할 수도 있다. 만약 DB로부터 데이터를 가져오는 것이라면 각 string의 DB ID를 key값으로 사용할 수도 있다.

key value는 ref와 더불어 React에서 고유한 값으로 사용된다. 요소가 생성될 때 React는 key 프로퍼티를 추출하고 생성된 요소에 즉각적으로 저장한다. key 값이 props에 속하는 것 처럼 보일 수 있지만 **이것은 this.props.key와 같은 형태로 참조할 수 없다.** React는 자동적으로 어떤 컴퍼넌트를 업데이트할 지 결정하기 위해서 key 값을 사용한다. **컴퍼넌트가 그것의 key 값을 알 수는 없다.**

하나의 리스트가 re-render될 때, React는 각 li의 key를 수집하고 이전 버전의 li들에서 일치하는 key 값을 찾는다. 현재 리스트가 이전 버전에는 없는 key를 갖는다면 React는 새로운 컴퍼넌트를 생성한다. 현재 리스트에 없는 키 값이라면 React는 이전의 컴퍼넌트는 제거한다. 키 값이 일치한다면 컴퍼넌트는 이동된다. (previous -> current list) key 값은 React에게 각 컴퍼넌트의 정체에 관해 알려준다. 각 컴퍼넌트는 React로 하여금 rendering 간의 상태를 유지할 수 있도록 해준다. 어떤 컴퍼넌트에서 key 값이 바뀐다면 컴퍼넌트는 제거되었다가 새로운 상태로 다시 생성된다.

동적으로 리스트를 생성할 때는 적절한 key 값을 할당해야만 한다. 그렇지 않으면 key 값 할당을 위해서 데이터 구조를 다시 정의해야 할 수도 있다.
key 값이 할당되지 않으면 React는 warning을 표시하고 기본적으로 배열 index를 key로 사용한다.
배열 index를 key로 사용하면 li의 순서를 조정하거나 추가/삭제할 때 문제의 소지가 있다.
유일한 key 전달 (key={i})로 경고 메시지를 제거할 수 있지만 배열 index 사용과 같은 문제가 발생할 수도 있다.
key 값들은 전역에서 유일한 값일 필요는 없다. 컴퍼넌트 간에서 유일하면 된다.

### Implementing Time Travel

게임의 history에서 각 이전 움직들은 유일한 ID를 갖는다. 이들은 움직임에 따른 순번이다. 이들은 재정렬되지 않고, 삭제, 삽입되지 않기 때문에 key 값으로 사용되기 적합하다.
Game 컴퍼넌트의 render 메소드에서 <li key={move}> 형태로 key 값을 할당할 수 있고 React의 경고 메시지는 없어질 것이다.

jumpTo 메소드를 정의하기 전에 stepNumber를 추가한다. stepNumber는 Game 컴퍼넌트의 state에 존재해야 하며 우리가 보고 있는 단계?, 순서?를 가리킨다.

초기값(initial state) 0으로 stepNumber를 Game’s constructor에 추가한다.

Game 컴퍼넌트에 stpeNumber를 업데이트하기 위해 jumpTo 메소드를 정의한다.

stepNumber의 상태는 사용자의 움직임을 반영하는 것이다. 움직임이 발생하면 stepNumber가 누적?되는 방식으로 stepNumber가 변경되야 한다.

새로운 움직임이 발생한 후에도 같은 움직임을 보여주지 않는 것을 보장한다.

>this.state.history -> this.state.history.slice(0, this.state.stepNumber + 1)

위와 같이 수정하면 특정 시점으로 되돌아간 후에도 그 시점으로부터 새롭게 이동할 수 있다. 부정확하게 될 **미래 history?**는 지워버리자!

다음과 같이 작동하도록 Game 컴퍼넌트의 render 메소드를 수정한다.
(As-Is)항상 최종 움직임을 렌더링 -> (To-Be) stepNumber에 의한 현재 선택된 움직임을 렌더링

history에서 어떤 step을 클릭하면 board는 board가 step 이동이 발생한 후 어떻게 생겼었는지 보여주기 위해 즉시 업데이트 된다.

1. Display the location for each move in the format (col, row) in the move history list.
    1. 행과 열의 좌표를 history list에 표시하기
        x,y 축을 각각 프로퍼티로 하는 객체
        { col: 1, row: 1}
        { col: 0, row: 0}
        { col: 0, row: 0}
        클릭되는 스퀘어는 index넘버로 구별할 수 있다.
        index : 0 ~ 2까지는 3으로 나누면 몫이 1 미만
        index : 3 ~ 5까지는 3으로 나누면 몫이 1 이상
        index : 6 ~ 8까지는 3으로 나누면 몫이 2 이상


2. Bold the currently selected item in the move list.
    1. history에서 클릭된 버튼을 어떻게 식별하는가?
    2.
3. Rewrite Board to use two loops to make the squares instead of hardcoding them.
4. Add a toggle button that lets you sort the moves in either ascending or descending order.
5. When someone wins, highlight the three squares that caused the win.
6. When no one wins, display a message about the result being a draw.

- 할 일
  - view folder 생성
    - signUp.html
    - gallery.html?
  - routing
  - modeling
  - express의 index.js 파일의 설정 가져와야 함.
    - .env, mongoose 설치 등등
