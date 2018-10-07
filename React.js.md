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
  - **여러 자식 컴퍼넌트와 부모 컴퍼넌트 또는 자식 요소끼리** 정보 공유를 위해서는 부모 컴퍼넌트에 shared state를 선언해주어야 한다.
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

After these changes, we’re again able to click on the Squares to fill them.
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

Square 컴퍼넌트를 기능적(functional) component로 바꾸어본다.
리액트에서는 기능적 컴퍼넌트는 컴퍼넌트를 작성하기 위한 보다 쉬운 방법이다. 그 컴퍼넌트들은 rendering을 위한 메소드만을 담고 있으며 그 자신들의상태와 관련된 것들은 갖고 있지 않다.
React.Component를 확장하는 하나의 클래스를 정의하는 것 대신에, props를 인자로 받고 화면에 rendering될 내용을 return하는 함수를 작성할 수있다.
기능적, 함수형 컴퍼넌트는 클래스 형태로 작성하는 것 보다 간결하다. 많은 컴퍼넌트들이 이러한 형태로 표현될 수 있다.

> Square 클래스에 함수 삽입

### Taking Turns

Tic-Tac-Toe게임의 순서를 명확히 하기 위해서 X,O가 번갈아서 표시되어야 한다. 첫번째는 default로 X로 시작한다. Board 컴퍼넌트의 constructor에서 초기값을 지정할 수 있다.


- 할 일
  - view folder 생성
    - signUp.html
    - gallery.html?
  - routing
  - modeling
  - express의 index.js 파일의 설정 가져와야 함.
    - .env, mongoose 설치 등등
