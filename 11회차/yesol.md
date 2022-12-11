# 40장 이벤트

Created: December 10, 2022 5:13 PM

## **40.1 이벤트 드리븐 프로그래밍**

-   이벤트가 발생했을 때 호출될 함수를 **이벤트 핸들러**라 함
-   이벤트가 발생했을 때 브라우저에게 이벤트 핸들러의 호출을 위임하는 것을 **이벤트 핸들러 등록**이라 함

```jsx
<button>Click me!</button>;

const button = document.querySelector("button");
button.onclick = () => {
    alert("button click");
};
```

-   버튼 요소에 onclick 함수를 할당했으며 onclick과 같이 특정 이벤트에 대응하는 다양한 이벤트 핸들러 프로퍼티를 가지고 있음
-   이벤트와 그에 대응하는 함수를 통해 사용자와 애플리케이션은 상호작용을 할 수 있으며 이와 같이 **프로그램의 흐름을 이벤트 중심으로 제어하는 프로그래밍 방식을 이벤트 드리븐 프로그래밍**이라 함

## **40.2 이벤트 타입**

**40.2.1 마우스 이벤트**

| 이벤트 타입 | 이벤트 발생시점                                              |
| ----------- | ------------------------------------------------------------ |
| click       | 마우스 버튼 클릭 시                                          |
| dblclick    | 마우스 버튼을 더블 클릭 했을 시                              |
| mousedown   | 마우스 버튼을 눌렀을 때                                      |
| mouseup     | s누르고 있던 마우스 버튼을 놓았을 때                         |
| mousemove   | 마우스 커서를 움직였을 때                                    |
| mouseenter  | 마우스 커서를 html 요소 안으로 이동했을 때(버블링 되지 않음) |
| mouseover   | 마우스 커서를 html 요소 안으로 이동했을 때(버블링 됨)        |
| mouseleave  | 마우스 커서를 html 요소 밖으로 이동했을 때(버블링 되지 않음) |
| mouseout    | 마우스 커서를 html 요소 밖으로 이동했을 때(버블링 됨)        |

**40.2.2 키보드 이벤트**

| 이벤트 타입                                                                         | 이벤트 발생 시점                       |
| ----------------------------------------------------------------------------------- | -------------------------------------- |
| keydown                                                                             | 모든 키를 눌렀을 때 발생               |
| \* 문자, 숫자, 특수 문자 키를 눌렀을 때는 연속적으로 발생하지만 그 외는 한번만 발생 |
| keypress                                                                            | 문자 키를 눌렀을 때 연속으로 발생      |
| keyup                                                                               | 누르고 있던 키를 놓았을 때 한번만 발생 |

**40.2.3 포커스 이벤트**

| 이벤트 타입 | 이벤트 발생 시점               | 버블링 |
| ----------- | ------------------------------ | ------ |
| focus       | html 요소가 포커스를 받았을때  | X      |
| blur        | html 요소가 포커스를 잃었을때  | X      |
| focusin     | html 요소가 포커스를 받았을 때 | O      |
| focusout    | html 요소가 포커스를 잃었을 때 | O      |

**40.2.4 폼 이벤트**

| 이벤트 타입 | 이벤트 발생 시점                                         |
| ----------- | -------------------------------------------------------- |
| submit      | form 요소 내의 submit 버튼을 클릭했을 때                 |
| reset       | form 요소 내의 reset 버튼을 클릭했을 때(최근에는 사용 X) |

**40.2.5 값 변경 이벤트**

| 이벤트 타입      | 이벤트 발생 시점                                                                                                 |
| ---------------- | ---------------------------------------------------------------------------------------------------------------- |
| input            | input, select, textarea 요소의 값이 입력 됐을 때                                                                 |
| change           | input, select, textarea 요소의 값이 변경 됐을 때                                                                 |
| readystatechange | html 문서의 로드와 파싱 상태를 나타내는 readyState(loading, interactive, complate) 프로퍼티 값 이 변경 되었을 때 |

**40.2.6 DOM 뮤테이션 이벤트**

| 이벤트 타입      | 이벤트 발생 시점                                             |
| ---------------- | ------------------------------------------------------------ |
| DOMContentLoaded | html 문서의 로드와 파싱이 완료되어 DOM 생성이 완료 되었을 때 |

**40.2.7 뷰 이벤트**

| 이벤트 타입 | 이벤트 발생 시점                                         |
| ----------- | -------------------------------------------------------- |
| resize      | 브라우저 윈도우의 크기를 리사이즈할 때 연속적으로 발생함 |
| scroll      | 웹페이지 또는 html 요소를 스크롤 할때 발생 함            |

**40.2.8 리소스 이벤트**

| 이벤트 타입 | 이벤트 발생 시점                                                           |
| ----------- | -------------------------------------------------------------------------- |
| load        | DOMContentLoaded 이벤트가 발생한 이후, 모든 리소스의 로딩이 완료 되었을 때 |
| unload      | 리소스가 언로드 될 때                                                      |
| abort       | 리소스 로딩이 중단되었을 때                                                |
| error       | 리소스 로딩이 실패했을 때                                                  |

## **40.3 이벤트 핸들러 등록**

**40.3.1 이벤트 핸들러 어트리뷰트 방식**

-   이벤트 핸들러 어트리뷰트의 이름은 onclick과 같이 on 접두사와 이벤트의 종류를 나타내는 이벤트 타입으로 이뤄져 있으며 어트리뷰트 값으로 함수 호출문 등의 문을 할당하면 이벤트 핸들러가 등록 된다

```jsx
<button onclick = 'sayHi('Lee')'>click me!</button>

function sayHi(name) {
		console.log(`Hi ${name}`)
}
```

-   위 예제에서 이벤트 핸들러 어트리뷰트 값으로 함수 호출문을 할당했는데 이때, **이벤트 핸들러 어트리뷰트의 값은 사실 암묵적으로 생성될 이벤트 핸들러의 함수 몸체를 의미** 함
-   즉, onclick=”sayHi(’Lee’)” 어트리뷰트는 파싱되어 다음과 같은 함수를 암묵적으로 onclick이벤트 핸들러 프로퍼티에 할당 함

```jsx
function onclick(event) {
    sayHi("Lee");
}
```

![Untitled](40%E1%84%8C%E1%85%A1%E1%86%BC%20%E1%84%8B%E1%85%B5%E1%84%87%E1%85%A6%E1%86%AB%E1%84%90%E1%85%B3%2007d49c0b164c46a988cf575fb24b199a/Untitled.png)

-   이처럼 동작하는 이유는 이벤트 핸들러에 인수를 전달하기 위하서 인데 이벤트 핸들러 어트리뷰트 값으로 함수 참조를 할당해야 한다면 이벤트 핸들러에 인수를 전달하기 곤란함
-   이벤트 핸들러 어트리뷰트 방식은 오래된 코드에서 간혹 사용해서 알아둘 필요는 있지만 더는 사용하지 않는 것으 좋음

**40.3.2 이벤트 핸들러 프로퍼티 방식**

-   window객체, Document, HTMLElement 타입의 DOM노드 객체는 이벤트에 대응하는 이벤트 핸들러 프로퍼티를 가지고 있음
-   onclick과 같이 on접두사와 이벤트의 종류를 나타내는 이벤트 타입으로 이루져 있는데 함수를 바인딩하면 이벤트 핸들러가 등록 됨

```jsx
<button>Click ME!</button>;

const button = document.querySelector("button");

button.onclick = function () {
    console.log("button click");
};
```

-   이벤트 핸들러를 등록하기 위해서는 **이벤트를 발생시킬 객체인 이벤트 타깃**과 **이벤트 종류를 나타내는 문자열인 이벤트 타입**, 그리고 **이벤트 핸들러를 지정**해야 함

![Untitled](40%E1%84%8C%E1%85%A1%E1%86%BC%20%E1%84%8B%E1%85%B5%E1%84%87%E1%85%A6%E1%86%AB%E1%84%90%E1%85%B3%2007d49c0b164c46a988cf575fb24b199a/Untitled%201.png)

-   이벤트 핸들러는 대부분 이벤트를 발생시킬 이벤트 타깃에 바인딩하지만 반드시 바인딩 해야 하는 것은 아님
-   이벤트 핸들러 어트리뷰트 방식도 결국 DOM 노드 객체의 이벤트 핸들러 프로퍼티로 변환되므로 결과적으로 이벤트 핸들러 프로퍼티 방식과 동일하다고 할 수 있지만 프로퍼티 방식은 html과 자바스크립트가 뒤섞이는 문제를 해결할 수 있음
-   하지만 이벤트 핸들러 프로퍼티에 하나의 이벤트 핸들러만 바인딩 할 수 있다는 단점이 있음

**40.3.3 addEventListener 메서드 방식**

-   addEventListener 메서드를 사용하여 이벤트 핸들러를 등록 할 수 있음

![Untitled](40%E1%84%8C%E1%85%A1%E1%86%BC%20%E1%84%8B%E1%85%B5%E1%84%87%E1%85%A6%E1%86%AB%E1%84%90%E1%85%B3%2007d49c0b164c46a988cf575fb24b199a/Untitled%202.png)

-   첫 번째 매개변수에는 이벤트의 종류를 나타내는 문자열인 이벤트 타입을 전달 함, 이때 프로퍼티 방식과는 달리 on 접두사를 붙이지 않음
-   두 번째 매개변수에는 이벤트 핸들러를 전달 함
-   마지막 매개변수에는 이벤트를 개치할 이벤트 전파단계를 지정함, 생략하거나 false를 지정하면 버블링 단계에서 이벤트를 캐치하고 true를 지정하면 캡처링 단계에서 이벤트를 캐치 함

```jsx
<button>Click ME!</button>;

const button = document.querySelector("button");

// 이벤트 핸들러 프로퍼티 방식
// button.onclick = function(){
//   console.log('button click')
// }

// addEventListener 메서드 방식
button.addEventListener("click", function () {
    console.log("button click");
});
```

-   프로퍼티 방식은 프로퍼티에 이벤트 핸들러를 바인딩 하지만, addEventListener 메서드는 이벤트 핸들러를 인수로 전달함
-   만약, 동일한 html 요소에서 발생한 동일한 이벤트에 대해 프로퍼티 방식과 addEventListener 방식을 모두 사용하여 등록하면 어떻게 동작할까?

-   addEventListener 메서드 방식은 바인딩된 이벤트 핸들러에 아무런 영향을 주지 않아서 버튼 요소에서 클릭 이벤트가 발생하면 2개의 이벤트 핸들러가 모두 호출 됨
-   프로퍼티 방식은 하나 이상의 이벤트 핸들러를 등록할 수 없지만 addEventListener 메서드는 하나 이상의 이벤트 핸들러를 등록할 수 있음

## **40.4 이벤트 핸들러 제거**

-   핸들러를 제거 하려면 removeEventListener 메서드를 사용해야 하는데 addEventListener 메서드에 전달한 인수와 removeEventListener 메서드에 전달한 인수가 일치하지 않으면 이벤트 핸들러가 제거 되지 않음

```jsx
<button>Click ME!</button>;

const button = document.querySelector("button");

const handleClick = () => console.log("button click");

// 이벤트 핸들러 등록
button.addEventListener("click", handleClick);

//이벤트 핸들러 제거
// addEventListener 메서드에 전달한 인수와 removeEventListener 메서드에
// 전달한 인수가 일치하지 않으면 이벤트 핸들러가 제거되지 않음
button.removeEventListener("click", handleClick, true); // 실패
button.removeEventListener("click", handleClick); // 성공
```

-   removeEventListener 메서드에 인수로 전달한 이벤트 핸들러는 addEventListener 메서드에 인수로 전달한 등록 이벤트 핸들러와 동일한 함수여야 하는데 따라서 무명 함수를 이벤트 핸들러로 등록한 경우 제거 할 수 없음 ⇒ 이벤트 핸들러를 제거하려면 참조를 변수나 자료구조에 저장하고 있어야 함

```jsx
// 이벤트 핸들러 등록
// 등록한 이벤트 핸들러를 참조할 수 없으므로 제거 할 수 없음
button.addEventListener("click", () => console.log("button click"));
```

-   기명 이벤트 핸들러 내부에서 removeEventListener 메서드를 호출하여 이벤트 핸들러 제거는 가능 단, 이벤트 핸들러는 단 한번만 호출 됨

```jsx
// 기명 함수를 이벤트 핸들러로 등록
button.addEventListener("click", function foo() {
    console.log("button click");

    // 이벤트 핸들러를 제거함, 따라서 이벤트 핸들러는 단 한 번만 호출 됨
    button.removeEventListener("click", foo);
});
```

-   기명 함수를 이벤트 핸들러로 등록할 수 없다면, 함수 자신을 가리키는 arguments.callee를 사용할 수 있음
-   단, 코드 최적화를 방해하므로 strict mode에서는 사용이 금지되며, 따라서 이벤트 핸들러의 참조를 변수나 자료구조에 저장하여 제거하는 것이 좋음

```jsx
// 무명 함수를 이벤트 핸들러로 등록
button.addEventListener("click", function () {
    console.log("button click");

    // 이벤트 핸들러를 제거함, 따라서 이벤트 핸들러는 단 한 번만 호출 됨
    // arguments.callee는 호출된 함수, 자기 자신을 가리 킴
    button.removeEventListener("click", arguments.callee);
});
```

## **40.5 이벤트 객체**

-   이벤트가 발생하면 이벤트에 관련한 다양한 정보를 담고 있는 **이벤트 객체가 동적으로 생성**되는데 이는 **이벤트 핸들러의 첫 번째 인수로 전달** 된다

```jsx
<p>클릭하면, 좌표가 표시 됩니다</p>
<em class="message"></em>

const msg = document.querySelector('.message')

// 클릭 이벤트에 의해 생성된 이벤트 객체는 이벤트 핸들러의 첫 번째 인수로 전달 됨
function showCoords(e) {
  msg.textContent = `clientX: ${e.clientX}, clientY: ${e.clientY}`
}

document.onclick = showCoords
```

-   클릭 이벤트에 의해 생성된 이벤트 객체는 이벤트 핸들러의 첫 번째 인수로 전달되어 매개변수 e에 암묵적으로 할당 됨
-   이는 브라우저가 이벤트 핸들러를 호출할 때 이벤트 객체를 인수로 전달하기 때문인데 이벤트 객체를 전달받으려면 이벤트 핸들러를 정의할 때 이벤트 객체를 전달받은 매개변수를 명시적으로 선언해야 함

**40.5.1 이벤트 객체의 상속 구조**

-   이벤트가 발생하면 타입에 따라 다양한 타입의 이벤트 객체가 생상되며 다음과 같은 상속 구조를 갖음

![Untitled](40%E1%84%8C%E1%85%A1%E1%86%BC%20%E1%84%8B%E1%85%B5%E1%84%87%E1%85%A6%E1%86%AB%E1%84%90%E1%85%B3%2007d49c0b164c46a988cf575fb24b199a/Untitled%203.png)

-   Event, UIEvent, MouseEvent 등 모두는 생성자 함수로 다음과 같이 생성자 함수를 호출하여 이벤트 객체를 생성할 수 있다

```jsx
// Event 생성자 함수를 호출하여 foo 이벤트 타입의 객체를 생성함
let e = new Event("foo");

console.log(e); // Event {isTrusted: false, type: 'foo', target: null, currentTarget: null, eventPhase: 0, …}
console.log(e.type); // foo
console.log(e instanceof Event); // true
console.log(e instanceof Object); // true

// FocusEvent 생성자 함수를 호출하여 focus 이벤트 타입의 객체를 생성 함
e = new FocusEvent("focus");
console.log(e); // FocusEvent {isTrusted: false, relatedTarget: null, view: null, detail: 0, sourceCapabilities: null, …}

// MouseEvent 생성자 함수를 호출하여 click 이벤트 타입의 객체를 생성한다
e = new MouseEvent("click");
console.log(e); // MouseEvent {isTrusted: false, screenX: 0, screenY: 0, clientX: 0, clientY: 0, …}

// KeyboardEvent 생성자 함수를 호출하여 keyup 이벤트 타입의 객체를 생성
e = new KeyboardEvent("keyup");
console.log(e); // KeyboardEvent {isTrusted: false, key: '', code: '', location: 0, ctrlKey: false, …}

// InputEvent 생성자 함수를 호출하여 change 이벤트 타입의 객체를 생성
e = new InputEvent("change");
console.log(e); // InputEvent {isTrusted: false, data: null, isComposing: false, inputType: '', dataTransfer: null, …}
```

-   이벤트가 발생하면 암묵적으로 생성되는 이벤트 객체도 생성자 함수에 의해 생성 됨
-   생성된 이벤트 객체는 생성자 함수와 더불어 생성되는 프로토타입으로 구성된 프로토타입 체인의 일원이 됨
-   이벤트 객체 중 일부는 사용자의 행위에 의해 생성된 것이고 일부는 자바스크립트 코드에 의해 인위적으로 생성 된 것임
-   Event 인터페이스는 DOM 내에서 발생한 이벤트에 의해 생성되는 이벤트 객체를 나타내며 모든 이벤트 객체의 공통 프로퍼티가 정의되어 있고 FocusEvent, MouseEvent, KeyBoardEvent, wheelEvent 같은 하위 인터페이스에는 이벤트 타입에 따라 고유한 프로퍼티가 정의되어 있다
-   다음 예제와 같이 이벤트 객체의 프로퍼티는 발생한 이벤트 타입에 따라 달라 짐

```jsx
<input type="text">
<input type="checkbox">
<button>Click Me</button>

const input = document.querySelector('input[type=text]')
const checkbox = document.querySelector('input[type=checkbox]')
const button = document.querySelector('button')

// load 이벤트가 발생하면 Event타입의 이벤트 객체가 생성
window.onload = console.log;

//change 이벤트가 발생하면 Event타입의 이벤트 객체가 생성
checkbox.onchange = console.log;

//focus 이벤트가 발생하면 FocusEvent 타입의 이벤트 객체가 생성
input.onfocus = console.log;

//input 이벤트가 발생하면 InputEvent 타입의 이벤트 객체가 생성
input.oninput = console.log;

//keyup 이벤트가 발생하면 KeyBoardEvent 타입의 이벤트 객체가 생성
input.onkeyup = console.log;

//click 이벤트가 발생하면 MouseEvent 타입의 이벤트 객체가 생성
input.onclick = console.log;
```

![Untitled](40%E1%84%8C%E1%85%A1%E1%86%BC%20%E1%84%8B%E1%85%B5%E1%84%87%E1%85%A6%E1%86%AB%E1%84%90%E1%85%B3%2007d49c0b164c46a988cf575fb24b199a/Untitled%204.png)

**40.5.2 이벤트 객체의 공통 프로퍼티**

-   Event 인터페이스, Event.prorotype에 정의되어 있는 이벤트 관련 프로퍼티는 UIEvent, CustomEvent, MouseEvent 등 모든 파생 이벤트 객체에 상송 됨 ⇒ 즉, Event 인터페이스의 이벤트 관련 프로터피는 모든 이벤트 객체가 상속받는 공통 프로퍼티 임

| 공통 프로퍼티                                                  | 설명                              | 타입    |
| -------------------------------------------------------------- | --------------------------------- | ------- |
| type                                                           | 이벤트 타입                       | string  |
| target                                                         | 이벤트를 발생시킨 DOM요소         | DOM요소 |
| currentTarget                                                  | 이벤트 핸들러가 바인딩된 DOM 요소 | DOM요소 |
| eventPhase                                                     | 이벤트 전파 단계                  |
| 0: 이벤트없음 / 1: 캡쳐링 단계 / 2: 타킷 단계 / 3: 버블링 단계 | number                            |
| bubbles                                                        | 이벤트 버블링으로 전파하는지 여부 |

다음 이벤트는 bubbles:false로 버블링하지 않음

-   포커스 이벤트 focus/blur
-   리소스 이벤트 load/unload/abort/error
-   마우스 이벤트 mouseenter/mouseleave | boolean |
    | cancelable | preventDefault 메서드를 호출하여 이벤트의 기본 동작을 취소할 수 있는지 여부
    다음 이벤트는 cancelable:false로 취소할 수 없음
-   포커스 이벤트 focus/blur
-   리소스 이벤트 load/unload/abort/error
-   마우스 이벤트 dbclick/mouseenter/mouseleave | boolean |
    | defaultPrevented | preventDefault 메서드를 호출하여 이벤트를 취소했는지 여부 | boolean |
    | inTrusted | 사용자의 행위에 발생한 이벤트인지 여부
    ex) click 메서드 또는 daspatchEvent 메서드를 통해 인위적으로 발생시킨 경우 isTrusted는 false임 | boolean |
    | timeStamp | 이벤트가 발생한 시각(1970/01/01:00:00:00)부터 경과한 밀리초 | number |

**40.5.3 마우스 정보 취득**

-   click, dbclick, mousedown, mouseup, mousemove, mouseenter, mouseleave 이벤트가 발생하면 생성되는 MouseEvent 타입의 객체는 고유의 프로퍼티를 갖는다
    -   마우스 포인트의 좌표 정보를 나타내는 프로퍼티: screenX/screenY, clientX/clientY, pageX/pageY, offsetX/offsetY
    -   버튼 정보를 나타내는 프로퍼티: altKey, ctrlKey, shiftKey, button

**40.5.4 키보드 정보 취득**

-   keydown, keyup, keypress 이벤트가 발생하면 생성되는 keyboardEvent 타입의 이벤트 객체는 altKey, ctrlKey, shiftKey, metaKey, key, keyCode 같은 고유의 프로프티를 갖음

## **40.6 이벤트 전파**

-   DOM 트리 상에 존재하는 요소 노드에서 발생한 이벤트는 DOM트리를 통해 전파되며 이를 이벤트 전파라 함

```jsx
<ul id="fruits">
    <li id="apple">Apple</li>
    <li id="banana">Banana</li>
    <li id="orange">Orange</li>
</ul>
```

-   ul 요소의 두 번째 자식인 li 요소를 클릭하면 클릭 이벤트가 발생하는데 이때 생성된 이벤트 객체는 이벤트를 발생시킨 DOM 요소의 이벤트 타깃을 중심으로 DOM 트리를 통해 전파 된다

![Untitled](40%E1%84%8C%E1%85%A1%E1%86%BC%20%E1%84%8B%E1%85%B5%E1%84%87%E1%85%A6%E1%86%AB%E1%84%90%E1%85%B3%2007d49c0b164c46a988cf575fb24b199a/Untitled%205.png)

-   캡처링 단계: 이벤트가 상위 요소에서 하위 요소 방향으로 전파
-   타깃 단계: 이벤트가 이벤트 타깃에 도달
-   버블링 단계: 이벤트가 하위 요소에서 상위 요소 방향으로 전파

-   ul요소에 이벤트 핸들링을 바인딩하고 ul요소의 하위 요소인 li요소를 클릭하여 이벤트를 발생 시킴
-   이때, 이벤트 타깃은 li요소이고 커런트 타깃은 ul 요소 이다

```jsx
<ul id="fruits">
    <li id="apple">Apple</li>
    <li id="banana">Banana</li>
    <li id="orange">Orange</li>
</ul>;

const fruits = document.getElementById("fruits");

// #fruits 요소의 하위 요소인 li요소를 클릭한 경우
fruits.addEventListener("click", (e) => {
    console.log(`이벤트 단계: ${e.eventPhase}`); // 이벤트 단계: 3 //// 3: 버블링 단계
    console.log(`이벤트 타깃: ${e.target}`); // 이벤트 타깃: [object HTMLLIElement]
    console.log(`이벤트 타깃: ${e.currentTarget}`); // 이벤트 타깃: [object HTMLUListElement]
});
```

-   li 요소를 클릭하면 클릭 이벤트 발생하여 이벤트 객체가 생성되고 클릭된 li 요소가 타깃이 된다
-   이때, 클릭 이벤트 객체는 window에서 시작해서 이벤트 타깃 방향으로 전파되는데 이것이 캡처링 단계이다
-   이후 이벤트 객체는 이벤트를 발생시킨 이벤트 타깃에 도달하는데 이것이 타깃 단계 이다
-   이후 이벤트 객체는 이벤트 타깃에서 시작해서 window 방향으로 전파 되는데 이것이 버블링 단계이다
-   이처럼 이벤트는 이벤트를 발생시킨 이벤트 타깃은 물론 상위 DOM 요소에서도 캐치 할 수 있다

## **40.7 이벤트 위임**

-   사용자가 내비게이션 아이템을 클릭하여 선택하면 현재 선택된 내비게이션 아이템에 active 클래스를 추가하고 그 외에는 active클래스를 제거하는 예제이다

```jsx
<nav>
    <ul id="fruits">
      <li id="apple" class="active">Apple</li>
      <li id="banana">Banana</li>
      <li id="orange">Orange</li>
    </ul>
</nav>
<div>선택된 내비게이션 아이템 <em class="msg">apple</em></div>

const fruits = document.getElementById('fruits')
const msg = document.querySelector('.msg')

function activate({target}) {
  [... fruits.children].forEach(fruit => {
      fruit.classList.toggle('active', fruit === target)
      msg.textContent = target.id
  })
}

document.getElementById('apple').onclick = activate;
document.getElementById('banana').onclick = activate;
document.getElementById('orange').onclick = activate;
```

-   모든 li 요소가 클릭 이벤트에 반응하도록 모든 아이템에 이벤트 핸들러인 activate를 등록함
-   만약 아이템이 100개라면 100개의 이벤트 핸들러를 등록해야 하는데 성능 저하의 원인이 되며 유지보수에도 적합하지 않음

-   이벤트 위임은 하나의 상위 DOM요소에 이벤트 핸들러를 등록하는 방법을 말한다

```jsx
const fruits = document.getElementById("fruits");
const msg = document.querySelector(".msg");

function activate({ target }) {
    // 이벤트를 발생시킨 요소가 #fruits의 자식이 아니면 무시
    if (!target.matches("#fruits > li")) return;

    [...fruits.children].forEach((fruit) => {
        fruit.classList.toggle("active", fruit === target);
        msg.textContent = target.id;
    });
}

// 이벤트 위임: 상위 요소는 하위 요소의 이벤트를 캐치 할 수 있음
fruits.onclick = activate;
```

## **40.8 DOM 요소의 기본 동작의 조작**

**40.8.1 DOM 요소의 기본 동작 중단**

-   DOM요소는 저마다 기본 동작이 있는데 이벤트 객체의 preventDefault 메서드는 이러한 DOM요소의 기본 동작을 중단 시킨다

```jsx
<a href="https://www.google.co.kr">gogo</a>;

document.querySelector("a").onclick = (e) => {
    // a 요소의 기본 동작을 중단함
    e.preventDafault();
};
```

**40.8.2 이벤트 전파 방지**

-   stopPropagation 메서드는 이벤트 전파를 중지 시킴

```jsx
// 상위 요소에서 이벤트를 캐치 할 수 없게 함
e.stopPropagation();
```

## **40.9 이벤트 핸들러 내부의 this**

**40.9.1 이벤트 핸들러 어트리뷰트 방식**

-   이벤트 핸들러 어트리뷰트의 값으로 지정한 문자열은 암묵적으로 생성되는 이벤트 핸들러의 문이라 했는데 따라서 handleClick 함수는 이벤트 핸들러에 의해 일반 함수로 호출되며 일반 함수로서 호출되는 함수 내부의 this는 전역 객체를 가리킨다
-   따라서 handleClick 함수 내부의 this는 전역 객체 window를 가리킨다
-   단, 이벤트 핸들러를 호출할 때 인수로 전달한 this는 이벤트를 바인딩한 DOM요소를 가리킴

```jsx
<button onclick="handleClick(this)">click me</button>;

function handleClick(button) {
    console.log(button); // 이벤트를 바인딩한 button 요소
    console.log(this); // window
}
```

-   handleClick 함수에 전달한 this는 암묵적으로 생성된 이벤트 핸들러 내부의 this로 이벤트를 바인딩한 DOM요소를 가리킨다

**40.9.2 이벤트 핸들러 프로퍼티 방식과 addEventListener 메서드 방식**

-   두 방식 모두 이벤트 핸들러 내부의 this는 이벤트를 바인딩한 DOM요소를 가리킴

```jsx
<button class="button1">0</button>
<button class="button2">0</button>

const button1 = document.querySelector('.button1')
const button2 = document.querySelector('.button2')

button1.onclick = function(e){
  // this는 이벤트를 바인딩한 DOM요소를 가리킴
  console.log(this) // button1
  console.log(e.currentTarget) // button1
  console.log(this === e.currentTarget) //true
}

button2.addEventListener('click', function(e){
  // this는 이벤트를 바인딩한 DOM 요소를 가리킴
  console.log(this) // button2
  console.log(e.currentTarget) // button2
  console.log(this === e.currentTarget) //true
})
```

-   화살표 함수로 정의한 이벤트 핸들러 내부의 this는 상위 스코프의 this를 가리킴 ⇒ 화살표 함수는 자체의 this바인딩을 가지지 않음

```jsx
button1.onclick = (e) => {
    // 화살표 함수 내부의 this는 상위 스코프의 this를 가리킴
    console.log(this); // window
    console.log(e.currentTarget); // button1
    console.log(this === e.currentTarget); //false
};

button2.addEventListener("click", (e) => {
    // 화살표 함수 내부의 this는 상위 스코프의 this를 가리킴
    console.log(this); // window
    console.log(e.currentTarget); // button2
    console.log(this === e.currentTarget); //false
});
```

## **40.10 이벤트 핸들러에 인수 전달**

-   이벤트 핸들러 어트리뷰트 방식은 함수 호출문을 사용할 수 있기 때문에 인수 전달 가능
-   이벤트 핸들러 프로퍼티와 addEventListener 메서드 방식의 경우 브라우저가 호출하기 때문에 인수를 전달할 수 없는데, 다음 예제와 같이 이벤트 핸들러 내부에서 함수를 호출하면서 인수를 전달할 수 있다

```jsx
<label>User name<input type="text"></label>
<em class="message"></em>

const MIN_USER_NAME_LENGTH = 5;
const input = document.querySelector('input')
const msg = documeng.querySelector('.message')

const checkUsernNameLength = min =>{
  msg.textContent = input.value.length < min ? `이름은 ${min}자 이상 입력해 주세요` : ''
};

// 이벤트 핸들러 내부에서 함수를 호출하면서 인수를 전달한다
input.onblur = () => {
  checkUsernNameLength(MIN_USER_NAME_LENGTH)
  }
```

-   또는 이벤트 핸들러를 반환하는 함수를 호출하면서 인수를 전달할 수도 있다

```jsx
<label>User name<input type="text"></label>
<em class="message"></em>

const MIN_USER_NAME_LENGTH = 5;
const input = document.querySelector('input')
const msg = documeng.querySelector('.message')

const checkUsernNameLength = min => e => {
  msg.textContent = input.value.length < min ? `이름은 ${min}자 이상 입력해 주세요` : ''
};

// 이벤트 핸들러 내부에서 함수를 호출하면서 인수를 전달한다
input.onblur = checkUsernNameLength(MIN_USER_NAME_LENGTH)
```

## **40.11 커스텀 이벤트**

**40.11.1 커스텀 이벤트 생성**

-   이벤트가 발생하면 이벤트 종류에 따라 타입이 결정되지만 Event, UIEvent, MouseEvent 같은 이벤트 생성자 함수를 호출하여 명시적으로 생성한 이벤트 객체는 임의의 이벤트 타입을 지정 할 수 있다 ⇒ 이를 이벤트 커스텀 이벤트라 함
-   이벤트 생성자 함수는 첫 번째 인수로 이벤트 타입을 나타내는 문자열을 전달 받는데 기존 이벤트 타입을 사용할수도 있고 임의의 문자열을 사용하여 새로운 이벤트 타입을 지정할 수도 있다 ⇒ 일반적으로 coustomEvent 이벤트 생성자 함수를 사용 함

```jsx
// keyBoardEvent 생성자 함수로 keyup 이벤트 타입의 커스텀 이벤트 객체를 생성
const keyboardEvent = new keyboardEvent("keyup");
console.log(keyBoardEvent.type); // keyup

// CustomEvent 생성자 함수로 foo 이벤트 타입의 커스텀 이벤트 객체를 생성
const customEvent = new CustomEvent("foo");
console.log(customEvent.type); // foo
```

-   생성된 커스텀 이벤트 객체는 버블링되지 않으며 preventDefault 메서드로 취소도 불가능하다 ⇒ bubbles와 cancelable 모두 기본값이 false

-   bubbles와 cancelable 프로퍼티뿐만 아니라 이벤트 타입에 따라 가지는 이벤트 고유의 프로퍼티 값을 지정할 수 있음

```jsx
// mouseEvent 생성자 함수로 click 이벤트 타입의 커스텀 이벤트 객체를 생성
const mouseEvent = new MouseEvent('click', {
		bubles: true;
		cancelable: true;
		clientX: 50;
		clientY: 100;
})

// keyBoardEvent 생성자 함수로 keyup 이벤트 타입의 커스텀 이벤트 객체를 생성
const keyboardEvent = new keyboardEvent('keyup', { key: 'Enter'})

console.log(keyboardEvent.key) //  Enter
```

-   이벤트 생성자 함수로 생성한 커스텀 이벤트는 isTrusted 프로퍼티의 값이 언제나 false이며 사용자의 행위에 의해 발생한 이벤트에 의해 생성된 이벤트 객체의 isTruste 프로퍼티 값은 언제나 true이다

```jsx
// InputEven 생성자 함수로 foo 이벤트 타입의 커스텀 이벤트 객체를 생성
const customEvent = new InputEvent('foo)
console.log(customEvent.inTrusted) // false
```

**40.11.2 커스텀 이벤트 디스패치**

-   생성된 커스텀 이벤트는 dispatchEvent 메서드로 디스패치(이벤트를 발생시키는 행위)할 수 있으며 이벤트 객체를 인수로 전달하면서 호출하면 인수로 전달한 이벤트 타입의 이벤트가 발생 함

```jsx
<button class="btn">click me</button>;

const btn = document.querySelector(".btn");

// 버튼 요소에 foo 커스텀 이벤트 핸들러를 등록
// 커스텀 이벤트를 디스패치 하기 이전에 이벤트 핸들러를 등록해야 함
btn.addEventListener("click", (e) => {
    console.log(e);
    alert(`${e} clicked`);
});

// 커스텀 이벤트 생성
const customEvent = new MouseEvent("click");

// 커스터 이벤트 디스패치(동기 처리) click 이벤트가 발생 함
btn.dispatchEvent(customEvent);
```

-   일반적으로 이벤트 핸들러는 비동기 처리 방식으로 동작하지만 dispatchEvent 메서드는 동기 처리 방식으로 호출 함
-   dispatchEvent 메서드를 호출하면 커스텀 이벤트에 바인딩된 이벤트 핸들러를 직접 호출하는 것과 같음
-   따라서, dispatchEvent 메서드로 이벤트를 디스패치 하기 이전에 커스텀 이벤트를 처리할 이벤트 핸들러를 등록해야 함

-   기존 이벤트 타입이 아닌 임의의 이벤트 타입을 지정하여 이벤트 객체를 생성하는 경우 CustomEvent 이벤트 생성자 함수를 사용함
-   CustomEvent 이벤트 생성자 함수에는 두 번째 인수로 이벤트와 함께 전달하고 싶은 정보를 담음 detail 프로퍼티를 포함하는 객체를 전달할 수 있는데 이 정보는 이벤트 객체의 detail 프로퍼티에 담겨 전달 된다

```jsx
<button class="btn">click me</button>;

const btn = document.querySelector(".btn");

// 버튼 요소에 foo 커스텀 이벤트 핸들러를 등록
// 커스텀 이벤트를 디스패치 하기 이전에 이벤트 핸들러를 등록해야 함
btn.addEventListener("foo", (e) => {
    // e.detail에는 customEvent 함수의 두 번째 인수로 전달한 정보가 담겨 있음
    alert(e.detail.message);
});

// customEvent 생성자 함수로 foo 이벤트 타입의 커스텀 이벤트 객체를 생성
const customEvent = new CustomEvent("foo", {
    detail: { message: "hello" }, // 이벤트와 함께 전달하고 싶은 정보
});

// 커스텀 이벤트 디스패치
btn.dispatchEvent(customEvent);
```

-   기존 이벤트 타입이 아닌 임의의 이벤트 타입을 지정하여 터스텀 이벤트 객체를 생성한 경우 반드시 addEventListener 메서드 방식으로 이벤트 핸들러를 등록해야 함

# 41장 타이머

Created: December 11, 2022 8:44 PM

## **41.1 호출 스케줄링**

-   함수를 명시적으로 호출하지 않고 일정 시간이 경과된 이후에 호출되도록 예약하려면 타이머 함수를 사용해야 함 ⇒ 이를 호출 스케줄링이라 함
-   타이머를 생성할 수 있는 함수 setTimeout과 setInterval은 모두 일정 시간이 경과된 이후 콜백 함수가 호출되도록 타이머를 생성
-   setTimeout 함수가 생성한 타이머는 단 한번만 동작 / setInterval 함수가 생성한 타이머는 반복 동작
-   타이머를 제거할 수 있는 타이머 함수 cleanTimeout과 cleanInterval을 제공 함
-   자바스크립트 엔진은 단 하나의 실행 컨텍스트 스택을 갖기 때문에 두 가지 이상의 태스크를 동시에 실행할 수 없는 싱글 스레드로 동작하는데 이런 이유로 타이머 함수 setTimeout과 setInterval은 비동기 처리 방식으로 동작 한다

## **41.2 타이머 함수**

**41.2.1 setTimeout / cleanTimeout**

-   setTimeout 함수는 두 번째 인수로 전달받은 시간(ms, 1/1000초)으로 단 한번 동작하는 타이머를 생성 함
-   이후, 타이머가 만료되면 첫 번째 인수로 전달받은 콜백 함수가 호출 됨

```jsx
const timeoutId = setTimeout(func|code[, delay, param1, param2, ...])
```

| 매개변수                           | 설명                                                                                              |
| ---------------------------------- | ------------------------------------------------------------------------------------------------- |
| func                               | 타이머가 만료된 뒤 호출될 콜백 함수                                                               |
| delay                              | 타이머 만료 시간 setTimeout 함수는 delay 시간으로 단 한 번 동작하는 타이머를 생성 함              |
| 인수 전달을 생략한 경우 기본값은 0 |
| param1, param2                     | 호출 스케줄링된 콜백 함수에 전달해야 할 인수가 존재하는 경우 세 번째 히우의 인수로 전달할 수 있음 |

```jsx
// 1초(1000 ms) 후 타이머가 만료되면 콜백 함수가 호출 됨
setTimeout(() => console.log("hi"), 1000);

// 1초(1000 ms) 후 타이머가 만료되면 콜백 함수가 호출 됨
// 이때 콜백 함수에 'Lee'가 인수로 전달 됨
setTimeout((name) => console.log(`hi ${name}`), 1000, "Lee");

// 두 번째 인수(delay)를 생략하면 기본값 0이 지정됨
setTimeout((name) => console.log(`hello`));
```

-   cleanTimeout 함수는 호출 스케줄링을 취소 한다

```jsx
// 1초(1000 ms) 후 타이머가 만료되면 콜백 함수가 호출 됨
// setTimeout 함수는 생성된 타이머를 식별할 수 있는 고유한 타이머 id를 반환함
const timeoutId = setTimeout(() => console.log(`hi`), 1000);

// setTimeout 함수가 반환한 타이머 id를 cleanTimeout 함수의 인수로 전달하여 타이머를 취소 함
// 타이머가 취소되면 setTimeout 함수의 콜백 함수가 실행 되지 않음
cleanTimeout(timeoutId);
```

**41.2.2 setInterval / cleanInterval**

-   setInverval 함수는 두 번째 인수로 전달받은 시간으로 반복 동작하는 타이머를 생성 함
-   cleanInterval 함수는 호출 스케줄링을 취소 함

```jsx
let count = 1;

const timeoutid = setInverval(() => {
    console.log(count); // 1 2 3 4 5

    if (count++ === 5) cleanIntervla(timeoutid);
}, 1000);
```

## **41.3 디바운스와 스로틀**

-   scroll, resize, input mousemove, mouseover 같은 이벤트는 짧은 시간 간격으로 연속해서 발생하는데 이러한 이벤트 핸들러는 과도하게 호출되어 성능에 문제를 일으킬 수 있다
-   디바운스와 스로틀은 연속해서 발생하는 이벤트를 그룹화 하여 과도한 이벤트 핸들러의 호출을 방지하는 프로그래밍 기법임

**41.3.1 디바운스**

-   짧은 시간 간격으로 이벤트가 연속해서 발생하면 호출하지 않다가 일정 시간이 경과한 이후에 이벤트 핸들러가 한 번만 호출 되도록 한다

```jsx
<input type="text">
<div class="msg"></div>

const input = document.querySelector('input')
const msg = document.querySelector('msg')

const debounce = (callback, delay) => {
  let timeId;

  // debounce 함수는 timeId을 기억하는 클로저를 반환 함
  return event => {

    // delay가 경과하기 이전에 이벤트가 발생하면 이전 타이머를 취소하고 새로운 타이머를 재설정 함
    // 따라서 delay보다 짧은 간격으로 이벤트가 발생하면 callback은 호출 되지 않음
    if(timeId) cleanTimeout(timeId)
    timeId = setTimeout(callback, delay, event)
  }
}

// debounce 함수가 반환하는 클로저가 이벤트 핸들러로 등록 됨
// 300ms보다 짧은 간격으로 input 이벤트가 발생하면 debounce 함수의 콜백 함수는
// 호출되지 않다가 300ms 동안 input 이벤트가 더 이상 발생하면 한 번만 호출 됨
input.onpoint = debounce(e => {
  msg.textContent = e.target.value
}, 300)
```

-   input 이벤트는 사용자가 텍스트 입력 필드에 값을 입력할 때마다 연속해서 발생하는데 사용자가 아직 입력을 완료하지 않았어도 Ajax 요청이 전송될 것인데 이는 서버에 부담을 주는 불필요한 처리이므로 사용자가 입력을 완료했을 때 한 번만 보내는 것이 바람직 하다
-   사용자가 입력을 완료했는지 여부는 정확히 알 수 없으므로 일정 시간 동안 텍스트 입력 필드에 값을 입력하지 않으면 입력이 완료된 것으로 간주 한다
-   이를 위해 debounce 함수가 반환한 함수는 debounce 함수에 두 번째 인수로 전달한 시간 보다 짧은 간격으로 이벤트가 발생하면 이전 타이머를 취소하고 새로운 타이머를 재설정 함
-   따라서, delay보다 짧은 간격으로 이벤트가 연속해서 발생하면 debounce 함수의 첫 번째 인수로 전달한 콜백 함수는 호출되지 않다가 delay동안 input 이벤트가 더 이상 발생하지 않으면 한 번만 호출 됨

![Untitled](41%E1%84%8C%E1%85%A1%E1%86%BC%20%E1%84%90%E1%85%A1%E1%84%8B%E1%85%B5%E1%84%86%E1%85%A5%20fcc38af49e304d93ac205c0beda62d12/Untitled.png)

-   resize, input 요소에 입력된 값으로 ajax 요청하는 입력 필드 자동완성, UI구현, 버튼 중복 클릭 방지 처리 등에 유용하게 사용 함
-   실무에서는 Underscore의 debounce 함수나 Lodash의 debounce 함수를 사용하는 것을 권장 함

**41.3.2 스로틀**

-   짧은 시간 간격으로 이벤트가 연속해서 발생하더라도 일정 시간 간격으로 이벤트 핸들러가 최대 한 번만 호출 되도록 함

```jsx
<div class="container">
    <div class="content"></div>
</div>

<div>
  일반 이벤트 핸들러가 scroll 이벤트를 처리한 횟수
  <span class="nomal-count">0</span>
</div>

<div>
  스트롤 이벤트 핸들러가 scroll 이벤트를 처리한 횟수
  <span class="throttle-count">0</span>
</div>

const container = document.querySelector('container')
const $nomalCount = document.querySelector('nomal-count')
const $throttleCount = document.querySelector('throttle-count')

const throttle = (callback, delay) => {
  let timeId;

	// throttle 함수는 timeId를 기억하는 클로저를 반환함
  return event => {
    if(timeId) return;

		// delay가 경과하기 이번에 이벤트가 발생하면 아무것도 하지 않다가
		// delay가 경과했을 때 이벤트가 발생하면 새로운 타이머를 재설정 함
		// 따라서 delay 간격으로 callback이 호출 됨
    timeId = setTimeout(() => {
      callback(event)
      timeId = null
    }, delay, event)
  }
}

let normalCount = 0;
container.addEventListener('scroll', ()=>{
  $nomalCount.textContent = ++nomalCount;
})

let throttleCount = 0;
// throttle 함수가 반환하는 클로저가 이벤트 핸들러로 등록 됨
container.addEventListener('scroll', ()=>{
  $throttleCount.textContent = ++throttleCount;
}, 100)
```

-   scroll이벤트는 사용자가 스크롤 할 때 잛은 시간 간격으로 연속해서 발생하는데 과도한 이벤트 핸들러의 호출을 방지하기 위해 throttle 함수는 이벤트를 구릅화 하여 일정 시간 단위로 이벤트 핸들러가 호출되도록 호출 주기를 만든다
-   throttle 함수가 반환한 함수는 delay 시간이 경과하기 이전에 이벤트가 발생하면 아무것도 하지 않다가 시간이 광과 했을 때 이벤트가 발생하면 콜백 함수를 호출하고 새로운 타이머를 재설정 한다, 따라서 delay 시간 간격으로 콜백 함수가 호출 된다

![Untitled](41%E1%84%8C%E1%85%A1%E1%86%BC%20%E1%84%90%E1%85%A1%E1%84%8B%E1%85%B5%E1%84%86%E1%85%A5%20fcc38af49e304d93ac205c0beda62d12/Untitled%201.png)

-   scroll 이벤트 처리나 무한 스크롤 UI 구현 등에 유용하게 사용된다
-   실무에서는 Underscore의 throttle 함수나 Lodash의 throttle 함수를 사용하는 것을 권장 함
