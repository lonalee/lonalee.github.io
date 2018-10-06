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

- lifting state up
  - 9 개의 null로 채워진 배열 생성 (Board component)
  - 초반부에 value prop을 Board에서 자식 컴퍼넌트로 전달했다. 이를 통해서 각 네모 상자에서 0~8까지 표시할 수 있게 되었다.
  - 각 숫자를 Square 컴퍼넌트의 상태에 따라(클릭 여부에 따라) X 표시로 교체했다.
  - 이것이 Square 컴퍼넌트가 현재 Board로부터 전달된 value prop을 무시하는 이유이다. ?
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

Square가 클릭되면 Board에 의해 onClick 함수가 호출되어 진다.
세부 단계는 다음과 같다.

1. button 컴퍼넌트에 있는 onClick prop이 React에 클릭 이벤트 리스너를 준비하라고 전달한다.
2. 버튼이 클릭되면, React는 Square의 render() 메소드에 정의된 onClick 이벤트 핸들러를 호출한다.
3. 이벤트 핸들러는 this.props.onClick() 함수를 호출한다. Square의 onClick prop은 Board에 의해 구체화되었다.
4. Board에서 Square로 onClick={ () => this.handleClick(i) } 이벤트 핸들러를 전달하기 때문에, Square는 클릭 이벤트 발생 시 this.handleClick(i)을 호출한다.
5. handleClick() 메소드는 아직 정의하지 않았기 때문에 코드는 실행되지는 않는다.

- 할 일
  - view folder 생성
    - signUp.html
    - gallery.html?
  - routing
  - modeling
  - express의 index.js 파일의 설정 가져와야 함.
    - .env, mongoose 설치 등등
