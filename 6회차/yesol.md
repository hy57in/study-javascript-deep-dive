# 24장 클로저

Created: November 5, 2022 9:36 PM

```jsx
const x = 1;

function outerFunc() {
    // innerFunc의 상위 스코프
    const x = 10;
    function innerFunc() {
        // 중첩 함수
        console.log(x); // 10
    }
    innerFunc();
}

outerFunc();
```

-   outerFunc 함수 내부에서 중첩 함수 innerFunc가 정의 및 호출
-   이때 중첩 함수 innerFunc의 상위 스코프는 외부 함수 outerFunc의 스코프다
-   따라서 중첩 함수 innerFunc 내부에서 자신을 포함하고 있는 외부 함수 outerFunc의 x 변수에 접근 할 수 있다

```jsx
const x = 1;

function outerFunc() {
    const x = 10;
    innerFunc();
}

function innerFunc() {
    console.log(x); // 1
}

outerFunc();
```

-   innerFunc 함수가 outerFunc 함수의 내부에서 정의된 중첩 함수가 아니라면 innerFunc 함수를 outerFunc 함수의 내부에서 호출된다 하더라도 outerFunc 함수의 변수에 접근 할 수 없다

⇒ 이 같은 현상이 발생하는 이유는 자바스크립트가 렉시컬 스코프를 따르는 프로그래밍 언어이기 때문

**24.1 렉시컬 스코프**

-   자바스크립트 엔진은 함수를 어디서 호출했는지가 아니라 **함수를 어디에 정의**했는지에 따라 상위 스코프를 결정함
    ⇒ 이를 렉시컬 스코프(정적 스코프)라 함

```jsx
const x = 1;

function foo() {
    const x = 10;
    bar();
}

function bar() {
    console.log(x);
}

foo(); // 1
bar(); // 1
```

-   foo와 bar 함수는 모두 전역에서 정의된 전역 함수
-   함수의 상위 스코프는 함수를 어디에서 정의했느냐에 따라 결정되므로 두 함수의 상위 스코프는 전역임
-   함수를 어디에서 호출했는지는 함수의 상위 스코프 결정에 어떠한 영향도 주지 못함

⇒ 함수의 상위 스코프는 함수를 정의한 위치에 의해 정적으로 결정되고 변하지 않음

-   스코프의 실체는 실행 컨텍스트의 렉시컬 환경이고 이 렉시컬 환경은 자신의 “외부 렉시컬 환경에 대한 참조”를 통해 상위 렉시컬 환경과 연결되는데 이것이 스코프 체인이다

⇒ 렉시컬 환경의 “외부 렉시컬 환경에 대한 참조”에 저장할 참조값, 즉 상위 스코프에 대한 참조는 함수 정의가 평가되는 시점에 함수가 정의된 환경(위치)에 의해 결정됨 ⇒ 이것이 렉시컬 스코프 임

**24.2 함수 객체의 내부 슬롯 [[Enviroment]]**

-   함수는 자신의 내부 슬롯 [[Enviroment]]에 자신이 정의된 환경, 즉 상위 스코프의 참조를 저장함
-   함수 객체의 내부슬롯 [[Enviroment]]에 저장된 현재 실행 중인 실행 컨텍스트의 렉시컬 환경의 참조가 바로 상위 스코프이며 자신이 호출되었을 때 생성될 함수 렉시컬 환경의 “외부 렉시컬 환경에 대한 참조”에 저장 될 참조값이다
-   함수 객체는 내부슬롯 [[Enviroment]]에 저장한 렉시컬 환경의 참조, 즉 상위 스코프를 자신이 존재하는 한 기억 한다

```jsx
const x = 1;

function foo() {
    const x = 10;

    // 상위 스코프는 함수 정의 환경에 따라 결정된다
    // 함수 호출 위치와 상위 스코프는 아무런 관계가 없다
    bar();
}

function bar() {
    console.log(x);
}
```

-   두 함수는 모두 전역에서 함수 선언문으로 정의되었기 때문에 전역 코드가 평가되는 시점에 평가되어 함수 객체를 생성하고 전역 객체 window의 메서드가 된다
-   이때 생성된 함수 객체의 내부 슬롯 [[Enviroment]]에는 함수 정의가 평가된 시점, 즉 전역 코드 평가 시점에 실행중인 실행 컨텍스트의 렉시컬 환경인 전역 렉시컬 환경의 참조가 저장된다
-   함수가 호출되면 함수 내부로 코드의 제어권이 이동하고 함수 코드를 평가하기 시작한다
-   함수 코드 평가 진행 순서

1. 함수 실행 컨텍스트 생성
2. 함수 렉시컬 환경 생성

    2.1 함수 환경 레코드 생성

    2.2 this바인딩

    2.3 외부 렉시컬 환경에 대한 참조 결정

-   이때 함수 렉시컬 환경의 구성 요소인 외부 렉시컬 환경에 대한 참조에는 함수 객체의 내부 슬롯 [[Enviroment]]에 저장된 렉시컬 환경의 참조가 할당된다 ⇒ 즉 함수 객체의 내부 슬롯에 [[Enviroment]]에 저장된 렉시컬 환경의 참조는 바로 함수의 상위 스코프를 의미하며 이것이 바로 함수 정의 위치에 따라 상위 스코프를 결정하는 렉시컬 스코프의 실체 이다

**24.3 클로저와 렉시컬 환경**

```jsx
const x = 1;

// 1.
function outer() {
    const x = 10;
    const inner = function () {
        console.log(x);
    }; // 2.
    return inner;
}

// outer 함수를 호출하면 중첩 함수 inner를 반환한다
// outer 함수의 실행 컨텍스트는 실행 컨텍스트 스택에서 팝되어 제거됨
const innerFunc = outer(); // 3.
innerFunc(); // 4. 10
```

-   outer 함수를 호출(3)하며 outer 함수는 중첩 함수 inner를 반환하고 생명 주기를 마감함 ⇒ outer 함수의 호출이 종료되면 실행 컨텍스트는 스택에서 제거 됨 ⇒ 이때, outer함수의 지역 변수 x와 변수 값 10을 저장하고 있던 outer 함수의 실행 컨텍스트가 제거 되었으므로 outer 함수의 지역 변수 x또한 생명 주기를 마감함
-   따라서 outer 함수의 지역 변수 x는 더는 유효하지 않게되어 x변수에 접근할 수 있는 방법은 없어 보이지만 코드의 실행결과(4)는 outer 함수의 지역 변수 x의 값인 10이다

⇒ 이처럼 외부 함수보다 중첩 함수가 더 오래 유지되는 경우 중첩 함수는 이미 생명 주기가 종료한 외부 함수의 변수를 참조 할 수 있는데 이러한 중첩 함수를 클로저라고 부른다

-   자바스크립트의 모든 함수는 자신의 상위 스코프를 기억하며 함수를 어디서 호출하든 상관없이 유지된다 ⇒ 따라서 함수를 어디서 호출하든 상관없이 언제나 자신이 기억하는 상위 스코프의 식별자를 참조할 수 있으며 바인딩 된 값을 변경 할 수도 있다
-   위 예제에서 inner 함수는 자신이 평가될 때 자신이 정의된 위치에 의해 결정된 상위 스코프를 [[Enviroment]] 내부 슬롯에 저장하는데 이때 저장된 상위 스코프는 함수가 존재하는 한 유지된다

1. 위 예제에서 outer 함수가 평가되어 함수 객체를 생성할 때(1) 현재 실행 중인 실행 컨텍스트의 렉시컬 환경, 즉 전역 렉시컬 환경을 outer 함수 객체의 [[Enviroment]] 내부 슬롯에 상위 스코프로서 저장한다
2. outer 함수를 호출하면 outer 함수의 렉시컬 환경이 생성되고 앞서 outer 함수 객체의 [[Enviroment]] 내부 슬롯에 저장된 전역 렉시컬 환경을 outer 함수 렉시컬 환경의 “외부 렉시컬 환경에 대한 참조”에 할당 한다
3. 그후 중첩 함수 inner가 평가되는데(2) 이때 중첩 함수 inner는 자신의 [[Enviroment]] 내부 슬롯에 현재 실행중인 실행 컨텍스트의 렉시컬 환경 즉, outer 함수의 렉시컬 환경을 상위 스코프로서 저장한다
4. outer 함수의 실행이 종료되면 inner 함수를 반환하면서 outer함수의 생명주기가 종료된다(3) 즉, outer 함수의 실행 컨텍스트가 스택에서 제거되는데 이때 outer 함수의 실행 컨텍스트는 제거되지만 outer 함수의 렉시컬 환경까지 소멸하는 것인 아니다

⇒ outer 함수의 렉시컬 환경은 inner 함수의 [[Enviroment]] 내부 슬롯에 의해 참조되고 있고 inner 함수는 전역 변수 innerFunc에 의해 참조되고 있으므로 가비지 컬렉션의 대상이 되지 않기 때문이다 ⇒ 가비지 컬렉터는 누군가가 참조하고 있는 메모리 공간을 함부로 해제 하지 않음

1. outer 함수가 반환한 innner 함수를 호출(4)하면 inner 함수의 실행 컨텍스트가 생성되고 스택에 푸시 된다 그리고 렉시컬 환경의 외부 렉시컬 환경에 대한 참조에는 inner 함수 객체의 [[Enviroment]] 내부 스롯에 저장되어 있는 참조값이 할당 된다

⇒ 중첩 함수 inner 는 외부함수 outer 보다 더 오래 생존 했다 이때 함수의 생존여부와 상관없이 자신이 정의된 위치에 의해 결정된 상위 스코프를 기억한다 이처럼 중첩함수 inner 내부에서는 상위 스코프를 참조 할 수 있으므로 상위 스코프의 식별자를 참조할 수 있고 값을 변경할 수 도 있다

⇒ 자바스크립트의 모든 함수는 상위 스코프를 기억하므로 모든 함수는 클로저이지만 일반적으로 모든 함수를 클로저라고 하지는 않음

```jsx
<!DOCTYPE html>
<html lang="en">
<body>
    <script>
        function foo(){
            const x=1
            const y=2

            //일반적으로 클로저라고 하지 않는다
            function bar(){
                const z = 3;

                debugger;
                // 상위 스코프의 식별자를 참조하지 않는다
                console.log(z)
            }
            return bar;
        }
        const bar = foo();
        bar();
    </script>
</body>
</html>
```

![Untitled](24%E1%84%8C%E1%85%A1%E1%86%BC%20%E1%84%8F%E1%85%B3%E1%86%AF%E1%84%85%E1%85%A9%E1%84%8C%E1%85%A5%20127c2c61dede4a76a19c0da487a8fe1d/Untitled.png)

-   위 예제의 중첩 함수 bar는 외부 함수 foo보다 더 오래 유지되지만 상위 스코프의 어떤 식별자도 참조하지 않음

⇒ 이처럼 상위 스코프의 어떤 식별자도 참조하지 않는 경우 대부분의 모든 브라우저는 최적화를 통해 다음 그림과 같이 상위 스코프를 기억하지 않는다 왜냐? 참조하지도 않는 식별자를 기억하는 것은 메모리 낭비이기 때문 ⇒ 따라서 bar 함수는 클로저라 할 수 없음

```jsx
<!DOCTYPE html>
<html lang="en">
<body>
    <script>
      function foo(){
        const x = 1;

        // bar 함수는 클로저였지만 곧바로 소멸
        // 이러한 함수는 일반적으로 클로저라고 하지 않음
        function bar(){
            debugger;
            // 상위 스코프의 식별자를 참조함
            console.log(x);
        }
        bar()
      }
      foo()
    </script>

</body>
</html>
```

![Untitled](24%E1%84%8C%E1%85%A1%E1%86%BC%20%E1%84%8F%E1%85%B3%E1%86%AF%E1%84%85%E1%85%A9%E1%84%8C%E1%85%A5%20127c2c61dede4a76a19c0da487a8fe1d/Untitled%201.png)

-   위 예제의 중첩 함수 bar는 상위 스코프의 식별자를 참조하고 있으므로 클로저이며 외부 함수의 외부로 반환되어 외부 함수보다 더 오래 살아 남는다 ⇒ 이처럼 외부 함수보다 중첩 함수가 더 오래 유지되는 경우 중첩 함수는 이미 생명 주기가 종료한 외부 함수의 변수를 참조할 수 있는데 이것을 클로저라고 부른다
-   클로저는 중첩 함수가 상위 스코프의 식별자를 참조하고 있고 중첩 함수가 외부 함수보다 더 오래 유지되는 경우에 한정하는 것이 일반적
-   클로저에 의해 참조되는 상위 스코프의 변수(위 예제에서 foo의 x)를 자유변수라고 부르는데 클로저란 함수가 자유 변수에 대해 닫혀있다 라는 의미이다 ⇒ 자유변수에 묶여있는 함수라 할 수 있음

**24.4 클로저의 활용**

-   클로저는 상태를 안전하게 변경하고 유지하기 위해 사용한다 ⇒ 상태가 의도치 않게 변경되지 않도록 상태를 안전하게 은닉하고 특정함수에게만 상태 변경을 허용하기 위해 사용된다

```jsx
//카운트 상태 변수
let num = 0;

//카운트 상태 변경 함수
const increase = function () {
		//카운트 상태를 1만큼 증가 시킨다
		return ++num;
};

console.log(increase()); // 1
console.log(increase()); // 2
console.log(increase()); // 3

// 전역변수를 지역변수로 바꾸어 상태 변경을 방지하자
======================================================

const increase = function () {
		// 카운트 상태 변수
		let num = 0;

		// 카운트 상태를 1만큼 증가 시킨다
		return ++num
}

// 이전 상태를 유지하지 못한다
console.log(increase()); // 1
console.log(increase()); // 1
console.log(increase()); // 1

// 이전 상태를 유지할 수 있도록 클로저를 사용해 보자
========================================================

// 카운트 상태 변경 함수
const increase = (function () {
		//카운트 상태 변수
		let num = 0;

		// 클로저
		return function () {
		// 카운트 상태를 1만큼 증가 시킨다
		return ++num;
		}
}());

console.log(increase()); // 1
console.log(increase()); // 2
console.log(increase()); // 3
```

-   코드는 잘 동작하지만 오류를 발생시킬 가능성을 내포하고 있는 좋지 않은 코드이다 ⇒ 위 예제가 바르게 동작하려면 다음의 전제 조건이 있어야 하기 때문임

1. 카운드 상태(num 변수의 값)는 increase 함수가 호출되기 전까지 변경되지 않고 유지되어야 한다
2. 이를 위해 카운드 상태(num 변수의 값)는 increase 함수만이 변경할 수 있어야 한다

-   하지만 카운트 상태는 전역 변수를 통해 관리되어 누구든 접근과 변경할 수 있으며 이는 오류로 이어진다

-   카운트 상태를 안전하게 변경하고 유지하기 위해 전역 변수 num을 increse 함수의 지역 변수로 변경하여 의도치 않은 상태 변경을 방지 ⇒ 이제 num 변수의 상태는 increase 함수만이 변경 할 수 있다
-   하지만 increase 함수가 호출될 때마다 지역 변수 num은 다시 선언되고 0으로 초기화되기 때문에 출력 결과는 언제나 1이된다 ⇒ 상태가 변경되기 이전 상태를 유지하지 못함

-   즉시 실행 함수가 호출되고 즉시 실행 함수가 반환한 함수가 increase 변수에 할당된다
-   increase 변수에 할당된 함수는 자신이 정의된 위치에 의해 결정된 상위 스코프인 즉시 실행 함수의 렉시컬 환경을 기억하는 클로저다
-   즉시 실행 함수는 호출된 이후 소멸되지만 즉시 실행 함수가 반환한 클로저는 increase 변수에 할당되어 호출된다
-   이때 즉시 실행 함수가 반환한 클로저는 자신이 정의된 위치에 의해 결정된 상위 스코프인 즉시 실행 함수의 렉시컬 환경을 기억하고 있다 ⇒ 따라서 즉시 실행 함수가 반환한 클로저는 카운트 상태를 유지하기 위한 자유 변수 num을 어디에서 호출하든지 참조하고 변경 할 수 있다
-   즉시실행 함수는 한 번만 실행되므로 increase가 호출될 때마다 num 변수가 재차 초기화될 되지 않으며 num 변수는 외부에서 직접 접근할 수 없는 변수이므로 안정적인 프로그래밍이 가능함
-   이처럼 클로저는 상태가 의도치 않게 변경되지 않도록 안전하게 은닉하고 특정 함수에게만 상태 변경을 허용하여 상태를 안전하게 변경하고 유지하기 위해 사용한다

```jsx
const counter = (function (){
		// 카운트 상태 변수
		let num = 0;

		// 클로저인 메서드를 갖는 객체를 반환한다
		// 객체 리터럴은 스코프를 만들지 않는다
		// 아래 메서드들의 상위 스코프는 즉시 실행 함수의 렉시컬 환경임
		return {
		// num: 0
		// 프로퍼티는 public하므로 은닉되지 않는다
				increase() {
						return ++num;
				},
				decrease() {
						return num > 0 ? --num : 0;
				}
		};
}());

console.log(counter.increase()); // 1
console.log(counter.increase()); // 2

console.log(counter.decrease()); // 1
console.log(counter.decrease()); // 0

//생성자 함수로 표현하면 다음과 같다
========================================================

const counter = (function (){
		// 1. 카운트 상태 변수
		let num = 0;

		function Counter() {
				//this.num = 0; // 2. 프로퍼티는 public하므로 은닉되지 않음
		}

		Counter.prototype.increase = function () {
				return ++num;
		}

		Counter.prototype.decrease = function () {
				return num > 0 ? --num : 0;
		}
		return Counter;
}())

const counter = new Counter();

console.log(counter.increase()); // 1
console.log(counter.increase()); // 2

console.log(counter.decrease()); // 1
console.log(counter.decrease()); // 0
```

-   즉시 실행 함수가 반환하는 객체 리터럴은 즉시 실행 함수의 실행 단계에서 평가되어 객체가 되며 객체의 메서드도 함수 객체로 생성된다
-   객체 리터럴의 중괄호는 코드 블록이 아니므로 별도의 스코프를 생성하지 않는다
-   increase, decrease 메서드의 상위 스코프는 메서드가 평가되는 시점에 실행중인 실행 컨텍스트인 즉시 실행 함수 실행 컨텍스트의 렉시컬 환경이며 어디서 호출되든 상관없이 즉시 실행 함수의 스코프의 식별자를 참조할 수 있다 =

-   1은 생성자 함수 Conuter가 생성할 인스턴스의 프로퍼티가 아니라 즉시 실행 함수 내에서 선언된 변수다
-   즉시 샐항 함수 내에서 선언된 num 변수는 인스턴스를 통해 접근 할 수 없으며 즉시 샐항 함수 외부에서도 접근 할 수 없는 은닉된 변수다
-   생성자 함수 Counter는 프로토타입을 통해 두 메서드를 상속받은 인스턴스를 생성한다
-   두 메서드는 모두 자신의 함수 정의가 평가되어 함수 객체가 될 때 실행 중인 실행 컨텍스트인 즉시 실행 함수 실행 컨텍스트의 렉시컬 환경을 기억하는 클로저다 ⇒ 따라서 프로토타입을 통해 상속되는 프로토타입 메서드일지라도 즉시 실행 함수의 자유 변수 num을 참조할 수 있다 ⇒ 다시 말해 num 변수의 값은 두 메서드만이 변경할 수 있다

⇒ 변수값은 변경될 수 있어 오류 발생의 근본적 원인이 되는데 상태 변경이나 가변 데이터를 피하고 불변성을 지향하는 함수형 프로그래밍에서 부수효과를 최대한 억제하여 오류를 피하고 프로그램의 안정성을 높이기 위해 클로저는 적극적으로 사용해야 한다

-   함수형 프로그래밍에서 클로저를 활용하는 예제

```jsx
// 함수를 인수로 전달받고 함수를 반환하는 고차 함수
// 이 함수는 카운트 상태를 유지하기 위한 자유 변수 counter를 기억하는 클로저를 반환한다
function makeCounter(predicate) {
		// 카운드 상태를 유지하는 위한 자유 변수
		let counter = 0;

		//클로저를 반환
		return function () {
				//인수로 전달받은 보조 함수에 상태 변경을 위임한다
				counter = predicate(counter);
				return counter;
		};
}

// 보조함수
function increase(n) {
		return ++n;
}

// 보조함수
function decrease(n) {
		return --n;
}

// 함수로 함수를 생성한다
// makeCounter 함수는 보조 함수를 인수로 전달받아 함수를 반환한다
const increase = makeCounter(increase); // 1.
console.log(increaser()); // 1
console.log(increaser()); // 2

// increaser 함수와는 별개의 독립된 렉시컬 환경을 갖기 때문에 카운터 상태가 연동하지 않는다
const increase = makeCounter(decrease); // 2.
console.log(decreaser()); // -1
console.log(decreaser()); // -2

// 자유변수 counter를 공유하여 증감이 가능한 카운터를 만들려면
// 렉시컬 환경을 공유하는 클로저를 만들어야 함
// 이를 위해서는 makeCounter 함수를 두번 호출하지 말아야 함
======================================================

// 함수를 반환하는 고차 함수
// 이 함수는 카운트 상태를 유지하기 위한 자유 변수 counter를 기억하는 클로저를 반환한다

const counter = (function(){
		// 카운트 상태를 유지하기 위한 자유 변수
		let counter = 0;
		// 함수를 인수로 전달받는 클로저를 반환
		return function (predicate) {
				counter = predicate(counter);
				return counter;
		};
}());

// 보조함수
function increase(n) {
		return ++n;
}

// 보조함수
function decrease(n) {
		return --n;
}

// 보조 함수를 전달하여 호출
console.log(counter(increase)); // 1
console.log(counter(increase)); // 2

// 자유 변수를 공유한다
console.log(counter(decrease)); // 1
console.log(counter(decrease)); // 0
```

-   makeCounter 함수가 반환하는 함수는 자신이 생성됐을 때의 렉시컬 환경인 makeCounter 함수의 스코프에 속한 counter 변수를 기억하는 클로저다
-   makeCounter 함수는 인자로 전달받은 보조 함수를 합성하여 자신이 반환하는 함수의 동작을 변경할 수 있다. 이때 주의해야 할 것은 makeCounter 함수를 호출해 함수를 반환할 때 반환된 함수는 자신만의 독립된 렉시컬 환경을 갖는다는 것 이다

-   1.에서 makeCounter 함수를 호출하면 함수의 실행 컨텍스트가 생성되고 함수는 함수 객체를 생성하여 반환 후 소멸 된다
-   makeCounter 함수가 반환한 함수는 함수의 렉시컬 환경을 상위 스코프로서 기억하는 클로저이며 전역변수인 increaser에 할당된다

-   2.에서 makeCounter 함수를 호출하면 새로운 함수의 실행 컨텍스트가 생성며 함수 객체를 생성하여 반환 후 소멸된다
-   makeCounter 함수가 반환한 함수는 함수의 렉시컬 환경을 상위 스코프로서 기억하는 클로저이며 전역 변수인 decreaser에 할당된다

⇒ 이때 makeConter 함수의 실행 컨텍스트는 소멸되지만 함수 실행 컨텍스트의 렉시컬 환경은 함수가 반환한 함수의 [[Enviroment]] 내부 슬롯에 의해 참조되고 있기 때문에 소멸되지 않는다

⇒ 전역 변수에 할당된 두 함수는 각각 자신만의 독립된 렉시컬 환경을 갖기 때문에 카운트를 유지하기 위한 자유 변수 counter를 공유하지 않아 카운터의 증감이 연동되지 않는다 따라서 독립된 카운터가 아니라 연동하여 증감이 가능한 카운터를 만들려면 렉시컬 환경을 공유하는 클로저를 만들어야 한다 ⇒ 이를 위해서는 makeCounter 함수를 두 번 호출하지 말아야 함

**24.5 캡슐화와 정보 은닉**

-   캡슐화는 객체의 상태를 나타내는 프로퍼티와 프로퍼티를 참조하고 조작할 수 있는 동작인 메서드를 하나로 묶는 것을 말함
-   캡슐화는 객체의 특정 프로퍼티나 메서드를 감출 목적으로 사용하기도 하는데 이를 정보은닉이라 함
-   정보은닉은 외부에 공개할 필요가 없는 구현의 일부를 외부에 공개되지 않도록 감추어 적절치 못한 접근으로부터 객체의 상태가 변경되는 것을 방지해 정보를 보호하고 객체간의 상호 의존성 즉 결합도를 낮추는 효과가 있다
-   자바스크립트는 public, private, protected 같은 접근 제한자를 제공하지 않는다 즉 객체의 모든 프로퍼티와 메서드는 기본적으로 public하다

```jsx
function Person(name, age) {
    this.name = name; // public
    let _age = age; // private

    // 인스턴스 메서드
    this.sayHi = function () {
        console.log(`hi my name is ${this.name} I am ${_age}`)
    }
}

const me = new Person('Jeong', 20);
me.sayHi();
console.log(me.name) // Jeong
console.log(me._age) // undefined

const you = new Person('Kim', 30);
you.sayHi();
console.log(you.name) // Kim
console.log(you._age) // undefined

// sayHi 메서드를 프로토타입 메서드로 변경하여 중복 생성 방지
========================================================

function Person(name, age) {
    this.name = name; // public
    let _age = age; // private
}

// 프로토타입 메서드
Person.prototype.sayHi = function () {
		//Person 생성자 함수의 지역 변수 _age를 참조할 수 없다
		console.log(`hi my name is ${this.name} I am ${_age}`)
}

// 즉시 실행 함수를 사용하여 하나의 함수 내에 모으기
=========================================================

const Person = (function (){
    let _age = 0; // private

    //생성자함수
    function Person(name, age) {
        this.name = name; // public
        _age = age;
    }
    //프로토타입 메소드
    Person.prototype.sayHi = function() {
        console.log(`hi my name is ${this.name} I am ${_age}`)
    }
    // 생성자 함수를 반환
    return Person;
}());

const me = new Person('Jeong', 20);
me.sayHi();
console.log(me.name) // Jeong
console.log(me._age) // undefined

const you = new Person('Kim', 30);
you.sayHi();
console.log(you.name) // Kim
console.log(you._age) // undefined
```

-   name 프로퍼티는 외부에 공개되어 있어 자유롭게 참조나 변경 가능 ⇒ public 하다
-   \_age 변수는 Person 생성자 함수의 지역 변수이므로 Person생성자 함수 외부에서 참조하거나 변경 할 수 없다 ⇒ private 하다

-   sayHi 메서드를 프로토타입 메서드로 변경하여 sayHi 메서드의 중복 생성을 방지해 보자
-   Person.prototype.sayHi 메서드 내에서 Person 생성자 함수의 지역 변수 \_age를 참조할 수 없는 문제가 발생

-   이를 즉시실행 함수를 사용하여 Person 생성자 함수와 Person.prototype.sayHi 메서드를 하나의 함수 내에 모아 보자
-   접근 제한자를 제공하지 않는 자바스크립트에서도 정보 은닉이 가능한것처럼 보인다
-   즉시 실행 함수가 반환하는 Person 생성자 함수와 인스턴스가 상속받아 호출할 Person.prototype.sayHi 메서드는 즉시 실행 함수가 종료된 이후 호출된다
-   하지만 Person 생성자 함수와 sayHi 메서드는 이미 종료되어 소멸한 즉시 실행 함수의 지역변수 \_age를 참조 할 수 있는 클로저다
-   하지만 이 코드도 문제가 있다 ⇒ Person 생성자 함수가 여러 개의 인스턴스를 생성할 경우 다음과 같이 \_age 변수의 상태가 유지되지 않는다

```jsx
const me = new Person("Jeong", 20);
me.sayHi(); // hi my name is Jeong I am 20

const you = new Person("Kim", 30);
you.sayHi(); // hi my name is Kim I am 30

// _age값이 변경됨
me.sayHi(); // hi my name is Kim I am 30
```

-   이는 Person.prototype.sayHi 메서드가 단 한번 생성되는 클로저이기 때문에 발생하는 현상으로 즉시 실행 함수가 호출될 때 생성된다
-   이때 Person.prototype.sayHi 메서드는 자신의 상위 스코프인 즉시 실행 함수의 실행 컨텍스트의 렉시컬 환경의 참조를 [[Environment]]에 저장하여 기억한다 ⇒ 따라서 Person 생성자 함수의 모든 인스턴스가 상속을 통해 호출할 수 있는 Person.prototype.sayHi 메서드의 상위 스코프는 어떤 인스턴스로 호출하더라도 하나의 동일한 상위 스코프를 사용하게 된다
-   이러한 이유로 Person 생성자 함수가 여러 개의 인스턴스를 생성할 경우 위와 같이 \_age 변수의 상태가 유지되지 않는다
-   이처럼 자바스크립트는 정보 은닉을 완전하게 지원하지 않으며 인스턴스 메서드를 사용한다면 자유 변수를 통해 private를 흉내 낼 수는 있지만 프로토타입 메서드를 사용하면 이마저도 불가능해 진다

**24.6 자주 발생하는 실수**

```jsx
var funcs = [];

for (var i=0; i<3; i++) {
		funcs[i] = function () {return i}; // 1.
}

for (var j =0; j<funcs.length; j++) {
		console.log(funcs[j]()); // 2.
}

// 클로저를 사용하여 바르게 동작하는 코드를 만들자
========================================================
var funcs = [];

for (var i=0; i<3; i++) {
		funcs[i] = (function (id) { // 1.
				return function() {
						return id;
				}
		}(i));
}

for (var j =0; j<funcs.length; j++) {
		console.log(funcs[j]());
}

// ES6의 let키워드를 사용하면 이 같은 번거로움이 해결된다
========================================================
var funcs = [];

for (let i=0; i < 3; i++) {
		funcs[i] = function() {return i;}
}

for (let i =0; i < funcs.length; i++) {
		console.log(funcs[i]()); // 0 1 2
}
```

-   첫번째 for문의 코드 블록 내에서 funcs 배열의 요소로 추가 된다
-   두번째 for문의 코드블록내에서 funcs 배열의 요소로 추가된 함수를 순차적으로 호출한다
-   funcs 배열의 요소로 추가된 3개의 함수가 0,1,2를 반환할것으로 기대하지만 결과는 그렇지 않다
-   var 키워드로 선언한 i는 함수 레벨스코프를 갖기때문에 전역 변수
-   전역 변수 i에는 0, 1, 2가 순차적으로 할당되는데 따라서 funcs 배열의 요소로 추가한 함수를 호출하면 전역 변수 i를 참조하여 i의 값 3이 출력된다

-   1.에서 즉시 실행 함수는 전역 변수 i에 현재 할당되어 있는 값을 인수로 전달받아 매개변수 id에 할당한 후 중첩 함수를 반환하고 종료된다 ⇒ 즉시 실행 함수가 반환한 함수는 funcs 배열에 순차적으로 저장된다
-   이때 즉시 실행 함수의 매개변수 id는 즉시 실행 함수가 반환한 중첩 함수의 상위 스코프에 존재하는데 즉시 실행 함수가 반환한 중첩 함수는 자신의 상위 스코프를 기억하는 클로저이고 매개변수 id는 즉시실행 함수가 반환한 중첩 함수에 묶여있는 자유 변수가 되어 그 값이 유지 된다

-   for문의 변수 선언문에서 let키워드로 선언한 변수를 사용하면 for문의 코드 블록이 반복 실행될 때마다 for문 코드 블록의 새로운 렉시컬 환경이 생성된다
-   만약 for문의 코드 블록 내에서 정의한 함수가 있따면 이 함수의 상위 스코프는 for문의 코드 블록이 반복 실행될 때마다 생성된 for문 코드 브록의 새로운 렉시컬 환경이다
-   이때 함수의 상위 스코프는 for문의 코드 블록이 반복 실행될 때마다 식별자의 값을 유지해야 하는데 이를 위해 for문이 반복될때마다 독립적인 렉시컬 환경을 생성하여 식별자의 값을 유지한다

# 25장 클래스

Created: November 6, 2022 4:41 PM

**25.1 클래스는 프로토타입의 문법적 설탕인가?**

-   프로토타입 기반 객체지향 언어는 클래스가 필요없는 객체지향 프로그래밍 언어다 ES6에서는 클래스 없이도 생성자 함수와 프로토타입을 통해 객체지향 언어의 상속을 구현할 수 있다
-   ES6에서 도입된 클래스는 클래스 기반 객체지향 프로그래밍에 익숙한 프로그래머가 더욱 빠르게 학습 할 수 있도록 하는 메커니즘을 제시함
-   클래스와 생성자 함수 모두 프로토타입 기반의 인스턴스를 생성하지만 정확히 동일하게 동작하지는 않는데 차이점은 다음과 같다

| 클래스                                                                                                           | 생성자 함수                                           |
| ---------------------------------------------------------------------------------------------------------------- | ----------------------------------------------------- |
| new 연산자 없이 호출하면 에러 발생                                                                               | new 연산자 없이 호출하면 일반 함수로 호출됨           |
| 상속을 지원하는 extends와 super키워드를 제공함                                                                   | 지원하지 않음                                         |
| 호이스팅이 발생하지 않는 것처럼 동작함                                                                           | 함수 선언문으로 정의된 생성자 함수는 함수 호이스팅이, |
| 함수 표현식으로 정의한 생성자 함수는 변수 호이스팅이 발생                                                        |
| 모든 코드에는 암묵적으로 strict mode가 지정되어 실행되며 해제 불가능                                             | 지정되지 않음                                         |
| constructor, 프로토타입 메서드, 정적 메서드는 모드 프로퍼티 어트리뷰트 [[Enumerable]] 값이 false ⇒ 열거되지 않음 |                                                       |

-   객체지향을 구현했따는 점에서 매우 유사하나 클래스는 생성자 함수 기반의 객체 생성 방식보다 견고하고 명료함
-   클래스를 프로토타입 기반 객세 생성 패턴의 단순한 문법적 설탕이라고 보기보다 새로운 객체 생성 메커니즘으로 보는게 합당

**25.2 클래스 정의**

```jsx
class Person {}

====================================================

// 익명 클래스 표현식
const Person = class {};

// 기명 클래스 표현식
const Person = class MyClass {};
```

-   class 키워드를 사용하여 정의 함
-   클래스의 이름은 파스칼 케이스를 사용하는 것이 일반적

-   일반적이진 않지만 표현식으로 정의할수도 있음
-   익명, 기명 둘 다 가능
-   클래스는 일급 객체로서 다음과 같은 특징을 갖음

    1. 무명의 리터럴로 생성할 수 있다 ⇒ 런타임에 생성이 가능함
    2. 변수나 자료구조(객체, 배열)에 저장할 수 있다
    3. 함수의 매개변수에 전달 할 수 있다
    4. 함수의 반환값으로 사용할 수 있다

-   클래스 몸체에는 0개 이상의 메서드만 정의 할 수 있다 정의 할 수 있는 메서드는 constructor, 프로토타입 메서드, 정적 메서드가 있다

```jsx
// 클래스 선언문
class Person {
    // 생성자
    costructor(name) {
        // 인스턴스 생성 및 초기화
        this.name = name; // name 프로퍼티는 public
    }
    // 프로토타입 메서드
    sayHi() {
        console.log(`hi i am ${this.name}`);
    }

    // 정적 메서드
    static sayHello() {
        console.log(`hello`);
    }
}

// 인스턴스 생성
const me = new Person("Lee");

// 인스턴스의 프로퍼티 참조
console.log(me.name); // Lee
// 프로토타입 메서드 호출
me.sayHi(); // hi i am Lee
// 정적 메서드 호출
Person.sayHello(); // hello
```

**25.3 클래스 호이스팅**

-   클래스 선언문으로 정의한 클래스는 함수 선언문과 같이 소스코드 평가 과정, 즉 런타임 이전에 먼저 평가되어 함수 객체를 생성한다
-   이때 클래스가 평가되어 생성된 함수 객체는 생성자 함수로서 호출할 수 있는 함수 즉 constructor이다
-   생성자 함수로서 호출할 수 있는 함수는 함수 정의가 평가되어 함수 객체를 생성하는 시점에 프로토타입도 더불어 생성된다
-   프로토타입과 생성자 함수는 단독으로 존재할 수 없고 언제나 쌍으로 존재해야 한다
-   단, 클래스는 클래스 정의 이전에 참조 할 수 없다

```jsx
const Person = "";

{
    // 호이스팅이 발생하지 않는다면 ''이 출력되어야 하지만
    //레퍼런스에러가 발생함
    console.log(Person);

    // 클래스 선언문
    class Person {}
}
```

-   클래스 선언문도 호이스팅이 발생
-   단, let이나 const 키워드로 선언한 변수처럼 호이스팅 된다
-   따라서 클래스 선언문 이전에 일시적 사각지대에 빠지기 때문에 호이스팅이 발생하지 않는 것처럼 동작함

**25.4 인스턴스 생성**

```jsx
class Person {}

// 인스턴스 생성
const me = new Person();
console.log(me); // Person {}
```

-   클래스는 생성자 함수이며 new연산자와 함께 호출되어 인스턴스를 생성함

```jsx
class Person {}

// new 연산자 없이 호출하면 타입 에러가 발생
const me = Person(); // TypeError
```

-   클래스는 반드시 new연산자와 함께 호출해야 함

```jsx
const Person = class MyClass {};

// 함수 표현식과 마찬가지로 클래스를 가리키는 식별자로 인스턴스를 생성해야 함
const me = new Person();

// 클래스 이름 MyClass는 함수와 동일하게 클래스 몸체 내부에서만 유효한 식별자다
// 외부 코드에서 접근 불가능 하기 때문
console.log(MyClass); // ReferenceError

const you = new MyClass(); // ReferenceError
```

**25.5 메서드**

**25.5.1 constructor**

```jsx
class Person {
    // 생성자
    constructor(name) {
        // 인스턴스 생성 및 초기화
        this.name = name;
    }
}

console.log(typeof Person); // function
console.dir(Person);
```

![Untitled](25%E1%84%8C%E1%85%A1%E1%86%BC%20%E1%84%8F%E1%85%B3%E1%86%AF%E1%84%85%E1%85%A2%E1%84%89%E1%85%B3%20884d109fc94a497fb4fa76a34f71a060/Untitled.png)

-   모든 함수 객체가 가지고 있는 prototype 프로퍼티가 가리키는 프로토타입 객체의 constructor 프로퍼티는 클래스 자신을 가리킨다 ⇒ 이는 클래스가 인스턴스를 생성하는 생성자 함수라는 것을 의미함 즉, new연산자와 함께 클래스를 호출하면 클래스는 인스턴스를 생성한다

```jsx
class Person {
    // 생성자
    constructor(name) {
        // 인스턴스 생성 및 초기화
        this.name = name;
    }
}

//생성자 함수
function Person(name) {
    this.name = name;
}
```

-   person 클래스의 constructor 내부에서 this에 추가한 name 프로퍼티가 클래스가 생성한 인스턴스의 프로퍼티로 추가 된 것을 확인할 수 있다 즉, 생성자 함수와 마찬가지로 constructor 내부에서 this에 추가한 프로퍼티는 인스턴스 프로퍼티가 된다
-   constructor 내부의 this는 생성자 함수와 마찬가지로 클래스가 새성한 인스턴스를 가리킨다

-   constructor는 생성자 함수와 유사하지만 차이가 있다

```jsx
class Person {
    // constructor는 생략하면 빈 constructor가 암묵적으로 정의됨
    constructor() {}
}

// 빈 객체 생성
const me = new Person();
console.log(me); // Person {}
```

-   constructor를 생략하면 클래스에 빈 constructor가 암묵적으로 정의 된다

```jsx
class Person {
		// 고정값으로 인스턴스 초기화
		constructor() {
				this.name = 'Lee';
				this.address = 'Seoul';
		}
}

// 빈 객체 생성
const me = new Person();
console.log(me) // Person {name:"Lee", address:"seoul"}

// 클래스 외부에서 초기값을 전달하려면
// constructor에 매게변수를 선언하고 인스턴스를 생성할때 값 전달
=========================================================
class Person {
		// 인수로 인스턴스 초기화
		constructor(name, address) {
				this.name = name;
				this.address = address;
		}
}

// 인스로 초기값을 전달
const me = new Person('Lee', 'Seoul');
console.log(me) // Person {name:"Lee", address:"seoul"}
```

-   프로퍼티가 추가되어 초기화된 인스턴스를 생성하려면 constructor 내부에서 this에 인스턴스 프로퍼티를 추가한다

-   constructor내에서는 인스턴스의 생성과 동시에 인스턴스 프로퍼티 추가를 통해 인스턴스의 초기화를 진행하기때문에 constructor를 생략해서는 안된다

```jsx
class Person {
    // 인수로 인스턴스 초기화
    constructor(name) {
        this.name = name;
        // 명시적으로 반환하면 암묵적 this가 반환이 무시됨
        return {};
    }
}

// constructor에서 명시적으로 반환한 빈 객체가 반환됨
const me = new Person("Lee");
console.log(me); // Person {}
```

-   constructor는 별도의 반환문을 갖지 않아야 함
-   new연산자와 함께 클래스가 호출되면 생성자 함수와 동일하게 암묵적으로 this, 즉 인스턴스를 반환하기 때문임
-   만약 this가 아닌 다른 객체를 명시적으로 반환하면 this, 즉 인스턴스가 반환되지 못하고 return 문에 명시한 객체가 반환됨
-   this가 아닌 다른 값을 반환하는 것은 클래스의 기본 동작을 훼손하는것이기 때문에 내부 return 문은 반드시 생략해야 함

**25.5.2 프로토타입 메서드**

```jsx
function Person(name) {
		this.name = name;
}

//프로토타입 메서드 명시적 추가
Person.prototype.sayHi = function () {
		console.log(`my name is ${this.name}`)
}

const me = new Person('Lee');
me.sayHi(); // my name is Lee

// 클래스는 prototype프로퍼티에 메서드를 추가하지 않아도 됨
====================================================

class Person {
		// 생성자
		constructor(name) {
				// 인스턴스 생성 및 초기화
				this.name = name;
		}
		//프로토타입 메서드
		sayHi() {
				console.log(`my name is ${this.name}`)
		}
}

const me = new Person('Lee');
me.sayHi(); // my name is Lee
```

-   생성자 함수를 사용하여 인스턴스를 생성하는 경우 프로토타입 메서드를 생성하기 위해서는 예제와 같이 명시적으로 프로토타입에 메서드를 추가하지만

-   클래스의 prototype 프로퍼티에 메서드를 추가하지 않아도 기본적으로 프로토타입 메서드가 된다

⇒ 생성자 함수와 마찬가지로 클래스가 생성한 인스턴스는 프로토타입 체인의 일원이 된다

⇒ 클래스 몸체에서 정의한 메서드는 인스턴스의 프로토타입에 존재하는 프로토타입 메서드가 되며 인스턴스는 프로토타입 메서드를 상속받아 사용할 수 있다

⇒ 결국 클래스는 생성자 함수와 같이 인스턴스를 생성하는 생성자 함수라고 볼 수 있으며 클래스는 생성자 함수와 마찬가지로 프로토타입 기반의 객체 생성 메커니즘이다

**25.5.3 정적 메서드**

-   정적 메서드는 인스턴스를 생성하지 않아도 호출할 수 있는 메서드를 말함

```jsx
// 생성자 함수
function Person(name) {
		this.name = name;
}

// 정적 메서드
Person.sayHi = function () {
		console.log(`hi`);
};

//정적 메서드 호출
Person.sayHi();

// 클래스에서는 static키워드를 붙이면 정적 메서드가 됨
========================================================
class Person {
		// 생성자
		constructor(name) {
				// 인스턴스 생성 및 초기화
				this.name = name;
		}
		// 정적 메서드
		static sayHi() {
				console.log(`hi`);
		}
}

Person.sayHi() // hi
```

-   생성자 함수의 경우 정적 메서드를 생성하기 위해서는 명시적으로 생성자 함수에 메서드를 추가해야 함

-   클래스에서는 static 키워드를 붙이면 정적 메서드가 됨
-   정적 메서드는 클래스에 바인딩된 메서드가 되며 함수 객체로 평가되므로 자신의 프로퍼티/메서드를 소유할 수 있다
-   클래스는 정의가 평가되는 시점에 함수 객체가 인정되므로 인서턴스와 달리 별다른 생성 과정이 필요 없음 ⇒ 따라서 정적 메서드는 클래스 정의 이후 인스턴스를 생성하지 않아도 호출 할 수 있다

**25.5.4 정적 메서드와 프로토타입 메서드의 차이**

| 정적 메서드                             | 프로토타입 메서드                       |
| --------------------------------------- | --------------------------------------- |
| 자신이 속해 있는 프로토타입 체인이 다름 | 자신이 속해 있는 프로토타입 체인이 다름 |
| 클래스로 호출                           | 인스턴스로 호출                         |
| 인스턴스 프로퍼티를 참조할 수 없음      | 인스턴스 프로퍼티를 참조할 수 있음      |

**25.5.5 클래스에서 정의한 메서드의 특징**

1. function 키워드를 생략한 메서드 축약 표현을 사용함
2. 객체 리터럴과는 다르게 클래스에 메서드를 정의할 때는 콤마가 필요없음
3. 암묵적으로 strict mode로 실행됨
4. for…in문이나 Object.keys 메서드 등으로 열거할 수 없음 [[Enumerable]]의 값이 false임
5. 내부 메서드 [[Construct]]를 갖지 않는 non-constructor다 따라서 new 연산자와 함께 호출할 수 없다

**25.6 클래스의 인스턴스 생성 과정**

-   new연산자와 함께 클래스를 호출하면 생성자 함수와 마찬가지로 클래스의 내부 메서드 [Construct]]가 호출된다
-   클래스는 new연산자 없이 호출할 수 없다
    1. **인스턴스 생성과 this 바인딩**
    -   new연산자와 함께 클래스를 호출하면 빈 객체가 생성되며 이 빈 객체가 클래스가 생성한 인스턴스다
    -   암묵적으로 생성된 빈 객체, 즉 인스턴스는 this에 바인딩 된다 ⇒ 따라서 constructor 내부의 this는 클래스가 생성한 인스턴스를 가리킴
    1. **인스턴스 초기화**
    -   constructor의 내부 코드가 실행되어 this에 바인딩 되어 있는 인스턴스를 초기화 한다
    1. **인스턴스 반환**
    -   클래스의 모든 처리가 끝나면 완성된 인스턴스가 바인딩된 this를 암묵적으로 반환한다
    ```jsx
    class Person {
        // 생성자
        constructor(name) {
            // 1. 암묵적으로 인스턴스가 생성되고 this에 바인딩 됨

            console.log(this); // Person {}
            console.log(Object.getPrototypeOf(this) === Person.prototype); // true

            // 2. this에 바인딩되어 있는 인스턴스를 초기화
            this.name = name;

            // 3. 완성된 인스턴스가 바인딩된 this를 암묵적으로 반화
        }
    }
    ```

**25.7 프로퍼티**

**25.7.1 인스턴스 프로퍼티**

-   인스턴스 프로퍼티는 constructor 내부에서 정의해야 함
-   constructor 내부 코드가 실행되기 이전에 내부의 this에는 이미 클래스가 암묵적으로 생성한 인스턴스인 빈 객체가 바인딩 되어 있다

**25.7.2 접근자 프로퍼티**

-   접근자 프로퍼티는 자체적으로는 값 ([[Value]] 내부슬롯)을 갖지 않고 다른 데이터 프로퍼티의 값을 읽거나 저장할 때 사용하는 접근자 함수로 구성된 프로퍼티이다
-   접근자 프로퍼티는 클래스에서도 사용할 수 있다 ⇒ 객체 리터럴을 클래스로 표현하면 다음과 같다

```jsx
class Person {
    constructor(firstName, lastName) {
        this.firstName = firstName;
        this.lastName = lastName;
    }

    // fullName은 접근자 함수로 구성된 접근자 프로퍼티다
    // getter 함수
    get fullName() {
        return `${this.firstName} ${this.lastName}`;
    }

    //setter 함수
    set fullName(name) {
        [this.firstName, this.lastName] = name.split(" ");
    }
}

const me = new Person("yesol", "jeong");

//데이터 프로퍼티를 통한 프로퍼티 값의 참조
console.log(`${me.firstName} ${me.lastName}`); // yesol jeong

//접근자 프로퍼티를 통한 프로퍼티 값의 저장
//접근자 프로퍼티 fullName에 값을 저장하면 setter 함수가 호출된다
me.fullName = "Heegun Lee";
console.log(me); // {fisrtName: "Heegun", lastName: "Lee"}

//접근자 프로퍼티를 통한 프로퍼티 값의 참조
// 접근자 프로퍼티 fullName에 접근하면 getter함수가 호출된다
console.log(me.fullName); // Heegun Lee

// fullName은 접근자 프로퍼티다
// 접근자 프로퍼티는 get, set, enumerable, configurable 프로퍼티 어트리뷰트를 갖는다
console.log(Object.getOwnPropertyDescriptor(Person.prototype, "fullName"));
```

-   접근자 프로퍼티는 자체적으로는 값을 갖지 않고 다른 데이터 프로퍼티의 값을 읽거나 저장할 때 사용하는 접근자 함수인 getter, setter함수로 구성되어 있다
-   getter는 인스턴스 프로퍼티에 접근할 때마다 프로퍼티 값을 조작하거나 별도의 행위가 필요할 때 사용
-   setter는 인스턴스 프로퍼티에 값을 할당할 때마다 프로퍼티 값을 조작하거나 별도의 행위가 필요할 때 사용
-   getter는 호출하는 것이 아니라 프로퍼티 처럼 참조하는 형식으로 사용하며 참조시에 getter가 호출됨
-   setter도 호출하는 것이 아니라 프로퍼티처럼 값을 할당하는 형식으로 사용하며 할당시에 setter가 호출됨
-   getter는 무언가를 취득할때 사용하므로 반드시 무언가를 반환해야 하고 setter는 무언가를 프로퍼티에 할당해야 할때 사용하므로 반드시 매게변수가 있어야 함

**25.7.3 클래스 필드 정의 제안**

-   클래스 필드는 클래스 기반 객체지향 언어에서 클래스가 생성할 인스턴스의 프로퍼티를 가리키는 용어이다

```jsx
clss Person {
		// 클래스 필드 정의
		name = 'Lee';
}

const me = new Person();
console.log(me) // Person{name: "Lee"}
```

-   클래스 몸체에서 클래스 필드를 정의할 수 있는 클래스 필드 정의 제안은 아직 ECMAStript의 정식 표준 사양으로 승급되지 않았지만 최신 브라우저와 최신 Node.js에서는 다음 예제와 같이 클래스 필드를 클래스 몸체에 정의 할 수 있다

```jsx
class Person{
		// this에 클래스 필드를 바인딩 해서는 안됨
		this.name = ''; // syntaxError
}

=====================================================

class Person{
		name = 'Lee';

		constructor() {
				console.log(name) // ReferneceError
		}
}

new Person();

=====================================================

class Person{
		name
}

const me = new Person();
console.log(me) // Person {name: undefined}
```

-   this에 클래스 필드를 바인딩 해서는 안됨 ⇒ costructor내에서만 유효

-   this를 반드시 사용해야 함

-   클래스 필드에 초기값을 할당하지 않으면 undefined

**25.7.4 private 필드 정의 제안**

-   자바스크립트는 캡슐화를 완전하게 지원하지 않음 언제나 public이다

```jsx
class Person{
		#name = '';

		constructor(name) {
				this.#name = name;
		}
}

const me = new Person('Lee')

// private 필드 #name은 클래스 외부에서 참조할 수 없다
console.log(me.#name); // SyntaxError

// 접근자 프로퍼티를 통해 간접적으로 접근하는 방법은 유효함
=======================================================

class Person{
		// 클래스 몸체에 정의해야 함 constructor에 정의하면 에러
		#name = '';

		constructor(name) {
				this.#name = name;
		}

		get name() {
				// private 필드를 참조하여 trim한다음 반환
				return this.#name.trim();
		}
}

const me = new Person('Lee')
console.log(me.name); // Lee
```

| 접근 가능성                 | public | private |
| --------------------------- | ------ | ------- |
| 클래스 내부                 | O      | O       |
| 자식 클래스 내부            | O      | X       |
| 클래스 인스턴스를 통한 접근 | O      | X       |

**25.8 상속에 의한 클래스 확장**

**25.8.1 클랫 상속과 생성자 함수 상속**

-   상속에 의한 클래스 확장은 기존 클래스를 상속받아 새로운 클래스를 확장하여 정의 하는 것 이다
-   클래스와 생성자 함수는 인스턴스를 생성할 수 있는 함수라는 점에서 매우 유사하지만 클래스는 상속을 통해 기존 클래스를 확장할 수 있는 문법이 기본적으로 제공되지만 생성자 함수는 그렇지 않다

```jsx
class Anamal {
    constructor(age, weight) {
        this.age = age;
        this.weight = weight;
    }

    eat() {
        return "eat";
    }
    move() {
        return "move";
    }
}

//상속을 통해 Animal 클래스를 확장한 Bird 클래스
class Bird extends Anamal {
    fly() {
        return "fly";
    }
}

const bird = new Bird(1, 5);

console.log(bird); // Bird {age: 1, weight: 5}
console.log(bird instanceof Bird); // true
console.log(bird instanceof Animal); // true

console.log(bird.eat()); // eat
console.log(bird.move()); // move
console.log(bird.fly()); // fly
```

-   클래스는 상속을 통해 다른 클래스를 확장할 수 있는 문법인 extends 키워드가 기본적으로 제공된다
-   extends 키워드를 사용한 클래스 확장은 간편하고 직관적이다

**25.8.2 extends 키워드**

```jsx
// 수퍼(파생/부모)클래스
class Base {}

// 서브(파생/자식) 클래스
class Derived extends Base {}
```

-   상속을 통해 클래스를 확장하려면 extends 키워드를 사용하여 상속받을 클래스를 정의함

**25.8.3 동적 상속**

```jsx
function Base1() {}

class Base2 {}

// 조건에 따라 동적으로 상속 대상을 결정하는 서버 클래스
class Derived extends (condition ? Base1 : Base2) {}

const derived = new Derived();
console.log(derived); // Derived {}

console.log(derived instanceof Base1); // true
console.log(derived instanceof Base2); // false
```

-   extends 키워드는 클래스 뿐만 아니라 생성자 함수를 상속받아 클래스를 확장할 수도 있다 단, extends키워드 앞에는 반드시 클래스가 와야 함
-   extends 키워드는 클래스뿐만 아니라 [[Construct]]내부 메서드를 갖는 함수 객체로 평가될 수 있는 모든 표현식을 사용할 수 있음

**25.8.4 서브클래스의 constructor**

-   클래스에서 constructor를 생략하면 클래스에 비어 있는 constructor가 암묵적으로 정의 됨

**25.8.5 super 키워드**

-   super 키워드는 함수처럼 호출할 수도 있고 this와 같이 식별자처럼 참조할 수 있는 특수한 키워드다
-   다음과 같이 동작함
    1. super를 호출하면 수퍼클래스의 constructor(super-constructor)를 호출한다
    2. super를 참조하면 수퍼클래스의 메서드를 호출할 수 있다
    -   **super 호출**
        -   super를 호출하면 수퍼클래스의 constructor(super-constructor)를 호출한다
    ```jsx
    //수퍼클래스
    class Base {
        constructor(a, b) {
            this.a = a;
            this.b = b;
        }
    }

    // 서브클래스
    class Derived extends Base {
        // 다음과 같이 암묵적으로 constructor가 정의된다
        // constructor( ... args) { super( ...args); }
    }

    const derived = new Derived(1, 2);
    console.log(derived); // Derived {a: 1, b: 2}
    ```
    -   수퍼클래스의 constructor 내부에서 추가한 프로퍼티를 그대로 갖는 인스턴스를 생성한다면 서브클래스의 constructor를 생략할 수 있다
    -   이때, new 연산자와 함께 서브클래스를 호출하면서 전달한 인수는 모두 서브클래스에 암묵적으로 정의된 constructor의 super 호출을 통해 수퍼클래스의 constructor에 전달됨
    ```jsx
    //수퍼클래스
    class Base {
        constructor(a, b) {
            // 4.
            this.a = a;
            this.b = b;
        }
    }

    // 서브클래스
    class Derived extends Base {
        constructor(a, b, c) {
            // 2.
            super(a, b); // 3.
            this.c = c;
        }
    }

    const derived = new Derived(1, 2, 3); //1.
    console.log(derived); // Derived {a: 1, b: 2, c: 3}
    ```
    -   수퍼클래스에서 추가한 프로퍼티와 서브클래스에서 추가한 프로퍼티를 갖는 인스턴스를 생성한다면 서브클래스의 costnructor를 생략할 수 없다
    -   이때 new 연산자와 함께 서브클래스를 호출하면서 전달한 인수 중에서 수퍼클래스의 constructor에 전달할 필요가 있는 인수는 서브클래스의 constoructor에 서 호출하는 super를 통해 전달됨
    -   new 연산자와 함께 Derived 클래스를 호출(1)하면서 전달한 인수 1,2,3의 Derived의 클래스의 constructor(2)로 전달되고 super(3)호출을 통해 Base 클래스의 constructor(4)에 일부가 전달된다

    -   super를 호출할때 주의할 사항

        1. 서브클래스에서 constructor를 생략하지 않는 경우 서브클래스의 constructor에서는 반드시 super를 호출해야 함
        2. 서브클래스의 constructor에서 super를 호출하기 전에는 this를 참조할 수 없다
        3. super는 반드시 서브클래스의 constructor에서만 호출한다 서브클래스가 아닌 클래스의 constructor나 함수에서 호출하면 에러

    -   **super 참조**
        -   메서드 내에서 super를 참조하면 수퍼클래스의 메서드를 호출할 수 있다
            ```jsx
            class Base {
            		constructor(name) {
            			this.name = name
            		}

            		sayHi() {
            				return `hi ${this.name}`;
            		}
            }

            // 서브 클래스

            class Derived extends Base {
            		sayHi() {
            		// super.sayHi는 수퍼클래스의 프로토타입 메서드를 가리킴
            			return `${super.sayHi() how are you}`
            		}
            }

            const derived = new Derived('Lee');
            console.log(derived.sayHi()) // hi Lee how are you
            ```
            1. 서브클래스의 프로토타입 메서드 내에서 super.sayHi는 수퍼클래스의 프로토타입 메서드 sayHi를 가리킨다
            -   super 참조를 통해 수퍼클래스의 메서드를 참조하려면 super가 수퍼클래스의 메서드가 바인딩된 객체, 즉 수퍼클래스의 prototype 프로퍼티에 바인딩된 프로토타입을 참조할 수 있어야 함
            ```jsx
            class Base {
            		static sayHi() {
            			return 'hi'
            	}
            }

            // 서브 클래스
            class Derived extends Base {
            		startic sayHi() {
            		// super.sayHi는 수퍼클래스의 정적 메서드를 가리킴
            			return `${super.sayHi() how are you}`
            		}
            }

            console.log(Derived.sayHi()) // hi Lee how are you
            ```
            1. 서브클래스의 정적 메서드 내에서 super.sayHi는 수퍼클래스의 정적 메서드 sayHi를 가리킴

**25.8.6 상속 크래스의 인스턴스 생성 과정**

1. 서브 클래스의 super 호출
    - 자바스크립트 엔진은 클래스를 평가할때 수퍼클래스와 서브클래스를 구분하기 위해 base 또는 derived를 값으로 갖는 내부슬롯을 갖는데 내부 슬롯값이 base로 설정되지만 다른 클래스를 상속받는 서브클래스는 내부 슬롯의 값이 derived로 설정된다 이를 통해 수퍼 클래스와 서브클래스는 new연산자와 함께 호출되었을 때의 동작이 구분된다
    - 서브클래스는 자신이 직접 인스턴스를 생성하지 않고 수퍼클래스에게 인스턴스 생성을 위함한다 ⇒ 이것이 바로 서브클래스의 consctuctor에서 반드시 super를 호출해야 하는 이유이다
2. 수퍼클래스의 인스턴스 생성과 this바인딩
    - 수퍼클래스의 constructor 내부의 코드가 실행되기 이전에 암묵적으로 빈 객체를 생성하며 이 빈 객체가 클래스가 생성한 인스턴스다 즉, 인스턴스는 this에 바인딩된다 따라서, 수퍼클래스의 constructor 내부의 this는 생성된 인스턴스를 가리킨다
3. 수퍼클래스의 인스턴스 초기화
    - 수퍼클래스의 constructor가 실행되어 this에 바인딩 되어 있는 인스턴스를 초기화 한다 즉, this에 바인딩 되어 있는 인스턴스에 프로퍼티를 추가하고 constructor가 인수로 전달받은 초기값으로 인스턴스의 프로퍼티를 초기화 한다
4. 서브클래스 constructor로의 복귀와 this바인딩
    - super의 호출이 종료되고 제어 흐름이 constructor로 돌아오는데 이때 super가 반환한 인스턴스가 this에 바인딩 된다. 서브클래스는 별도의 인스턴스를 생성하지 않고 super가 반환한 인스턴스를 this에 바인딩하여 그대로 사용한다
    - super가 호출되지 않으면 인스턴스가 생성되지 않으며 this바인딩도 할 수 없다 서브클래스의 constructor에서 super를 호출하기 전에는 this를 참조할 수 없는 이유가 바로 이 때문이다
5. 서브클래스의 인스턴스 초기화
    - super호출 히우 서브클래스의 constructor에 기술되어 있는 인스턴스 초기화가 실행된다, 즉 this바인딩되어 있는 인스턴스에 프로퍼티를 추가하고 constructor가 인수로 전달받은 초기값으로 인스턴스 프로퍼티를 초기화 한다
6. 인스턴스 변환
    - 클래스의 모든 처리가 끝나면 완성된 인스턴스가 바인딩된 this가 암묵적으로 반환된다

**25.8.7 표준 빌트인 생성자 함수 확장**

-   extends 키워드는 클래스 뿐만 아니라 construct내부 메서드를 갖는 함수 객체로 평가될 수 있는 모든 표현식을 사용할 수 있다
-   String, Number, Array 같은 표준 빌트인 객체도 해당 키워드를 사용하여 확장 할 수 있다
