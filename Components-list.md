---
title: Components
subtitle: Oh my gosh!
layout: page
icon: fa-book
order: 3
---

## Lesson-learned

### Sign up 페이지 MarkUp & Styling *09. 12.*

- tabindex로 focus 적용
  - 휴대전화 번호의 드롭다운 박스를 열고, 아무것도 선택하지 않고 화면의 다른 영역(드롭다운 박스 이외의)을 클릭할 때 드롭다운 박스를 없애기 위해서 blur event를 적용하고자 한다. 따라서 드롭다운 박스를 화면 표시할 때 focus를 줘야 하는데 div, ul 등의 요소는 tabindex로 인위적으로 focusing 가능하게 해야 focus() 메소드가 의도대로 작동한다.
- overflow : scroll
  - overflow 되는 영역에 대한 scroll을 가능하게 하는 속성
  - position absolute 끼리 레이어되는 순서? -> z-index
  - 반응형? (**to do**)

* *순발력* +1
  * 선택 박스를 display none하고자, event.currentTarget과 event.target이 다름을 이용하여 제거. 이것은 eventListening을 하고 있는 요소가 무엇인지에 따라서 적용 못 할수도 있겠다. currentTarget과 target이 같을 때 display none 처리했음 -> 이를 위해서 선택 박스 *별도의 컨테이너에 패딩(bottom)*을 약간 줘서 currentTarget과 target이 같아질 수 있도록 인위적으로 처리함.

### Sign Up 페이지의 Express(Node.js) 연동

  express// index.ejs 파일에서 스크립트 로드할 때, public으로 기본적으로 src 루트가 설정되어있는듯.
    <script type="text/javascript" src="./signup.js"></script>


- 사용자 접근 시 public폴더에 정적 파일들(html 등)을 렌더링하도록 구현 (express.static)
- 로그인 페이지의 사용자 입력 정보를 express를 이용하여 서버에 전송하고 응답값을 수신하였다.
- 헤로쿠에 deploy하고 local web 환경 까지 구축.
- 헤로쿠(**lonalee-web**) <-> mongoDB 연결 (현재는 local 테스트 진행 중)
- **express_lonalee-web**(MY NEW REPO) <-> heroku
- mongolab-fluffy-64010
- DB : lonalee2018
- DB로 POST하려면 MODEL 정의 : model명.js -> export (DB의 스키마를 생성; 데이터의 자료형을 정의한다)
- index.js에서 해당 모듈 import(require)
- 사용자 입력값을 객체화 하려면 input 하나하나에 이벤트를 걸어야 한다...(이러한 이유로 angular와 같은 framework에서는 form 관련 요소가 존재했던 것.)
  - vanila JS에서는 유사한 개념이 없는지

### Responsive web site

- breakpoint에서 적용될 스타일의 정의
- initial-scale=1.0의 의미, 예를 들어, 모바일에서는 페이지를 줌 아웃해서는 안된다

```
  @media only screen and (max-width: 1200px) {
  .hero-text-box {
    width: 100%;
    padding: 0 2%;
  }
}
```
화면의 크기가 1200px 미만으로 작아지면 해당 클래스에 스타일이 적용된다.
width는 브라우저의 크기 그대로를 적용 (100%), 화면이 작아짐에 따라서 요소의 크기가 작아지는 것을 확인할 수 있다.

```
.long-copy {
    width: 80%;
    margin-left: 10%;
  }
```
컨텐트 크기가 80%를 차지하므로 좌측에는 10%를 할당하면 자연스럽게 우측 10%로 컨텐트는 중앙에 배치되겠다.