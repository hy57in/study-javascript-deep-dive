# 38장 브라우저의 렌더링 과정

Created: December 4, 2022 6:54 PM

-   파싱
    -   파싱은 프로그래밍 언어의 문법에 맞게 작성된 텍스트 문서를 읽어 들여 실행하기 위해 텍스트 문서의 문자열을 토큰으로 분석하고 토큰에 문법적 의미와 구조를 반영하여 트리 구조의 자료구조인 파스 트리를 생성하는 일련의 과정을 말함
    -   일반적으로 파싱이 완료된 이후에는 파스 트리를 기반으로 중간 언어인 바이트코드를 생성하고 실행함
-   렌더링

    -   HTML, CSS, 자바스크립트로 작성된 문서를 파싱하여 브라우저에 시각적으로 출력하는 것을 말함

-   브라우저는 다음과 같은 과정을 거쳐 렌더링을 수행 함
    1. 브라우저는 HTML, CSS, 자바스크립트, 이미지, 폰트 파일 등 렌더링에 필요한 리소스를 요청하고 서버로부터 응답을 받음
    2. 브라우저 렌더링 엔진은 서버로부터 응답된 HTML과 CSS파싱하여 DOM과 CSSOM을 생성하고 이들을 결합하여 렌더 트리를 생성 함
    3. 브라우저의 자바스크립트 엔진은 서버로부터 응답된 자바스크립트를 파싱하여 생성하고 바이트코드로 변환하여 실행 함
    4. 렌더 트리를 기반으로 HTML 요소의 레이아웃을 계산하고 브라우저 화면에 요소를 페인팅 함

**38.1 요청과 응답**

-   브라우저의 핵심 기능흔 필요한 리소스를 서버에 요청하고 응답받아 브라우저에 시각적으로 렌더링하는 것 임
-   서버에 요청을 전송하기 위해 브라우저는 주소창을 제공하며 url을 입력하면 호스트 이름이 DNS를 통해 IP주소로 변환되고 서버에게 요청을 전송 함

**38.2 HTTP 1.1과 HTTP 2.0**

-   http는 웹에서 브라우저와 서버가 통신하기 위한 프로토콜임
-   http/1.1은 커넥션 당 하나의 요청과 응답만 처리 해 리소스의 동시 전송이 불가능한 구조로 요청한 리소스의 개수에 비례하여 응답 시간도 증가하는 단점이 있음
-   http/2는 커넥션 당 여러 개의 요청과 응답이 가능하고 여러 리소스의 동시 전송이 가능하므로 1.1에 비해 페이지 로드 속도가 50% 정도 빠르다고 알려져 있음

**38.3 HTML 파싱과 DOM 생성**

-   html 문서는 문자열로 이루어진 순수한 텍스트로 브라우저에 시각적인 픽셀로 랜더링하려면 브라우저가 이해할 수 있는 자료구조로 변환하여 메모리에 저장해야 함
    1. 서버에 존재하던 HTML 파일이 브라우저 요청에 의해 응답되어 파일을 읽어 들여 메모리에 저장한 다음 메모리에 저장된 바이트를 인터넷을 경유하여 응답 함
    2. 브라우저는 서버가 응답한 문서를 바이트 형태로 응답받아 meta 태그의 charset 어트리뷰트에 의해 지정된 인코딩 방식을 기준으로 문자열로 반환함
    3. 문자열로 변환된 문서를 읽어 들여 토큰으로 분해 함
    4. 각 토큰들을 객체로 변환하여 노드를 생성하는데 그 내용에 따라 문서, 요소, 어트리뷰트, 텍스트 노드가 생성되고 돔을 구성하는 기본 요소가 된다
    5. html 요소는 중첩 관계를 갖으며 콘텐츠 영역에는 중첩 관계에 의해 부자 관계가 형성된다 이런 관계를 반영하여 모든 노드를 트리 자료구조로 구성하며 이 노드늘로 구성된 트리 자료 구조를 돔이라 부름

**38.4 CSS파싱과 CSSOM 생성**

-   랜더링 엔진은 문서를 처음부터 한 줄씩 순차적으로 파싱하여 돔을 생성해 나가다가 css를 로드하는 link태그나 style태그를 만나면 돔 생성을 일시 중단함
-   css를 html과 동일한 파싱 과정(바이트→문자→토큰→노드→CSSOM)을 거치며 해석하여 CSSOM을 생성 한다
-   이후 css파싱을 완료하면 html 파싱이 중단된 지점에서 다시 html 을 파싱하기 시작하여 DOM생성을 재개 함

**38.5 렌더 트리 생성**

-   렌더링 엔진은 서버로부터 응답된 html과 css를 파싱하여 각각 dom과 cssom을 생성함 그리고 랜더링을 위해 렌더 트리로 결합 됨
-   이후 완성된 렌더 트리는 각 html 요소의 레이아웃을 계산하는 데 사용되며 브라우저 화면에 픽셀을 랜더링하는 페인팅 처리에 입력 됨

![Untitled](38%E1%84%8C%E1%85%A1%E1%86%BC%20%E1%84%87%E1%85%B3%E1%84%85%E1%85%A1%E1%84%8B%E1%85%AE%E1%84%8C%E1%85%A5%E1%84%8B%E1%85%B4%20%E1%84%85%E1%85%A6%E1%86%AB%E1%84%83%E1%85%A5%E1%84%85%E1%85%B5%E1%86%BC%20%E1%84%80%E1%85%AA%E1%84%8C%E1%85%A5%E1%86%BC%208682aa2821f84dd485a7dd5bfac4c068/Untitled.png)

-   브라우저의 랜더링 과정은 반복해서 실행될수 있는데 리렌더링은 비용이 많이 들고 성능에 악영향을 주기 때문에 가급적 리렌더링이 발생하지 않도록 주의할 필요가 있음

**38.6 자바스크립트 파싱과 실행**

-   DOM은 HTML 문서의 구조와 정보뿐만 아니라 HTML 요소와 스타일 등을 변경할 수 있는 프로그래밍 인터페이스로서 DOM API를 제공 함
-   자바스크립트의 파싱은 CSS파싱과 마찬가지로 렌더링 엔진이 HTML 문서를 한 줄씩 읽어 나가다 script태그를 만나면 자바스크립트 엔진에 제어권을 넘기며 시작 됨
-   이후 자바스크립트 파싱과 실행이 종료되면 렌더링 엔진으로 다시 제어권을 넘겨 HTML 파싱이 중단된 시점부터 다시 HTML 파싱을 시작하여 DOM 생성을 재개한다.
-   자바스크립트 파싱과 실행은 브라우저의 렌더링 엔진이 아닌 자바스크립트 엔진이 처리한다.
-   자바스크립트 엔진 또한 렌더링 엔진과 마찬가지로 자바스크립트 코드를 해석하여 AST(추상적 구문 트리)를 생성한다. 그리고 AST를 기반으로 인터프리터가 실행할 수 있는 중간 코드인 바이트 코드를 생성하여 실행한다.

![https://velog.velcdn.com/images%2Fturtle601%2Fpost%2Feb500487-3c54-4f75-8576-73faeccbaa80%2F%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA%202022-01-18%20%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE%208.19.53.png](https://velog.velcdn.com/images%2Fturtle601%2Fpost%2Feb500487-3c54-4f75-8576-73faeccbaa80%2F%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA%202022-01-18%20%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE%208.19.53.png)

**토크나이징**

-   단순한 문자열인 자바스크립트 소스코드를 어휘 분석하여 문법적 의미를 갖는 코드의 최소 단위인 토큰으로 분해하며 이과정을 렉싱이라고 부르기도 하지만 토크나이징과 미묘한 차이가 있음

**파싱**

-   토큰들의 집합을 구문분석하여 AST(추상적 구문 트리)를 생성하며 토큰에 문법적 의미와 구조를 반영한 트리 구조의 자료구조 임
-   AST를 사용하면 TypeScript, Babel, Prettier 같은 트랜스파일러를 구현할수도 있음

**바이트코드 생성과 실행**

-   파싱의 결과물로서 생성된 AST는 인터프리터가 실행할 수 있는 중간 코드인 바이트코드로 변환되고 인터프리터에 의해 실행 됨

**38.7 리플로우와 리페인트**

-   자바스크립트 코드에 DOM이나 CSSOM을 변경하는 DOM API가 사용된 경우 DOM이나 CSSOM이 변경된다. 변경된 DOM과 CSSOM은 다시 렌더 트리로 결합되고 변경된 렌더 트리를 기반으로 레이아웃과 페인트 과정을 거쳐 브라우저 화면에 다시 렌더링하는데 이를 리플로우, 리페인트 라 함
-   리플로우는 레이아웃을 다시 계산하는 것을 말하며 노드 추가/삭제, 요소의 크기/위치 변경, 윈도우 리사이징 등에 레이아웃에 영향을 주는 변경이 발생한 경우에 한하여 실행 됨
-   리페인트는 재결합된 렌더 트리를 기반으로 다시 페인트를 하는 것을 말함

**38.8 자바스크립트 파싱에 의한 HTML 파싱 중단**

-   렌더링 엔진과 자바스크립트 엔진은 직렬적으로 파싱을 수행 함
-   브라우저는 동기적으로 순차적으로 파싱하고 실행하며 이것은 script 태그의 위치에 따라 파싱이 블로킹되어 돔 생성이 지연될수도 있다는 것을 의미함
-   자바스크립트 코드는 body 요소의 가장 아래에 위치하는 것이 좋음
    -   돔이 완성되지 않은 상태에서 자바스크립트가 돔을 조작하면 에러가 발생할 수 있음
    -   로딩/파싱/실행으로 인해 html 요소들의 렌더링에 지장받는 일이 발생하지 않아 페이지 로딩 시간이 단축 됨

**38.9 script 태그의 async/defer 어트리뷰트**

-   자바스크립트 파싱에 의한 DOM 생성이 중단 되는 문제를 근본적으로 해결하기 위해 HTML5부터 script 태그에 async와 defer 어트리뷰트가 추가 되었음
-   async와 defer 어트리뷰트는 src 어트리뷰트를 통해 외부 자바스크립트 파일을 로드하는 경우에만 사용 할 수 있음

**async 어트리뷰트**

-   html 파싱과 외부 자바스크립트 파일의 로드가 비동기적으로 동시에 진행된다
-   단, 파싱과 실행은 자바스크립트 파일의 로드가 완료된 직후 진행되며 이때 html 파싱이 중단 됨
-   여러 개의 script 태그에 async 어트리뷰트를 지정하면 순서와 상관없이 로드가 완료되므로 순서가 보장되지 않아서 순서 보장이 필요한 script 태그에는 지정하지 않아야 함

![https://velog.velcdn.com/images%2Fturtle601%2Fpost%2F4bc82b62-e35e-4af7-adb5-2b3ed77a3668%2F%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA%202022-01-18%20%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE%208.47.02.png](https://velog.velcdn.com/images%2Fturtle601%2Fpost%2F4bc82b62-e35e-4af7-adb5-2b3ed77a3668%2F%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA%202022-01-18%20%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE%208.47.02.png)

**defer 어트리뷰트**

-   html파싱과 외부 자바스크립트 파일의 로드가 비동기적으로 동시에 진행 된다
-   단, 자바스크립트 파싱과 실행은 html파싱이 완료된 직후 진행되므로 돔 생성이 완료된 이후 실행되어야 할 자바스크립트에 유용함

![https://velog.velcdn.com/images%2Fturtle601%2Fpost%2Fda90867e-e28d-47ba-bd8f-b691acaaaa35%2F%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA%202022-01-18%20%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE%208.47.23.png](https://velog.velcdn.com/images%2Fturtle601%2Fpost%2Fda90867e-e28d-47ba-bd8f-b691acaaaa35%2F%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA%202022-01-18%20%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE%208.47.23.png)

# 39장 DOM

Created: December 2, 2022 9:25 AM

**39.1 노드**

**39.1.1 HTML 요소와 노드 객체**

-   HTML 요소는 문서를 구성하는 개별적인 요소를 의미하며 렌더링 엔진에 의해 파싱되어 DOM을 구성하는 요소 노드 객체로 변환된다

![Untitled](39%E1%84%8C%E1%85%A1%E1%86%BC%20DOM%203442c56469394d078690184bdedaeb7b/Untitled.png)

-   이때 html 요소의 어트리뷰트는 어트리뷰트 노드로, 텍스트 콘텐츠는 텍스트 노드로 변환된다

![Untitled](39%E1%84%8C%E1%85%A1%E1%86%BC%20DOM%203442c56469394d078690184bdedaeb7b/Untitled%201.png)

-   html 문서는 html 요소들의 집합으로 이뤄지며 중첩 관계를 갖으며 이때 중첩 관계에 의해 계층적인 부자 관계가 형성됨 ⇒ 이런 관계를 반영하여 html 요소를 객체화한 모든 노드 객체들을 트리 자료구조로 구성함

**트리 자료 구조**

-   트리 자료 구조는 노드들의 계층 구조(부모, 자식 노드)로 이루어진 노드 간의 계층적 구조를 표현하는 비선형 자료구조를 말함
-   하나의 최상위 노드에서 시작하며 최상위 노드는 부모 노드가 없고 루트 노드라 함
-   루트 노드는 0개 이상의 자식 노드를 갖으며 자식 노드가 없는 노드는 리프 노드라 함
-   노트 객체들로 구성된 트리 자료구조를 DOM이라 하며 DOM트리라 부르기도 함

**39.1.2 노드 객체의 타입**

-   다음 html 문서는 다음과 같이 DOM을 생성함

```jsx
<!DOCTYPE html>
<html lang="en">
    <head>
        <meta charset="UTF-8" />
        <link rel="stylesheet" href="style.css" />
    </head>
    <body>
        <ul>
            <li id="Apple">Apple</li>
            <li id="Banana">Banana</li>
            <li id="Orange">Orange</li>
        </ul>
        <script src="app.js"></script>
    </body>
</html>
```

![Untitled](39%E1%84%8C%E1%85%A1%E1%86%BC%20DOM%203442c56469394d078690184bdedaeb7b/Untitled%202.png)

**문서 노드**

-   돔트리의 최상위에 존재하는 루트 노드로서 document 객체를 가리킴
-   document 객체는 브라우저가 랜더링한 html 문서 전체를 가리키는 객체로서 전역 객체 window의 document 프로퍼티에 바인딩되어 있어 window.document 또는 document로 참조할 수 있음
-   브라우저 환경이 모든 자바스크립트 코드는 script 태그에 의해 분리되어 있어도 하나의 전역 객체 window를 공유하고 있으므로 모든 자바스크립트 코드는 하나의 document 객체를 바라봄 ⇒ html문서 당 document 객체는 유일함
-   document 객체는 DOM 트리의 루트 노드이므로 DOM 트리의 노드들에 접근하기 위한 진입점 역할을 담당한다 ⇒ 요소, 어트리뷰트, 텍스트 노드에 접근하려면 문서 노드를 통해야 함

**요소 노드**

-   html 요소를 가리키는 객체로 요소 간의 중첩에 의해 부자 관계를 가지며 이 관계를 통해 정보를 구조화 함, 따라서 요소 노드는 문서의 구조를 표현 함

**어트리뷰트 노드**

-   html요소의 어트리뷰트를 가리키는 객체로 어트리뷰트가 지정된 html 요소의 요소 노드와 연결되어 있음
-   어트리뷰트 노드는 부모 노드와 연결되어 있지 않고 요소 노드에만 연결되어 있음
-   어트리뷰트 노드는 부모 노드가 없으므로 요소 노드의 형제 노드는 아님 ⇒ 따라서 어트리뷰트 노드에 접근하여 어트리뷰트를 참조하거나 변경하려면 먼저 요소 노드에 접근 해야 함

**텍스트 노드**

-   html 요소의 텍스트를 가리키는 객체로 문서의 정보를 표현함
-   요소 노드의 자식 노드이며 자식 노드를 가질 수 없는 리프 노드로 DOM 트리의 최종단임
-   따라서 텍스트 노드에 접근하려면 먼저 부모 노드인 요소 노드에 접근해야 함

**39.1.3 노드 객체의 상속 구조**

-   DOM은 html 문서의 계층적 구조와 정보를 표현하며 이를 제어할 수 있는 API, 즉 프로퍼티와 메서드를 제공하는 트리 자료 구조이며 DOM을 구성하는 노드 객체는 자신의 구조와 정보를 제어할 수 있는 DOM API를 사용 할 수 있다
-   이를 통해 노드 객체는 자신의 부모, 형제, 자식을 탐색 할 수 있으며 자신의 어트리뷰트와 텍스트를 조작할 수도 있다

![Untitled](39%E1%84%8C%E1%85%A1%E1%86%BC%20DOM%203442c56469394d078690184bdedaeb7b/Untitled%203.png)

-   위와 같이 모든 노드 객체는 Object, EventTarget, Node 인터페이스를 상속 받음
-   문서 노드는 document, HTMLDocument 인터페이스를 상속받고 어트리뷰트 노드는 Attr, 텍스트 노드는 CharacterDate를 상속 받는다
-   요소 노드는 Element 인터페이스를 상속 받음
-   프로토타입 체인 관점에서 보면 input 요소 노드 객체는 프로토타입 체인에 있는 모든 프로토타입의 프로퍼티나 메서드를 상속받아 사용할 수 있다

![Untitled](39%E1%84%8C%E1%85%A1%E1%86%BC%20DOM%203442c56469394d078690184bdedaeb7b/Untitled%204.png)

| input 요소 노드 객체의 특성                                                | 프로토타입을 제공하는 객체 |
| -------------------------------------------------------------------------- | -------------------------- |
| 객체                                                                       | Object                     |
| 이밴트를 발생시키는 객체                                                   | EventTarget                |
| 트리 자료구조의 노드 객체                                                  | Node                       |
| 브라우저가 렌더링할 수 있는 웹 문서의 요소(HTML, XML, SVG)를 표현하는 객체 | Element                    |
| 웹 문서의 요소 중에서 HTML 요소를 표현하는 객체                            | HTMLElement                |
| HTML 요소 중에서 input 요소를 표현하는 객체                                | HTMLInputElement           |

-   노드 객체의 상속 구조는 개발자 도구의 Elements 패널 우측의 properties패널에서 확인할 수 있음
-   노드 객체는 종류 및 타입에 상관없이 공통으로 갖는 기능도 있고 고유한 기능도 있음
-   DOM은 html 문서의 계층적 구조와 정보를 표현하는 것은 물론 노드 객체의 종류, 즉 노드 타입에 따라 필요한 기능을 프로퍼티와 메서드의 집합인 DOM API로 제공한다 ⇒ 이 DOM API를 통해 html의 구조나 내용 또는 스타일 등을 동적으로 저작할 수 있음

**39.2 요소 노드 취득**

```jsx
<!DOCTYPE html>
<html lang="en">
    <head>
        <meta charset="UTF-8" />
        <link rel="stylesheet" href="style.css" />
    </head>
    <body>
        <ul id="fruits">
            <li id="apple" class="fruit apple">Apple</li>
            <li id="banana" class="fruit banana">Banana</li>
            <li id="orange" class="fruit orange">Orange</li>
        </ul>
    </body>
</html>
```

**39.2.1 id를 이용한 요소 노드 취득**

-   Document.prototype.getElementById 메서드는 인수로 전달한 id 어트리뷰트 값을 갖는 하나의 요소 노드를 탐색하여 반환함

```jsx
// id값이 'banana'인 요소 노드를 탐색하여 반환함
// 두 번째 li 요소가 파싱되어 생성된 요소 노드가 반환됨
const elem = document.getElementById("banana");

// 취득한 요소 노드의 style.color 프로퍼티 값을 변경함
elem.style.color = "red";
```

-   id값은 유일한 값이어야 함 단, 여러개이더라도 어떠한 에러도 발생하지 않음
-   id가 여러개일 경우 getElementById 메서드는 인수로 전달된 id 값을 갖는 첫 번째 요소 노드만 반환됨
    ⇒ getElementById 메서드는 언제나 단 하나의 요소만 반환한다는 뜻
-   인수로 전달된 id값을 갖는 html 요소가 존재하지 않는 경우 null을 반환함

```jsx
<!DOCTYPE html>
<html lang="en">
    <head>
        <meta charset="UTF-8" />
        <link rel="stylesheet" href="style.css" />
    </head>
    <body>
        <div id="foo"></div>
        <script>
            // id 값과 동일한 이름의 전역 변수가 암묵적으로 선언되고 해당 노드 객체가 할당됨
            console.log(foo === document.getElementById('foo')) //true

            // 암묵적 전역으로 생성된 프로퍼티는 삭제되지만 전역 변수는 삭제되지 않음
            delete foo;
            console.log(foo) // <div id="foo"></div>
        </script>
    </body>
</html>
```

-   id를 부여하면 id값과 동일한 이름의 전역 변수가 암묵적으로 선언되고 해당 노드 객체가 할당되는 부수 효과가 있음
-   id값과 동일한 이름의 전역 변수가 선언되어 있으면 노드 객체가 재할당 되지 않음

**39.2.2 태그 이름을 이용한 요소 노드 취득**

-   Document.prototype/Element.prototype.getElementsByTagName 메서드는 인수로 전달한 태그 이름을 갖는 모든 요소 노드들을 탐색하여 반환함
-   여러 개의 요소 노드 객체를 갖는 DOM 컬렉션 객체인 HTMLCollection 객체를 반환함
-   getElementsByTagName 메서드가 반환하는 DOM 컬렉션 객체인 HTMLCollection 객체는 유사 배열 객체이면서 이터러블임

```jsx
const elems = document.getElementsByTagName('li')

// 취득한 모든 요소 노드의 style.color 프로퍼티 값을 변경함
[...elems].forEach((elem) => {elem.style.color = "red"});
```

![Untitled](39%E1%84%8C%E1%85%A1%E1%86%BC%20DOM%203442c56469394d078690184bdedaeb7b/Untitled%205.png)

-   모든 요소 노드를 취득하려면 인수로 \*을 전달함

```jsx
// 모든 요소 노드를 탐색하여 반환함
const all = document.getElementsByTagName("*");
```

**39.2.3 class를 이용한 요소 노드 취득**

-   Document.prototype/Element.prototype.getElementByClassName 메서드는 인수로 전달한 class 어트리뷰트 값을 갖는 모든 요소 노드를 탐색하여 반환함
-   인수로 전달할 class값은 공백으로 구분하여 여러 개의 class를 지정할 수 있음

```jsx
// class 값이 fruit인 요소 노드를 모드 탐색하여 HTMLColletion 객체에 담아 반환함
const elems = document.getElementsByClassName("fruit");

// class 값이 fruit apple 인 요소 노드를 모두 탐색하여 HTMLColletion 객체에 담아 반환함
const elems = document.getElementsByClassName("fruit apple");
```

**39.2.4 CSS 선택자를 이용한 요소 노드 취득**

-   CSS선택자는 스타일을 적용하고자 하는 html 요소를 특정할 때 사용하는 문법임

```jsx
// 전체 선택자
* {...}

// 태그 선택자
p {...}

// id 선택자
#foo {...}

// class 선택자
.foo {...}

// 어트리뷰트 선택자 - input type 중 어트리뷰트 값이 text인 요소 모두 선택
input[type=text] {...}

// 후손 선택자 - div 요소 후손 중 p 요소 선택
div p {...}

// 자식 선택자 - div 자식 중 p 요소 선택
div > p {...}

// 인접 형제 선택자 - p요소 형제 중에 p요소 바로 뒤에 위치하는 ul 요소를 선택
p + ul {...}

// 일반 형제 선택자 - p요소 형제 중에 p요소 뒤에 위치하는 ul 요소를 모두 선택
p ~ ul {...}

// 가상 클래스 선택자 - hover 상태인 a 요소를 모두 선택
a:hover {...}

// 가상 요소 선택자 - p요소의 콘텐츠의 앞에 위치하는 공간을 선택
p::before {...}
```

-   Document.prototype/Element.prototype.querySelector 메서드는 인수로 전달한 CSS 선택자를 만족시키는 하나의 요소 노드를 탐색하여 반환함

```jsx
// class 어트리뷰트 값이 'banana'인 첫 번째 요소 노드를 탐색하여 반환함
const elem = document.querySelector(".banana");
```

-   Document.prototype/Element.prototype.querySelectorAll 메서드는 인수로 전달한 CSS 선택자를 만족시키는 모든 요소 노드를 탐색하여 반환함

```jsx
// ul 요소의 자식 요소인 li 요소를 모두 탐색하여 반환
const elems = document.querySelectorAll("ul > li");
// NodeList(3) [li#apple.fruit.apple, li#banana.fruit.banana, li#orange.fruit.orange]

// 모든 요소 노드를 취득하려면 인수로 전체선택자 *를 전달함
const all = document.querySelectorAll("*");
// NodeList(10) [html, head, meta, link, body, ul, li#apple.fruit.apple, li#banana.fruit.banana, li#orange.fruit.orange, script]
```

-   CSS 선택자 문법을 사용하는 두 메서드는 getElementBy\*\*\* 메서드보다 다소 느린것으로 알려져 있지만 해당 문법을 사용하면 좀 더 구체적인 조건으로 요소 노드를 취득할 수 있고 일관된 방식으로 요소 노드를 취득할 수 있다는 장점이 있음
-   따라서, id 어트리뷰트가 있는 요소 노드를 취득하는 경우에는 getElementById를 사용하고 그 외에 경우에는 CSS 선택자 메서드를 사용하는 것을 권장함

**39.2.5 특정 요소 노드를 취득할 수 있는지 확인**

-   Element.prototype.matches 메서드는 인수로 전달한 CSS 선택자를 통해 특정 요소 노드를 취득할 수 있는지 확인함
-   이벤트 위임을 사용할때 유용함

```jsx
const apple = document.querySelector(".apple");

// apple 노드는 #fruits > li.apple로 취득할 수 있음
console.log(apple.matches("#fruits > li.apple")); // true

// apple 노드는 #fruits > li.banana취득할 수 없음
console.log(apple.matches("#fruits > li.banana")); // false
```

**39.2.6 HTMLCollection과 NodeList**

-   DOM API가 여러 개의 결과값을 반환하기 위한 DOM컬렉션 객체로 모두 유사 배열 객체이면서 이터러블이다
-   따라서, for … of문으로 순회할 수 있으며 스프레드 문법을 사용하여 간단히 배열로 변환할 수 있다
-   **노드 객체의 상태 변화를 실시간으로 반영하는 살아 있는 객체** 라는 것 임

-   HTMLCollection은 언제나 live 객체로 동작함
-   NodeList는 대부분 과거의 정적 상태를 유지하는 non-live객체로 동작하지만 경우에 따라 live 객체로 동작할 때가 있음

**HTMLCollection**

-   getElementByTagName, getElementByClassName 메서드가 반환하는 HTMLCollection 객체는 노드 객체의 상태 변화를 실시간으로 반영하는 살아있는 DOM컬렉션임 ⇒ 살아있는 객체라고 부름

```jsx
<!DOCTYPE html>
<html lang="en">
    <head>
        <style>
            .red {color: red;}
            .blue {color: blue;}
        </style>
        <meta charset="UTF-8" />
        <link rel="stylesheet" href="style.css" />
    </head>
    <body>
        <ul id="fruits">
            <li class="red">Apple</li>
            <li class="red">Banana</li>
            <li class="red">Orange</li>
        </ul>
        <script>
            const elems = document.getElementsByClassName("red");

            console.log(elems);

            for (let i = 0; i < elems.length; i++) {
                elems[i].className = "blue";
            }

            console.log(elems);
        </script>
    </body>
</html>
```

![Untitled](39%E1%84%8C%E1%85%A1%E1%86%BC%20DOM%203442c56469394d078690184bdedaeb7b/Untitled%206.png)

-   모든 li의 요소의 class 값이 blue로 변경될 것 같지만 두 번째 li 요소에는 적용되지 않음, 이유는?

    1. 첫 번째 반복(i === 0)

    -   elem[0] 첫 번째 li 요소는 class 값이 blue로 변경됨
    -   className 메서드의 인자로 전달한 red와 일치하지 않기 때문에 elems에서 실시간으로 제거 됨 ⇒ 이처럼 실시간으로 노드 객체의 상태 변경을 반영하는 살아있는 DOM컬렉션 임

    1. 두 번째 반복(i === 1)

    -   첫 번째 반복에서 첫 번째 li 요소는 제거 됨
    -   따라서 elems[1]은 세 번째 li요소 임 ⇒ 이 세 번째 li요소의 class 값도 blue로 변경되고 elems에서 실시간으로 제외 됨

    1. 세 번째 반복(i === 2)

    -   첫 번째, 두 번째 반복에서 세 번째 li 요소가 elems에서 제거 됨 따라서 elems에는 두 번째 li 요소 노드만 남음
    -   elems.length는 1이므로 for문의 조건식 i < elems.length가 false로 평가되어 반복이 종료 됨
    -   따라서 elems에 남아 있는 두 번째 li 요소의 class 값은 변경되지 않음

-   이처럼 객체는 실시간으로 노드 객체의 상태 변경을 반영하여 요소를 제거 할 수 있기 때문에 for문으로 순회하면서 노드 객체의 상태를 변경 할 때 주의 해야 함

```jsx
// for 문을 역방향으로 순회하는 방법으로 회피 할 수 있음
for(let i=elems.length - 1; i >= 0; i--){
		elems[i].className = 'blue'
}

==========================================================

// while문으로 HTMLCollection에 요소가 남아 있지 않을 때까지 무한 반복
let i = 0;
while (elems.length > i) {
		elems[i].className = 'blue'
}

===========================================================

// 유사 배열 객체이면서 이터러블인 HTMLCollection을 사용하지 않고
// 유용한 배열의 고차함수(forEach, map, filter, reduce)를 사용
[...elems].forEach(elems => elem.className = 'blue')
```

**NodeList**

-   HTMLCollection 객체의 부작용을 해결하기 위해 querySelectorAll 메서드를 사용하는 방법도 있음
-   NodeList 객체는 실시간으로 노드 객체의 상태 변경을 반영하지 않는 객체임

```jsx
const elems = document.querySelectorAll("red");

// NodeList객체는 Node.prototype.forEach 메서드를 상속받아 사용할 수 있음
elems.forEach((elem) => (elem.className = "blue"));
```

-   childNodes 프로퍼티가 반환하는 NodeList객체는 HTMLCollection 객체와 같이 실시간으로 노드 객체의 상태 변경을 반영하는 live 객체로 동작하므로 주의 필요
-   따라서 노드 객체의 상태 변경과 상관없이 안전하게 사용하려면 HTMLCollection이나 NodeList객체를 배열로 변환하여 사용하는 것을 권장 함

**39.3 노드 탐색**

-   요소 노드를 취득한 다음 DOM트리의 노드를 옮겨 다니며 부모, 형제, 자식 노드 등을 탐색해야 할때가 있음

```jsx
<ul id="fruits">
    <li class="apple">Apple</li>
    <li class="banana">Banana</li>
    <li class="orange">Orange</li>
</ul>
```

-   fruits 요소는 3개의 자식 요소를 갖는데 ul#fruits 요소 노드를 취득한 다음 자식 노드를 모두 탐색하거나 자식 중 하나만 탐색할 수 있다
-   li.banana 요소는 2개의 형제 요소와 부모 요소를 갖는데 이때 먼저 li.banana 요소 노드를 취득한 다음 형제 노드를 탐색하거나 부모 노드를 탐색할 수 있다
-   이처럼 DOM 트리 상의 노드를 탐색할 수 있도록 Node, Element 인터페이스는 트리 탐색 프로퍼티를 제공 함

![Untitled](39%E1%84%8C%E1%85%A1%E1%86%BC%20DOM%203442c56469394d078690184bdedaeb7b/Untitled%207.png)

-   parentNode, previosSibling, firstChild, childNodes 프로퍼티는 Node.prototype이 제공하고 프로퍼티 키에 Element가 포함된 previousElementSibling, nextElementSibling과 children 프로퍼티는 Element.prototype이 제공한다
-   노드 탐색 프로퍼티는 모두 접근자 프로퍼티이지만 노드 탐색 프로퍼티는 setter없이 getter만 존재하여 참조만 가능한 읽기 전용 접근자 프로퍼티로 값을 할당하면 아무런 에러 없이 무시 됨

**39.3.1 공백 텍스트 노드**

-   html 요소 사이의 스페이스, 탭, 줄바꿈 등의 공백 문자는 텍스트 노드를 생성한다 ⇒ 이를 공백 텍스트 노드라 함

![Untitled](39%E1%84%8C%E1%85%A1%E1%86%BC%20DOM%203442c56469394d078690184bdedaeb7b/Untitled%208.png)

-   이처럼 공백 문자는 공백 텍스트 노드를 생성하기 때문에 주의해야 함

**39.3.2 자식 노드 탐색**

-   자식 노드를 탐색하기 위해 다음과 같은 노드 탐색 프로퍼티를 사용한다

| 프로퍼티                            | 반환 노드    | 설명                                                                                        |
| ----------------------------------- | ------------ | ------------------------------------------------------------------------------------------- |
| Node.prototype.childNodes           | 요소, 텍스트 | - 자식 노드를 모두 탐색하여 DOM 컬렉션 객체인 NodeList에 담아 반환함                        |
| Element.prototype.children          | 요소         | - 자식 노드 중에서 요소 노드만 모두 탐색하여 DOM 컬렉션 객체인 HTMLCollection에 담아 반환함 |
| Node.prototype.firstChild           | 요소, 텍스트 | - 첫 번째 자식 노드를 반환                                                                  |
| Node.prototype.lastChild            | 요소, 텍스트 | - 마지막 자식 노드를 반환                                                                   |
| Element.prototype.firstElementChild | 요소         | - 첫 번째 자식 노드를 반환                                                                  |
| Element.prototype.lastElementChild  | 요소         | - 마지막 자식 노드를 반환                                                                   |

**39.3.3 자식 노드 존재 확인**

-   자식 노드가 존재하는지 확인하려면 Node.prototype.hasChildNodes 메서드를 사용하며 boolean 타입으로 반환됨
-   텍스트 노드를 포함하여 자식 노드의 존재를 확인 함

```jsx
<!DOCTYPE html>
<html lang="en">
    <body>
        <ul id="fruits">
				</ul>
        <script>
            const fruits = document.getElementById("fruits");

						// 텍스트 노드를 포함하여 자식 노드의 존재를 확인함
            console.log(fruits.hasChildNodes()); // true

						// 자식 노드 중 텍스트 노드가 아닌 요소 노드가 존재하는지 확인함
						console.log(!!fruits.children.length); // false

						// 자식 노드 중 텍스트 노드가 아닌 요소 노드가 존재하는지 확인함
            console.log(!!fruits.childElementCount); // false
        </script>
    </body>
</html>
```

-   텍스트 노드가 아닌 요소 노드가 존재하는지 확인하려면 hasChildNodes 대신 children.length 또는 Element 인터페이스의 childElementCount 프로퍼티를 사용 해야 함

**39.3.4 요소 노드의 텍스트 노드 탐색**

-   텍스트 노드는 요소 노드의 자식 노드이며 따라서 텍스트 노드는 firstChild 프로퍼티로 접근할 수 있다
-   firstChild 프로퍼티는 첫 번째 자식 노드를 반환하며 텍스트 노드이거나 요소 노드이다

```jsx
<div id="foo">Hello</div>;
console.log(document.getElementById("foo").firstChild); // text
```

**39.3.5 부모 노드 탐색**

-   부모 노드를 탐색하려면 Node.prototype.parentNode 프로퍼티를 사용함
-   텍스트 노드는 DOM트리 최종단 노드인 리프 노드이므로 부모 노드가 텍스트 노드인 경우는 없음

```jsx
const banana = document.querySelector(".banana");
console.log(banana.parentNode); // ul#fruits
```

**39.3.6 형제 노드 탐색**

| 프로퍼티                                 | 반환 노드    | 설명                         |
| ---------------------------------------- | ------------ | ---------------------------- |
| Node.prototype.previousSibling           | 요소, 텍스트 | 자신의 이전 형제 노드를 반환 |
| Node.prototype.nextSibling               | 요소, 텍스트 | 자신의 다음 형제 노드를 반환 |
| Element.prototype.previousElementSibling | 요소         | 자신의 이전 형제 노드를 반환 |
| Element.prototype.nextElementSibling     | 요소         | 자신의 다음 형제 노드를 반환 |

**39.4 노드 정보 취득**

-   노드 객체에 대한 정보를 취득하려면 다음과 같은 노드 정보 프로퍼티를 사용 함

| 프로퍼티                | 설명                                               |
| ----------------------- | -------------------------------------------------- |
| Node.prototype.nodeType | 노드 객체의 종류, 노드 타입을 나타내는 상수를 반환 |

-   Node.ELEMENT_NODE : 요소 노드 타입을 나타내는 상수 1을 반환
-   Node.TEXT_NODE: 텍스트 노드 타입을 나타내는 상수 3을 반환
-   Node.DOCUMENT_NODE: 문서 노드 타입을 나타내는 상수 9을 반환 |
    | Node.prototype.nodeName | 노드의 이름을 문자열로 반환
-   요소 노드: 대문자 문자열로 태그 이름(’UL’, ‘LI’)을 반환
-   텍스트 노드: 문자열 “#text”를 반환
-   문서 노드: 문자열 “#document”를 반환 |

**39.5 요소 노드의 텍스트 조작**

**39.5.1 nodeValue**

-   Node.prototype.nodeValue 프로퍼티는 setter와 getter 모두 존재하는 접근자 프로퍼티로 참조와 할당 모두 가능함

```jsx
<!DOCTYPE html>
<html lang="en">
    <body>
        <div id="foo">Hello</div>
        <script>
						// 문서 노드의 nodeValue 프로퍼티를 참조한다
            console.log(document.nodeValue); // null

						// 요소 노드의 nodeValue 프로퍼티를 참조한다
            const foo = document.getElementById("foo");
            console.log(foo.nodeValue); // null

						// 텍스트 노드의 nodeValue 프로퍼티를 참조한다
            const textNode = foo.firstChild;
            console.log(textNode.nodeValue); // Hello

						// nodeValue 프로퍼티를 사용하여 텍스트 노드의 값을 변경함
            textNode.nodeValue = "world"
            console.log(textNode.nodeValue); // world
        </script>
    </body>
</html>
```

-   텍스트 노드의 nodeValue 프로퍼티를 참조할 때만 텍스트 노드의 값, 텍스트를 반환하며 값을 할당하면 텍스트 변경 가능
-   노드 객체의 nodeValue 프로퍼티를 참조하면 null을 반환함
-   텍스트를 변경하려면 다음과 같은 순서의 처리가 필요함
    1. 텍스트를 변경할 요소 노드를 취득한 뒤 텍스트 노드는 요소 노드의 자식 노드이므로 firstChild 프로퍼티를 사용하여 탐색
    2. 텍스트 노드의 nodeValue 프로퍼티를 사용하여 텍스트 노드의 값을 변경함

**39.5.2 textContent**

-   Node.prototype.textContent 프로퍼티는 setter와 getter 모두 존재하는 접근자 프로퍼티로 요소 도느의 텍스트와 모든 자손 노드의 텍스트를 모두 취득하거나 변경 함

```jsx
<div id="foo">
    Hello <span>world</span>
</div>;

// #foo 요소 노드의 텍스트를 모두 취득하며 html 마크업은 무시 됨
console.log(document.getElementById("foo").textContent); // Hello world
```

-   nodeValue 프로퍼티를 사용하면 textContent 프로퍼티를 사용 할때와 비교해서 코드가 복잡해 짐

```jsx
<div id="foo">Hello</div>;

console.log(foo.textContent === foo.firstchild.nodeValue); // true
```

-   innerText 프로퍼티는 다음과 같은 이유로 사용하지 않는 것이 좋음
    1. CSS에 순종적이라 비표시(visibility: hidden)로 지정된 요소 노드의 텍스트를 반환하지 않음
    2. CSS를 고려해야 하므로 textContent 프로퍼티보다 느림

**39.6 DOM조작**

-   새로운 노드를 생성하여 DOM에 추가하거나 기존 노드를 삭제 또는 교체하는 것을 말함
-   돔 조작에 의해 새로운 노드가 추가되거나 삭제되면 리플로우와 리페인트가 발생하는 원인이 되므로 성능에 영향을 주기 때문에 성능 최적화를 위해 주의해서 다루어야 함

**39.6.1 innerHTML**

-   Element.prototype.innerHTML 프로퍼티는 setter와 getter 모두 존재하는 접근자 프로퍼티로 요소 노드의 HTML 마크업을 취득하거나 변경함

```jsx
<div id="foo">
    Hello <span>world</span>
</div>;

// #foo 요소의 콘텐츠 영역 내의 HTML 마크업을 문자열로 취득함
console.log(document.getElementById("foo").innerHTML); // Hello <span>world</span>

// HTML 마크업이 파싱되어 요소 노드의 자식 노드로 돔에 반영됨
document.getElementById("foo").innerHTML = "Hi <span>there!</span>";
```

-   요소 노드의 innerHTML로 프로퍼티에 할당한 마크업 문자열은 요소 노드의 자식으로 돔에 반영됨
-   입력받은 데이터를 그대로 프로퍼티에 할당하는 것은 크로스 사이트 스크립팅 공격에 취약하므로 위험함

```jsx
<div id="foo">Hello</div>;

// innerHTML 프로퍼티로 스크립트 태그를 삽입하여 자바스크립트가 실행되도록 한다
// HTML5는 innerHTML 프로퍼티로 삽입된 script 요소 내의 자바스크립트 코드를 실행하지 않음
document.getElementById("foo").innerHTML = "<script>alert(document.cookie)</script>";

// 모던 브라우저에서 에러 이벤트를 강제로 발생시켜 자바스크립트 코드가 실행되도록 함
document.getElementById("foo").innerHTML = '<img src="x" onerror="alert(document.cookie)">';
```

-   innerHTML 프로퍼티는 복잡하지 않은 요소를 새롭게 추가할 때 유용하지만 기존 요소를 제거하지 않으면서 위치를 지정해 새로운 요소를 삽입해야 할 때는 사용하지 않는 것이 좋음

**39.6.2 insertAdjacentHTML**

-   Element.prototype.insertAdjacentHTML 메서드는 기존 요소를 제거하지 않으면서 위치를 지정해 새로운 요소를 삽입함
-   기존 요소에는 영향을 주지 않고 새롭게 삽입될 요소만 자식 요소로 추가하므로 innerHTML보다 효율적이고 빠름
-   HTML 마크업 문자열을 파싱하므로 크로스 사이트 스크립팅 공격에 취약하다는 점은 동일 함

```jsx
// beforebegin
<div id="foo">// afterbegin text // beforebeend</div>
// afterend
```

**39.6.3 노드 생성과 추가**

**요소 노드 생성**

-   Document.prototype.createElement(tagName) 메서드는 요소 노드를 생성하여 반환

```jsx
// 1. 요소 노드 생성
const li = document.createElement("li");

// 생성된 요소 노드는 아무런 자식 노드가 없음
console.log(li.childNodes); // NodeList []
```

-   createElement 메서드로 생성한 요소 노드는 기존 돔에 추가되지 않고 홀로 존재하는 상태로 생성된 요소 노드를 돔에 추가하는 처리가 필요함

**텍스트 노드 생성**

-   Document.prototype.createTextNode(text) 메서드는 텍스트 노드를 생성하여 반환

```jsx
// 2. 텍스트 노드 생성
const textNode = document.createTextNode("Banana");
```

-   텍스트 노드는 요소 노드의 자식이지만 createTextNode 메서드로 생성한 텍스트 노드는 요소 노드의 자식 노드로 추가되지 않고 홀로 존재하는 상태로 이후에 생성된 텍스트 노드를 요소 노드에 추가하는 처리가 필요함

![Untitled](39%E1%84%8C%E1%85%A1%E1%86%BC%20DOM%203442c56469394d078690184bdedaeb7b/Untitled%209.png)

**텍스트 노드를 요소 노도의 자식 노드로 추가**

-   Node.prototype.appendChild(childNode) 메서드는 매개변수 childNode에게 인수로 전달한 노드를 appendChild메서드를 호출한 노드의 마지막 자식 노드로 추가함
-   createTextNode 메서드로 생성한 텍스트 노드를 전달하면 appendChild메서드를 호출한 노드의 마지막 자식 노드로 텍스트 노드가 추가 됨

```jsx
// 3. 텍스트 노드를 li요소 노드의 자식 노드로 추가
li.appendChild(textNode);
```

![Untitled](39%E1%84%8C%E1%85%A1%E1%86%BC%20DOM%203442c56469394d078690184bdedaeb7b/Untitled%2010.png)

-   appendChild로 요소, 텍스트 노드는 부자 관계로 연결되었지만 돔에는 추가 되지 않은 상태임
-   자식 노드가 하나도 없는 경우에는 textContent 프로퍼티를 사용하는 편이 간편함

```jsx
// 텍스트 노드
li.appendChild(document.createTextNode("Banana"));

// textContent 위 코드와 동일하게 동작함
li.textContent = "Banana";
```

-   단, 요소 도느에 자식이 있는 경우 요소 노드의 모든 자식 노드가 제거되고 할당한 문자열이 추가되므로 주의해야 함

**요소 노드를 DOM에 추가**

-   Node.prototype.appendChild 메서드를 사용하여 텍스트 노드와 부자 관계로 연결한 요소 노드를 # fruits 요소 노드의 마지막 자식 요소로 추가 함

```jsx
// 4. li요소 노드를 #fruits(부모) 요소 노드의 마지막 자식 노드로 추가
fruits.appendChild(li);
```

-   이 과정을 거치면 새롭게 생성한 요소 노드가 돔에 추가 됨 ⇒ 이때, 리플로우와 리페인트가 실행 됨

**39.6.4 복수의 노드 생성과 추가**

```jsx
const fruits = document.getElementById("fruits");

["Apple", "Banana", "Orange"].forEach((text) => {
    // 1. 요소 노드 생성
    const li = document.createElement("li");

    // 2. 텍스트 노드 생성
    const textNode = document.createTextNode(text);

    // 3. 텍스트 노드를 li요소 노드의 자식 노드로 추가
    li.appendChild(textNode);

    // 4. li 요소 노드를 #fruits 요소 노드의 마지막 자식 노드로 추가
    fruits.appendChild(li);
});
```

-   3개의 요소 노드를 생성하여 돔에 3번 추가 하므로 돔이 3번 변경됨 ⇒ 이때, 리플로우와 리페인트가 3번 실행되므로 비효율적이다
-   컨테이너 요소 사용 ⇒ 컨테이너 요소를 미리 생성 후, 요소 노드를 컨테이너 요소에 자식 노드로 추가하고 컨테이너 요소를 #fruits 요소에 자식으로 추가하면 돔은 한번만 변경 됨

```jsx
const fruits = document.getElementById("fruits");

// 컨테이너 요소 노드 생성
const container = document
    .createElement("div")

    [("Apple", "Banana", "Orange")].forEach((text) => {
        // 1. 요소 노드 생성
        const li = document.createElement("li");

        // 2. 텍스트 노드 생성
        const textNode = document.createTextNode(text);

        // 3. 텍스트 노드를 li요소 노드의 자식 노드로 추가
        li.appendChild(textNode);

        // 4. li 요소 노드를 컨테이너 요소의 마지막 자식 노드로 추가
        container.appendChild(li);
    });

// 5. 컨테이너 요소 노드를 #fruits 요소 노드의 마지막 자식 노드로 추가
fruits.appendChild(container);
```

-   돔을 한번만 변경하므로 성능에 유리하지만 불필요한 컨테이너 요소(div)가 추가되는 부작용이 있음 ⇒ 바람직하지 않음
-   이러한 문제는 DocumentFragment 노드를 통해 해결 가능

![Untitled](39%E1%84%8C%E1%85%A1%E1%86%BC%20DOM%203442c56469394d078690184bdedaeb7b/Untitled%2011.png)

-   기존 돔과는 별도로 존재하므로 노드에 자식 노드를 추가하여도 기존 돔에는 어떠한 변경도 발생하지 않고 돔에 추가하면 자신은 제거되고 자신의 자식 노드만 돔에 추가 됨

```jsx
const fruits = document.getElementById("fruits");

// DocumentFragment 노드 생성
const fragment = document
    .createDocumentFragment()

    [("Apple", "Banana", "Orange")].forEach((text) => {
        // 1. 요소 노드 생성
        const li = document.createElement("li");

        // 2. 텍스트 노드 생성
        const textNode = document.createTextNode(text);

        // 3. 텍스트 노드를 li요소 노드의 자식 노드로 추가
        li.appendChild(textNode);

        // 4. li 요소 노드를 DocumentFragment 노드의 마지막 자식 노드로 추가
        fragment.appendChild(li);
    });

// 5. DocumentFragment 노드를 #fruits 요소 노드의 마지막 자식 노드로 추가
fruits.appendChild(fragment);
```

-   돔 변경이 발생하는 것은 한 번뿐이며 리플로우와 리페인트도 한 번만 실행 됨 ⇒ 여러 개의 요소 노드를 돔에 추가 할 경우에는 DocumentFragment 노드를 사용하는 것이 효율적

**39.6.5 노드 삽입**

**마지막 노드로 추가**

-   Node.prototype.appendChild 메서드는 인수로 전달받은 노드를 자신을 호출한 노드의 마지막 자식 노드로 돔에 추가 함

```jsx
<ul id="fruits">
    <li>Apple</li>
    <li>Banana</li>
</ul>;

// 요소 노드 생성
const li = document.createElement("li");

// 텍스트 노드를 li 요소 마지막 자식 노드로 추가
li.appendChild(document.createTextNode("Orange"));

// li 요소를 #fruits 요소 노드의 마지막 자식 노드로 추가
document.getElementById("fruits").appendChild(li);
```

![Untitled](39%E1%84%8C%E1%85%A1%E1%86%BC%20DOM%203442c56469394d078690184bdedaeb7b/Untitled%2012.png)

**지정한 위치에 노드 삽입**

-   Node.prototype.insertBefore(newNode, childNode) 메서드는 첫 번째 인수로 전달받은 노드를 두 번째 인수로 전달받은 노드 앞에 삽입 함
-   두 번째 인수로 전달받은 노드는 반드시 insertBefore 메서드를 호출한 노드의 자식 노드여야 함 그렇지 않으면 에러 발생

```jsx
const fruits = document.getElementById("fruits");

// 요소 노드 생성
const li = document.createElement("li");

// 텍스트 노드를 li 요소 마지막 자식 노드로 추가
li.appendChild(document.createTextNode("Orange"));

// li 요소 노드를 #fruits 요소 노드의 마지막 자식 요소 앞에 삽입
fruits.insertBefore(li, fruits.lastElementChild);
```

![Untitled](39%E1%84%8C%E1%85%A1%E1%86%BC%20DOM%203442c56469394d078690184bdedaeb7b/Untitled%2013.png)

```jsx
// null을 넣으면 마지막 자식 노드로 추가 됨
fruits.insertBefore(li, null);
```

![Untitled](39%E1%84%8C%E1%85%A1%E1%86%BC%20DOM%203442c56469394d078690184bdedaeb7b/Untitled%2012.png)

**39.6.6 노드 이동**

-   돔에 존재하는 노드를 appendChild 또는 inserBefore 메서드를 사용하여 돔에 추가하면 현재 위치에서 노드를 제거하고 새로운 위치에 노드가 추가 됨

```jsx
<ul id="fruits">
    <li>Apple</li>
    <li>Banana</li>
    <li>Orange</li>
</ul>;

const fruits = document.getElementById("fruits");

// 이미 존재하는 요소 노드를 취득
const [apple, banana] = fruits.children;

// 이미 존재하는 apple 요소 노드를 fruits 요소 노드의 마지막 노드로 이동
fruits.appendChild(apple); // Banana - Orange - Apple

// 이미 존재하는 banana 요소 노드를 fruits 요소의 마지막 자식 노드 앞으로 이동
fruits.insertBefore(banana, fruits.lastElementChild); // Orange - Banana - Apple
```

**39.6.7 노드 복사**

-   Node.prototype.cloneNode 메서드는 노드의 사본을 생성하여 반환함
-   매개변수 deep에 true를 인수로 전달하면 노드를 깊은 복사하여 모든 자손 노드가 포함된 사본을 생성함
-   false를 인수로 전달하거나 생략하면 얕은 복사가 되어 노드 자신만의 사본을 생성함, 자손노드도 복사되지 않으므려 텍스트 노드도 없음

```jsx
<ul id="fruits">
    <li>Apple</li>
</ul>;

const fruits = document.getElementById("fruits");
const apple = fruits.firstElementChild;

// apple 요소를 얕은 복사하여 사본을 생성, 텍스트 노드가 없는 사본이 생성 됨
const shallowClone = apple.cloneNode();

// 사본에 텍스트 추가
shallowClone.textContent = "Banana";

// 사본 요소 노드를 #fruits 요소 노드의 마지막 노드로 추가
fruits.appendChild(shallowClone);

// #fruits를 깊은 복사하여 모든 자손 노그다 포함된 사본을 생성
const deepClone = fruits.cloneNode(true);

// 사본 요소 노드를 #fruits 요소 노드의 마지막 노드로 추가
fruits.appendChild(deepClone);
```

![Untitled](39%E1%84%8C%E1%85%A1%E1%86%BC%20DOM%203442c56469394d078690184bdedaeb7b/Untitled%2014.png)

**39.6.8 노드 교체**

-   Node.prototype.replaceChild(newChild, oldChild) 메서드는 자신을 호출한 노드의 자식 노드를 다른 노드로 교체 함
-   첫 번째 매개변수 newChild에는 교체할 새로운 노드를 인수로 전달하고, 두 번째 매개변수 oldChild에는 이미 존재하는 교체 될 노드를 인수로 전달 함
-   replaceChild 메서드는 자신을 호출한 노드의 자식 노드인 oldChild노드를 newChild 노드로 교체 함 ⇒ 이때, oldChild 노드는 돔에서 제거 됨

```jsx
<ul id="fruits">
    <li>Apple</li>
</ul>;

const fruits = document.getElementById("fruits");

// 기존 노드와 교체할 요소 노드를 생성
const newChild = document.createElement("li");
newChild.textContent = "Banana";

// #fruits 요소 노드의 첫 번째 자식 요소 노드를 newChild 요소 노드로 교체
fruits.replaceChild(newChild, fruits.firstElementChild);
```

**39.6.9 노드 삭제**

-   Node.prototype.removeChild(child)메서드는 child 매개변수에 인수로 전달한 노드를 돔에서 삭제 함

```jsx
// #fruits 요소 노드의 마지막 요소를 돔에서 삭제
fruits.removeChild(fruits.lastElementChild);
```

**39.7 어트리뷰트**

**39.7.2 어트리뷰트 노드와 attributes 프로퍼티**

-   html 요소는 여러 개의 어트리뷰트(속성)을 가질 수 있다
-   글로벌 어트리뷰트(id, class, style, title, lang 등)
-   이벤트 핸들러 어트리뷰트(onclick, onchange, onfocus, onblur 등)
-   id, class, style 어트리뷰트는 모든 html 요소에 사용할 수 있음
-   type, value, checked는 input 요소에만 사용할 수 있음
-   요소 노드의 모든 어트리뷰트 노드는 getter만 존재하는 읽기 전용 접근자 프로퍼티이며 NamedNodeMap 객체를 반환 함

![Untitled](39%E1%84%8C%E1%85%A1%E1%86%BC%20DOM%203442c56469394d078690184bdedaeb7b/Untitled%2015.png)

**39.7.2 HTML 어트리뷰트 조작**

-   어트리뷰트 값을 참조 하려면 Element.prototype.getAttribute 메서드를 사용하고,
-   값을 변경하려면 setAttribute 메서드를 사용 함
-   어트리뷰트가 존재하는 확인하려면 Element.prototype.hasAttribute메서드를 사용하고
-   삭제하려면 removeAttribute메서드를 사용 함

**39.7.3 HTML 어트리뷰트 VS. DOM 프로퍼티**

-   요소 노드 객체에는 html 어트리뷰트에 대응하는 프로퍼티가 존재하며 초기값으로 가지고 있다
-   html 어트리뷰트의 역할은 요소의 초기 상태를 의미하며 이는 변하지 않음
-   첫 랜더링 이후 사용자가 요소에 무언가를 입력하면 상황이 달라지는데 이처럼 요소는 2개의 상태 초기 상태와 최신 상태를 관리 해야 한다
-   요소 노드의 초기 상태는 어트리뷰트 노드가 관리하며 요소 노드의 최신상태는 돔 프로퍼티가 관리한다

**어트리뷰트 노드**

-   html에서 어트리뷰트로 지정한 요소의 초기 상태는 어트리뷰트 노드에서 관리 한다
-   이 값은 사용자의 입려겡 의해 상태가 변경되지 않고 요소의 초기 상태를 그대로 유지 한다
-   getAttribute 메서드로 취득한 값은 html요소의 어트리뷰트 초기 값으로 사용자의 입력에 의해 변하지 않으므로 결과는 언제나 동일 함

```jsx
// getAttribute 프로퍼티에 지정된 어트리뷰트 값을 취득함 결과는 언제나 동일
document.getElementById("user").getAttribute("value"); // yesol
```

-   setAttribute 메서드는 어트리뷰트 노드에서 관리하는 html 요소에 지정한 어트리뷰트 값을 변경한다

```jsx
document.getElementById("user").setAttribute("value", "foo"); // yesol
```

**DOM 프로퍼티**

-   사용자가 입력한 최신 상태는 html 어트리뷰트에 대응하는 요소 노드의 돔 프로퍼티가 관리하며 사용자의 입력에 의한 상태 변화에 반응하여 언제나 최신 상태를 유지한다
-   최신 상태 값은 사용자의 입력에 의해 언제든지 동적으로 변경되지만 getAttribute 메서드로 취득한 html 어트리뷰드 값, 초기값은 변하지 않고 유지 됨

**HTML 어트리뷰트와 DOM 프로퍼티의 대응 관계**

-   대부분의 html 어트리뷰트는 이름과 동일한 돔 프로퍼티와 1:1로 대응하는데 언제나 그런 것은 아님
    -   id : 어트리뷰트와 id프로퍼티는 대응하며 동일한 값으로 연동 함
    -   input : value 어트리뷰트는 1:1로 대응하지만 어트리뷰트는 초기 상태를, 프로퍼티는 최신 상태를 갖음
    -   class : className, classList 프로퍼티와 대응 함
    -   for: htmlFor 프로퍼티와 1:1 대응 함
    -   td : 요소의 colspan 어트리뷰트는 대응하는 프로퍼티가 존재하지 않음
    -   textContent : 프로퍼티는 대응하는 어트리뷰트가 존재하지 않음
    -   어트리뷰트는 대소문자를 구별하지 않지만 대응하는 프로퍼티는 카멜 케이스를 따름

**DOM 프로퍼티 값의 타입**

-   돔 프로퍼티로 취득한 최신 상태 값은 문자열이 아닐 수 있다
    ex) checked 어트리뷰트는 값은 문자열이지만 프로퍼티값은 불리언 타입 임

**39.7.4 data 어트리뷰트와 dataset 프로퍼티**

-   data 어트리뷰트와 dataset 프로퍼티를 사용하면 요소에 정의한 어트리뷰트와 자바스크립트 간에 데이터를 교환할 수 있다
-   data-uer-id, data-role과 같이 data- 접두사 다음에 임의의 이름을 붙여 사용 함

```jsx
<li id="1" data-user-id="1111" data-role="admin">Lee</li>
<li id="2" data-user-id="2222" data-role="subscriber">Kim</li>

const users = [...document.querySelector('.users').children];

// user-id가 1111인 요소 노드를 취득
const user = users.find(user => user.dataset.userId === '1111')
// user-id가 1111인 요소 노드에서 data-role 값을 취득함
console.log(user.dataset.role) // admin

// user-id가 1111인 요소의 data-role 값을 변경함
user.dataset.role = 'subscriber'

console.log(user.dataset) // DOMStringMap {userId: "1111", role: "subscriber"}
```

**39.8 스타일**

**39.8.1 인라인 스타일 조작**

-   HTMLElement.prototype.style 프로퍼티는 setter와 getter 모두 존재하는 접근자 프로퍼티로서 요소 노드의 인라인 스타일을 취득하거나 추가 또는 변경 한다

```jsx
<div style="color: red">Hello world</div>;

// 인라인 스타일 취득
const div = document.querySelector("div");

// 인라인 스타일 변경
div.style.color = "blue";
div.style.background = "yellow";
div.style.width = "100px";
div.style.height = "100px";
```

![Untitled](39%E1%84%8C%E1%85%A1%E1%86%BC%20DOM%203442c56469394d078690184bdedaeb7b/Untitled%2016.png)

**39.8.2 클래스 조작**

-   .으로 시작하는 클래스 선택자를 사용하여 CSS class를 미리 정의한 다음 html 요소의 어트리뷰트를 값을 변경하여 요소의 스타일을 변걍 할 수 있음

**className**

-   Element.prototype.className 프로퍼티는 setter와 getter 모두 존재하는 접근자 프로퍼티로서 값을 취득하거나 변경 함
-   className 프로퍼티를 참조하면 값을 문자열로 반환하고 문자열을 할당하면 class 어트리뷰트 값을 할당한 문자열로 변경 함
-   문자열을 반환하므로 공백으로 구분된 여러 개의 클래스를 반환하는 경우 다루기가 불편함

```jsx
<div class="box red">Hello world</div>;

const box = document.querySelector(".box");
console.log(box.className); // 'box red'
```

**classList**

-   Element.prototype.classList 프로퍼티는 class 어트리뷰트의 정보를 담은 DOMTokenList 객체를 반환 함

```jsx
<div class="box red">Hello world</div>;

const box = document.querySelector(".box");

// .box 요소의 class 어트리뷰트 정보를 담은 DOMTokenList 객체를 취득
console.log(box.classList); // DOMTokenList(2) ['box', 'red', value: 'box red']

// .box 요소의 class 어트리뷰트 값 중에서 'red'만 'blue'로 변경
box.classList.replace("red", "blue");
```

-   add(…className) : 인수로 전달한 1개 이상의 문자열을 class 어트리뷰트 값으로 추가 함

```jsx
box.classList.add("foo", "baz");
```

-   remove(…className) : 인수로 전달한 1개 이상의 문자열과 일치하는 클래스를 class 어트리뷰트에서 삭제 함

```jsx
box.classList.remove("foo", "baz");
```

-   item(index) : 인수로 전달한 index에 해당하는 클래스를 class 어트리뷰트에서 반환한다

```jsx
box.classList.item(0); // 'box'
```

-   container(className) : 인수로 전달한 클래스가 포함되어 있는지 확인 함

```jsx
box.classList.container("box"); // true
box.classList.container("x"); // false
```

-   replace(oldClassName, newClassName) : 첫 번째 인수로 전달한 문자열을 두 번째 인수로 전달한 문자열로 변경

```jsx
box.classList.replace("red", "blue"); // class="box blue"
```

-   toggle(className[.force]) : 인수로 전달한 문자열과 일치하는 클래스가 존재하면 제거하고 존재하지 않으면 추가 함

```jsx
box.classList.toggle('foo') // class="box blue foo"
box.classList.toggle('foo') // class="box blue"

// true 강제로 클래스 추가, false 강제로 클래스 제거
box.classList.toggle('foo', true) // class="box blue foo"
box.classList.toggle('foo' false) // class="box blue"
```

**38.8.3 요소에 적용되어 있는 CSS 스타일 참조**

-   style 프로퍼티는 인라인 스타일만 반환함, 따라서 클래스를 적용할 스타일이나 상속을 통해 암묵적으로 적용된 스타일은 style 프로퍼티로 참조 할 수 없으며 참조 해야 할 경우 getComputedStyle 메서드를 사용 한다

```jsx
<div class="box">Box</div>;

const box = document.querySelector(".box");

// .box 요소에 적용된 모든 css 스타일을 담고 있는 CSSStyleDeclaration 객체를 취득
const computedStyle = window.getComputedStyle(box);
console.log(computedStyle); // CSSStyleDeclaration

// 임베딩 스타일
console.log(computedStyle.width);
console.log(computedStyle.border);
```

-   getComputedStyle 메서드의 두 번째 인수로 :after, :before와 같은 의사 요소를 지정하는 문자열을 전달할 수 있음

```jsx
<!DOCTYPE html>
<html lang="en">
    <head>
        <style>
            .box:before {
                content: "Hello";
            }
        </style>
    </head>
    <body>
        <div class="box">Box</div>
    </body>
    <script>
        const box = document.querySelector(".box");

				// 의사 요소 :before의 스타일을 취득한다
        const computedStyle = window.getComputedStyle(box, ":before");
        console.log(computedStyle.content); // "Hello"
    </script>
</html>
```

**39.9 DOM 표준**

-   2018년 4월부터 구글, 애플, 마이크로소프트, 모질라로 구성된, 4개의 주류 브라우저 벤더사가 주도하는 WHATWG이 단일 표준을 내놓기로 합의 함
