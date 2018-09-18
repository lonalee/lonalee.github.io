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

- 사용자 접근 시 public폴더에 정적 파일들(html 등)을 렌더링하도록 구현 (express.static)
- 로그인 페이지의 사용자 입력 정보를 express를 이용하여 서버에 전송하고 응답값을 수신하였다.
- 헤로쿠에 deploy하고 local web 환경 까지 구축.
- 헤로쿠 <-> mongoDB만 하면 됨