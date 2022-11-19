# 28장 Number

Created: November 18, 2022 10:54 AM

**28.1 Number 생성자 함수**

```jsx
const numObj = new Number();
console.log(numObj); 
// Number{[[PrimitiveValue]]: 0}
```

- 표준 빌트이니 객체인 Number는 생성자 함수 객처로 new 연산자와 함께 호출하여 인스턴스를 생성 할 수 있다 ⇒ 인수를 전달하지 않고 호출하면 내부 슬롯에 0을 할당한 래퍼 객체를 생성

```jsx
const numObj = new Number(10);
console.log(numObj); 
// Number{[[PrimitiveValue]]: 10}
```

- [[PrimitiveValue]] 접근할 수 없는 프로프티가 보이는데 이는 [[NumberData]] 내부 슬롯을 가리킴 ⇒ Number 생성자 함수의 인수로 숫자를 전달하면서 호출하면 내부 슬롯에 인수로 전달받은 숫자를 할당한 래퍼 객체를 생성

```jsx
let numObj = new Number('10');
console.log(numObj); 
// Number{[[PrimitiveValue]]: 10}

numObj = new Number('hello');
console.log(numObj); 
// Number{[[PrimitiveValue]]: NaN}
```

- 숫자가 아닌 값을 전달하면 인수를 숫자로 강제 변환하여 변환된 숫자를 할당하는 객체를 생성함
- 인수를 숫자로 변환할 수 없다면 NaN을 내부 슬롯에 할당한 래퍼 객체를 생성함

```jsx
// 문자열 타입 => 숫자 타입
Number('0') // 0
Number('-1') // -1
Number('10.53') // 10.53

//불리언 타입 => 숫자 타입
Number(true) // 1
Number(false) // 0
```

- new 연산자를 사용하지 않고 Number 생성자 함수를 호출하면 인스턴스가 아닌 숫자를 반환 ⇒ 이를 이용하여 명시적으로 타입을 변환하기도 함

**28.2 Number 프로퍼티**

**28.2.1 Number.EPSILON**

- ES6에서 도입된 Number.EPSILON은 1과 1보다 큰 숫자 중에서 가장 작은 숫자와의 차이와 같다
- 약 2.2204460492503130808472633361816 x 10-16

```jsx
0.1 + 0.2 // 0.30000000000000004
0.1 + 0.2 === 0.3 // false
```

- 부동소수점 산술 연산은 정확한 결과를 기대하기 어렵다
- 정수는 2진법으로 오차없이 저장 가능하지만 부동소수점은 2진법으로 변환했을 때 무한소수가 되어 미세한 오차가 발생할 수 밖에 없는 구조적 한계가 있다

```jsx
function isEqual(a, b){
	// a와 b를 뺀 값의 절대값이 Number.EPSILON보다 
	// 작으면 같은 수로 인정한다
	return Math.abs(a - b) < Number.EPSILON;
}

isEqual(0.1 + 0.2, 0.3); // true
```

- Number.EPSILON은 부동소수점으로 발생하는 오차를 해결하기 위해 사용함

**28.2.2 Number.MAX_VALUE**

- 자바스크립트에서 표현할 수 있는 가장 큰 양수 값이다 ⇒ Number.MAX_VALUE보다 큰 숫자는 Infinity다

```jsx
Number.MAX_VALUE // 1.7976931348623157e+308
Infinity > Number.MAX_VALUE // true
```

**28.2.3 Number.MIN_VALUE**

- 자바스크립트에서 표현할 수 있는 가장 작은 양수 값이다 ⇒ Number.MIN_VALUE보다 작은 숫자는 0

```jsx
Number.MIN_VALUE // 5e-324
Number.MIN_VALU > 0; // true
```

**28.2.4 Number.MAX_SAFE_INTEGER**

- 자바스크립트에서 안전하게 표현할 수 있는 가장 큰 정수값

```jsx
Number.MAX_SAFE_INTEGER // 9007199254740991
```

**28.2.5 Number.MIN_SAFE_INTEGER**

- 자바스크립트에서 안전하게 표현할 수 있는 가장 작은 정수값

```jsx
Number.MIN_SAFE_INTEGER // -9007199254740991
```

**28.2.6 Number.POSITIVE_INFINITY**

- 양의 무한대를 나타내는 숫자값으로 Infinity와 같다

```jsx
Number.POSITIVE_INFINITY // Infinity
```

**28.2.7 Number.NEGATIVE_INFINITY**

- 음의 무한대를 나타내는 숫자값으로 -Infinity와 같다

```jsx
Number**.NEGATIVE_INFINITY** // -Infinity
```

**28.2.8 Number.NaN**

- 숫자가 아님을 나타내는 숫자값이다

```jsx
Number**.NaN** // NaN
```

**28.3 Number 메서드**

**28.3.1 Number.isFinite**

- ES6에서 도입 ⇒ 정적 메서드는 인수로 전달된 숫자값이 정상적인 유한수, Infinity 또는 -Infinity 가 아닌지 검사하여 그 결과를 불리언 값으로 반환

```jsx
// 인수가 정상적인 유한수이면 true를 반환한다
Number.isFinite(0) // true
Number.isFinite(Number.MAX_VALUE) // true
Number.isFinite(Number.MIN_VALUE) // true

// 인수가 무한수이면 false를 반환한다
Number.isFinite(Infinity) // false
Number.isFinite(-Infinity) // false

// 인수가 NaN이면 언제나 false를 반환
Number.isFinite(NaN) // false

// Number.isFinite는 인수를 숫자로 암묵적 타입 변환하지 않음
Number.isFinite(null) // false

// isFinite는 인수를 숫자로 암묵적 타입 변환함
isFinite(null) // true
```

**28.3.2 Number.isInteger**

- ES6도입 ⇒ 정적 메서드는 인수로 전달된 숫자값이 **정수인지 검사**하여 불리언 값으로 반환한다 검사하기 전 인수를 숫자로 암묵적 타입 변환하지 않음

```jsx
//인수가 정수이면 true를 반환
Number.isInteger(0) // true
Number.isInteger(123) // true
Number.isInteger(-123) // true

//소수는 정수가 아님
Number.isInteger(0.5) // false

// '123'을 숫자로 암묵적 타입 변환하지 않음
Number.isInteger('123') // false

//false를 숫자로 암묵적 타입 변환하지 않음
Number.isInteger(false) // false

//Infinity -Infinity는 정수가 아님
Number.isInteger(Infinity) // false
Number.isInteger(-Infinity) // false
```

**28.3.3 Number.isNaN**

- ES6에서 도입 ⇒ 정적 메서드는 인수로 전달된 숫자값이 NaN인지 검사하여 불리언값으로 반환

```jsx
Number.isNaN(NaN) // true
```

```jsx
//Number.isNaN은 인수를 숫자로 암묵적 타입 변환하지 않음
Number.isNaN(undefined) // false

//isNaN은 인수를 숫자로 암묵적 타입 변환함
//undefined는 NaN으로 암묵적 타입 변환됨
isNaN(undefined) // true
```

**28.3.4 Number.isSafeInteger**

- ES6에서 도입 ⇒ 정적 메서드는 인수로 전달된 숫자값이 안전한 정수인지 검사하여 불리언 값으로 반환, 인수를 숫자로 암묵적 타입 변환하지 않음

```jsx
// 0은 안전한 정수
Number.isSafeInteger(0) //true

// 10000000000은 안전한 정수
Number.isSafeInteger(10000000000) //true

// 0.5는 정수가 아님
Number.isSafeInteger(0.5) //false

//'123'을 숫자로 암묵적 타입 변환하지 않음
Number.isSafeInteger('123') //false

//false를 숫자로 암묵적 타입 변환하지 않음
Number.isSafeInteger(false) //false

//Infinity는 정수가 아님
Number.isSafeInteger(Infinity) //false
```

**28.3.5 Number.prototype.toExponential**

- 메서드는 숫자를 지수 표기법으로 변환하여 문자열로 반환 ⇒ 지수 표기법이란 매우 크거나 작은 숫자를 표기할 때 사용하며 e(Exponent) 앞에 있는 숫자의 10의 n승을 곱하는 형식으로 수를 나타내는 방식이다
- 인수로 소수점 이하로 표현할 자릿수를 전달 할 수 있음

```jsx
(77.1234).toExponential() // "7.71234e+1"
(77.1234).toExponential(4) // "7.7123e+1"
(77.1234).toExponential(2) // "7.71e+1"
```

```jsx
77.toExponential() //SyntaxError

// 그룹연산자를 사용할 것을 권장
(77).toExponential() // "7.7e+1"

// .뒤에 공백이 오면 프로퍼티 접근 연산자로 해석
77 .toExponential() // "7.7e+1"
```

- 숫자 리터럴과 함께 Number 프로토타입 메서드를 사용할 경우 에러 발생
- 숫자 뒤의 .은 의미가 모호 ⇒ 부동소수점의 기호일수도 있고 객체 프로퍼티에 접근하기 위한 접근 연산자 일수도 있음

**28.3.6 Number.prototype.toFixed**

- 숫자를 반올림하여 문자열로 반환 함

```jsx
// 소수점 이하 반올림, 인수를 생략하면 기본값 0이 지정됨
(12345.6789).toFixed() // '12346'

// 소수점 이하 1자릿수 유효, 나머지 반올림
(12345.6789).toFixed(1) // '12345.7'

// 소수점 이하 2자릿수 유효, 나머지 반올림
(12345.6789).toFixed(2) // '12345.68'

// 소수점 이하 3자릿수 유효, 나머지 반올림
(12345.6789).toFixed(3) // '12345.679'
```

**28.3.7 Number.prototype.toPrecision**

- 인수로 전달받은 전체 자릿수까지 유효하도록 나머지 자릿수를 반올림하여 문자열로 반환, 인수로 전달받은 전체 자릿수로 표현할 수 없는 경우 지수 표기법으로 결과 반환

```jsx
//전체 자릿수 유효, 인수를 생략하면 기본값 0이 지정
(12345.6789).toPrecision() // '12345.6789'

//전체 1자릿수 유효, 나머지 반올림
(12345.6789).toPrecision(1) // "1e+4"

//전체 2자릿수 유효, 나머지 반올림
(12345.6789).toPrecision(2) // "1.2e+4"

//전체 6자릿수 유효, 나머지 반올림
(12345.6789).toPrecision(6) // "12345.7"
```

**28.3.8 Number.prototype.toString**

- 숫자를 문자열로 반환

```jsx
// 인수를 생략하면 10진수 문자열을 반환
(10).toString() // 10

// 2진수 문자열을 반환
(16).toString(2) // "10000"

// 8진수 문자열을 반환
(16).toString(8) // "20"

// 8진수 문자열을 반환
(16).toString(16) // "10"
```

# 29장 Math

Created: November 18, 2022 3:58 PM

**29.1 Math 프로퍼티**

**29.1.1 Math.PI**

```jsx
Math.PI // 3.141592653589793
```

- 원주율 PI값을 반환

**29.2 Math 메서드**

**29.2.1 Math.abs**

```jsx
Math.abs(-1) // 1
Math.abs('-1') // 1
Math.abs('') // 0
Math.abs([]) // 0
Math.abs(null) // 0
Math.abs(undefined) // NaN
Math.abs({}) // NaN
Math.abs("string") // NaN
Math.abs() // NaN
```

- 인수로 전달된 숫자의 절대값을 반환함, 절대값은 반드시 0 또는 양수여야 함

**29.2.2 Math.round**

```jsx
Math.round(1.4) // 1
Math.round(1.6) // 2
Math.round(-1.4) // -1
Math.round(-1.6) // -2
Math.round(1) // 1
Math.round() // NaN
```

- 인수로 전달된 숫자의 소수점 이하를 반올림한 정수를 반환함

**29.2.4 Math.ceil**

```jsx
Math.ceil(1.4) // 2
Math.ceil(1.6) // 2
Math.ceil(-1.4) // -1
Math.ceil(-1.6) // -1
Math.ceil(1) // 1
Math.ceil() // NaN
```

- 인수로 전달된 숫자의 소수점 이하를 올림한 정수를 반환
- 소수점 이하를 올림하면 더 큰 정수가 됨

**29.2.4 Math.floor**

```jsx
Math.floor(1.9) // 1
Math.floor(9.1) // 9
Math.floor(-1.9) // -2
Math.floor(-9.1) // -10
Math.floor(1) // 1
Math.floor() // NaN
```

- 인수로 전달된 숫자의 소수점 이하를 내림한 정수를 반환 Math.ceil과 반대 개념

**29.2.5 Math.sqrt**

```jsx
Math.sqrt(9) // 3
Math.sqrt(-9) // NaN 
Math.sqrt(2) // 1.4142135623730951
Math.sqrt(1) // 1
Math.sqrt(0) // 0
Math.sqrt() // NaN
```

- 인수로 전달된 숫자의 제곱근을 반환

**29.2.6 Math.random**

- 임의의 난수를 반환, 0~1미만의 실수

```jsx
Math.random() // 0~1미만 실수

// 1에서 10범위의 랜덤 정수 취득
// 1. 0~1미만의 실수를 구한 다음 10을 곱하기
// 2. 0~10미만의 랜덤 실수에 1을 더해 1에서 10 범위의 랜덤 실수 구하기
// 3. Math.floor로 1~10 범위의 랜덤 실수의 소수점 이하를 떼어 버린 다음 정수 반환
const random1 = Math.floor(Math.random()*10) + 1
console.log(random1)
```

**29.2.7 Math.pow**

```jsx
Math.pow(2, 8) // 256
Math.pow(2, -1) // 0.5
Math.pow(2) // NaN

// ES7 지수 연산자를 사용하면 가독성이 더 좋음
2 ** 2** 2 // 16
Math.pow(Math.pow(2, 2), 2) // 16
```

- 첫 번째 인수를 밑으로, 두번째 인수를 지수로 거듭제곱한 결과를 반환

**29.2.8 Math.max**

```jsx
Math.max(1) // 1
Math.max(1, 2) // 2
Math.max(1, 2, 3) // 3
Math.max() // -Infinity

// 배열 요소 중 최대값 
Math.max.apply(null, [1,2,3]) //3

// ES6 스프레드 문법
Math.max(...[1,2,3]) // 3
```

**29.2.9 Math.min**

- 전달받은 인수 중에서 가장 큰 수를 반환 함
- 인수가 전달되지 않으면 -Infinity를 반환

- 배열의 최대값을 구하려면 Function.prototype.apply또는 스프레드 문법을 사용해야 함

```jsx
Math.min(1) // 1
Math.min(1, 2) // 1
Math.min(1, 2, 3) // 1
Math.min() // Infinity

// 배열 요소 중 최대값 
Math.min.apply(null, [1,2,3]) //1

// ES6 스프레드 문법
Math.min(...[1,2,3]) // 1
```

- 전달 받은 인수 중에서 가장 작은 수를 반환
- 인수가 전달되지 않으면 Infinity 반환

- 배열의 최대값을 구하려면 Function.prototype.apply또는 스프레드 문법을 사용해야 함

# 30장 Date

Created: November 18, 2022 5:33 PM

**30.1 Date 생성자 함수**

- 생성자 함수로 생성한 객체는 내부적으로 날짜와 시간을 나타내는 정수값을 가짐
- 1970년 1월 1일 00:00:00(UTC)를 기점으로 Date 객체가 나타나는 날짜와 시간까지의 밀리초를 나타냄
- 현재 날짜와 시간이 아닌 다른 날짜와 시간을 다르고 싶은 경우 함수에 명시적으로 해당 날짜와 시간 정보를 인수로 지정해야 함

**30.1.1 new Date()**

```jsx
// 인수없이 호출하면 현재 날짜와 시간을 가지는 객체를 반환
new Date() //Fri Nov 18 2022 17:38:32 GMT+0900 (대한민국 표준시)

// new연산자 없이 호출하면 문자열 반환
Date() //"Fri Nov 18 2022 17:39:19 GMT+0900 (대한민국 표준시)"
```

**30.1.2 new Date(milliseconds)**

- 숫자 타입의 밀리초를 인수로 전달하면 1970년 1월 1일 00:00:00(UTC)를 기점으로 전달된 밀리초만큼 경과한 날짜와 시간을 나타내는 객체를 반환함

```jsx
// 한국 표준시 KST는 협정 세계시 UTC에 9시간을 더한 시간임
new Date(0) // Thu Jan 01 1970 09:00:00 GMT+0900 (대한민국 표준시)

// 86,400,000ms는 1day를 의미함
// 1s = 1,000ms
// 1m = 60s * 1,000ms = 60,000ms
// 1h = 60m * 60,000ms = 3,600,000ms
// 1d = 24h * 3,600,000ms = 86,400,000ms

new Date(86400000) //Date Fri Jan 02 1970 09:00:00 GMT+0900 (대한민국 표준시)
```

**30.1.3 new Date(dateString)**

- 생성자 함수에 날짜와 시간을 나타내는 문자열을 인수로 전달하면 지정된 날짜와 시간을 나타내는 객제를 반환하는데 이때 인수로 전달한 문자열은 Date.parse 메서드에 의해 해석 가능한 형식이어야 함

```jsx
new Date('May 26, 2020 10:00:00') //Date Tue May 26 2020 10:00:00 GMT+0900 (대한민국 표준시)
new Date('2020/03/26/10:00:00') //Date Thu Mar 26 2020 10:00:00 GMT+0900 (대한민국 표준시)
```

**30.1.4 new Date(year, month[, day, hour, minute, second, millisecond])**

- 생성자 함수에 연, 월, 일, 시, 분, 초, 밀리초를 의미하는 숫자를 인수로 전달하면 지정된 날짜와 시간을 나타내는 객체를 반환함, 이때 연, 월은 반드시 지정해야 함, 지정하지 않은 옵션 정보는 0 또는 1로 초기화 됨

| 인수 | 내용 |
| --- | --- |
| year | 연을 나타냄, 1900년 이후의 정수, 0~99는 1900~1999로 처리 |
| month | 월을 나타냄, 0~11까지의 정수(0부터 시작) |
| day | 일을 나타냄, 1~31까지의 정수 |
| hour | 시를 나타냄, 0~23까지의 정수 |
| minute | 분을 나타냄, 0~59까지의 정수 |
| second | 초를 나타냄, 0~59까지의 정수 |
| millisecond | 밀리초를 나타냄, 0~99까지의 정수 |
- 연, 월을 지정하지 않으면 1970년 1월 1일 00:00:00(UTC)를 나타내는 Date를 반환함

**30.2 Date 메서드**

**30.2.1 Date.now**

```jsx
const now = Date.now(); // 1668831956933
// Sat Nov 19 2022 13:25:56 GMT+0900 (대한민국 표준시)
new Date(now); 
```

- 기준날짜를 기점으로 현재 시간까지 경과한 밀리초를 숫자로 반환함

**30.2.2 Date.parse**

```jsx
// UTC
Date.parse('Nov 19, 2022 00:00:00 UTC'); //1668816000000
// KST
Date.parse('Nov 19, 2022 00:00:00'); //1668783600000
Date.parse('2022/11/19/00:00:00'); //1668783600000

```

- 기준날짜를 기점으로 인수로 전달된 지정시간 까지의 밀리초를 숫자로 반환함
- UTC라고 명시해 놓지 않으면 로컬 타임으로 인식함

**30.2.3 Date.UTC**

```jsx
Date.UTC(2022, 10, 19) //1668816000000
Date.UTC('1970/1/2') //NaN
```

- 기준날짜를 기점으로 인수로 전달된 지정 시간까지의 밀리초를 숫자로 반환
- UTC로 인식하며 0부터 시작이므로 주의 해야 함

**30.2.4 Date.prototype.getFullYear**

```jsx
new Date('2022/11/19').getFullYear(); // 2022
```

- 객체의 연도를 나타내는 정수를 반환

**30.2.5 Date.prototype.setFullYear**

```jsx
const today = new Date();

//년도 지정
today.setFullYear(2000)
today.getFullYear(); // 2000

//년도, 월, 일 지정
today.setFullYear(1900, 0, 1)
today.getFullYear(); // 1900
```

- 객체에 연도를 나타내는 정수를 설정함, 연도 이외에 옵션으로 월, 일도 설정 가능

**30.2.6 Date.prototype.getMonth**

```jsx
new Date('2020/07/24').getMonth(); // 6
```

- 객체의 월을 나타내는 0~11의 정수를 반환
- 0부터 시작

**30.2.7 Date.prototype.setMonth**

```jsx
const today = new Date();

//월 지정
today.setMonth(0) // 1월
today.getMonth(); // 0

//월, 일 지정
today.setMonth(11, 1) // 12월 1일
today.setMonth(); // 11
```

- 객체에 월을 나타내는 0~11의 정수를 설정
- 월 이외에 옵션으로 일도 설정 할 수 있음

**30.2.8 Date.prototype.getDate**

```jsx
new Date('2020/07/24').getDate(); //24
```

- 객체의 날짜(1~31)를 나타내는 정수를 반환

**30.2.9 Date.prototype.setDate**

```jsx
const today = new Date();

//날짜 지정
today.setDate(1);
today.getDate(); // 1
```

- 객체에 날짜(1~31)를 나타내는 정수를 설정

**30.2.10 Date.prototype.getDay**

```jsx
new Date('2022/11/19').getDay(); //6
```

- 객체의 요일(0~6)을 나타내는 정수를 반환

| 요일 | 반환값 |
| --- | --- |
| 일 | 0 |
| 월 | 1 |
| 화 | 2 |
| 수 | 3 |
| 목 | 4 |
| 금 | 5 |
| 토 | 6 |

**30.2.11 Date.prototype.getHours**

```jsx
new Date('2020/7/24/12:00').getHours(); // 12
```

- 객체의 시간(0~23)을 나타내는 정수를 반환

**30.2.12 Date.prototype.setHours**

```jsx
const today = new Date();

//월 지정
today.setHours(7) 
today.getHours(); // 7

// 시간, 분, 초, 밀리초 지정
today.setHours(0, 0, 0, 0) // 12월 1일
today.getHours(); // 0
```

- 객체의 시간(0~23)을 나타내는 정수를 설정
- 시간 이외에 옵션으로 분, 초, 밀리초도 설정 가능

**30.2.13 Date.prototype.getMinutes**

```jsx
new Date('2022/11/19/12:40').getMinutes() // 40
```

- 객체에 분(0~59)을 나타내는 정수를 반환

**30.2.14 Date.prototype.setMinutes**

```jsx
const today = new Date();

//월 지정
today.setMinutes(50) 
today.getMinutes(); // 50

// 분, 초, 밀리초 지정
today.setMinutes(5, 10, 999) // HH:05:10:999
today.getMinutes(); // 5
```

- 객체에 분(0~59)을 나타내는 정수를 설정
- 분 외에 초, 밀리초도 설정 가능

**30.2.15 Date.prototype.getSeconds**

```jsx
new Date('2022/11/19/12:30:10').getSeconds(); // 10
```

- 객체의 초(0~59)를 나타내는 정수를 반환

**30.2.16 Date.prototype.setSeconds**

```jsx
const today = new Date();

//월 지정
today.setSeconds(30) 
today.getSeconds(); // 30

// 분, 초, 밀리초 지정
today.setSeconds(10, 0) // HH:MM:10:000
today.getSeconds(); // 5
```

- 객체에 초(0~59)를 나타내는 정수를 설정
- 초 이외에 밀리초도 설정 가능

**30.2.17 Date.prototype.getMilliseconds**

```jsx
new Date('2020/11/19/12:30:10:150').getMilliseconds()
// 150 ?? NaN 나옴
```

- 객체의 밀리초(0~999)를 나타내는 정수를 반환

**30.2.18 Date.prototype.setMilliseconds**

```jsx
const today = new Date();

//밀리초지정
today.setMilliseconds(123)
today.getMilliseconds()
```

- 객체의 밀리초(0~999)를 나타내는 정수를 설정

**30.2.19 Date.prototype.getTime**

```jsx
new Date('2020/07/24/12:30').getTime(); //1595561400000
```

- 기준날짜를 기점으로 Date객체의 시간까지 경과된 밀리초를 반환

**30.2.20 Date.prototype.setTime**

```jsx
const today = new Date();

today.setTime(86400000); //1day
console.log(today) 
// Fri Jan 02 1970 09:00:00 GMT+0900 (대한민국 표준시)
```

- 기준날짜를 기점으로 경과된 밀리초를 설정함

**30.2.21 Date.prototype.getTimezoneOffset**

```jsx
// KST
const today = new Date();

// UTC와 today의 지정 로캘 KTS와의 차이는 -9시간임
today.getTimezoneOffset() / 60 // -9
```

- UTC와 date 객체에 지정된 로캘 시간과의 차이를 분단위로 반환

**30.3.22 Date.prototype.toDateString**

```jsx
const today = new Date('2022/11/19/12:30');

// "Sat Nov 19 2022 12:30:00 GMT+0900 (대한민국 표준시)"
today.toString();

// "Sat Nov 19 2022"
today.toDateString();
```

- 사람이 읽을 수 있는 형식의 문자열로 객체의 날짜를 반환

**30.2.23 Date.prototype.toTimeString**

```jsx
const today = new Date('2022/11/19/12:30');

// "Sat Nov 19 2022 12:30:00 GMT+0900 (대한민국 표준시)"
today.toString();

// "12:30:00 GMT+0900 (대한민국 표준시)"
today.toTimeString();
```

- 사람이 읽을 수 있는 형식으로 객체의 시간을 표현한 문자열을 반환

**30.2.24 Date.prototype.toISOString**

```jsx
const today = new Date('2022/11/19/12:30');

today.toISOString(); //"2022-11-19T03:30:00.000Z"

today.toISOString().slice(0, 10) //"2022-11-19"
today.toISOString().slice(0, 10).replace(/-/g, '')
// "20221119"
```

- ISO 8601 형식으로 객체의 날짜와 시간을 표현한 문자열을 반환

**30.2.25 Date.prototype.toLocaleString**

```jsx
const today = new Date('2000/11/19/12:30');

today.toLocaleString(); //"2000. 11. 19. 오후 12:30:00"
today.toLocaleString('ko-KR'); //"2000. 11. 19. 오후 12:30:00"
today.toLocaleString('en-US'); //"11/19/2000, 12:30:00 PM"
today.toLocaleString('ja-JP'); //"2000/11/19 12:30:00"
```

- 인수로 전달한 로캘을 기준으로 객체의 날짜와 시간을 표현한 문자열을 반환
- 인수를 생략한 경우 브라우저가 동작중인 시스템의 로캘을 적용함

**30.2.25 Date.prototype.toLocaleTimeString**

```jsx
const today = new Date('2000/11/19/12:30');

today.toLocaleTimeString(); // "오후 12:30:00"
today.toLocaleTimeString('ko-KR'); // "오후 12:30:00"
today.toLocaleTimeString('en-US'); // "12:30:00 PM"
today.toLocaleTimeString('ja-JP'); // "12:30:00"
```

- 인수로 전달한 로캘을 기준으로 객체의 날짜와 시간을 표현한 문자열을 반환
- 인수를 생략한 경우 브라우저가 동작중인 시스템의 로캘을 적용함

**30.3 Date를 활용한 시계 예제**

- 현재 날짜와 시간을 초 단위로 반복 출력함

```jsx
(function printNow() {
    const today = new Date();

    const dayNames = [
        "(일요일)",
        "(월요일)",
        "(화요일)",
        "(수요일)",
        "(목요일)",
        "(금요일)",
        "(토요일)",
    ];

    // getDay 메서더는 해당 요일(0~6)을 나타내는 정수를 반환한다
    const day = dayNames[today.getDay()];

    const year = today.getFullYear();
    const month = today.getMonth() + 1;
    const date = today.getDate();
    let hour = today.getHours();
    let minute = today.getMinutes();
    let second = today.getSeconds();
    const ampm = hour >= 12 ? "PM" : "AM";

    // 12시간제로 변경
    hour %= 12;
    hour = hour || 12; // hour이 0이면 12를 재할당

    // 10미만인 분과 초를 2자리로 변경
    minute = minute < 10 ? "0" + minute : minute;
    second = second < 10 ? "0" + second : second;

    const now = `${year}년 ${month}월 ${date}일 ${day} ${hour}:${minute}:${second} ${ampm}`;

    console.log(now);

    // 1초바다 printNow 함수를 재귀 호출함
    setTimeout(printNow, 1000);
})();
```

# 31장 RegExp

Created: November 19, 2022 3:47 PM

**31.1 정규 표현식이란?**

- 정규 표현식은 일정한 패턴을 가진 문자열의 집합을 표현하기 위해 사용하는 형식 언어
- 자바스크립트의 고유 문법이 아니며 대부분 프로그래밍 언어와 코드 에디터에 내장되어 있음
- 정규 표현식은 문자열을 대상으로 패턴 매칭 기능을 제공함
- 패턴 매칭 기능이란 특정 패턴과 일치하는 문자열을 검색하거나 추출 또는 치환할 수 있는 기능을 말함

```jsx
// 사용자로부터 입력받은 휴대폰 전화번호
const tel = '010-1234-567팔';

// 정규 표현식 리터럴로 휴대폰 전화번호 패턴을 정의
const regExp = /^\d{3}-\d{4}-\d{4}$/;

// tel이 휴대폰 전화번호 패턴에 매칭하는지 테스트 한다
regExp.test(tel) //  false
```

- 만약 정규표현식을 사용하지 않는다면 반복문과 조건문을 통해 한 문자씩 연속해서 체크해야 함
- 정규표현식을 사용하면 반복문과 조건문 없이 패턴을 정의하고 테스트하는 것으로 간단히 체크할 수 있다 하지만 정규표현식은 주석이나 공백을 허용하지 않고 여러가지 기호를 혼합해서 사용하기 때문에 가독성은 좋지 않다

**31.2 정규 표현식의 생성**

- 정규 표현식 객체를 생성하기 위해서는 리터럴과 RegExp 생성자 함수를 사용할 수 있다
- 정규표현식 리터럴은 패턴과 플래그로 구성된다

![Untitled](31%E1%84%8C%E1%85%A1%E1%86%BC%20RegExp%2056665e09e3cd43ff81521b15834b2682/Untitled.png)

```jsx
const target = 'Is this all there is?'

//패턴: is
//플래그: i => 대소문자를 구별하지 않고 검색한다
const regexp = /is/i

// test 메서드는 target 문자열에 대해 정규 표현식 regexp의 패턴을 검색하여
// 매칭 결과를 불리언 값으로 변환함
regexp.test(target) // true
```

- RegExp 생성자 함수를 사용하여 RegExp객체를 생성할 수도 있다

```jsx
// pattern : 정규 표현식의 패턴
// flags : 정규 표현식의 플래그 (g, i, m, u, y)

new RegExp(pattern[, flags])
```

```jsx
const target = 'Is this all there is?'

const regexp = new RegExp(/is/i)
// const regexp = new RegExp(/is/, 'i')
// const regexp = new RegExp('is', 'i')

regexp.test(target) // true
```

- RegExp 생성자 함수를 사용하면 변수를 사용해 동적으로 객체를 생성할 수도 있다

```jsx
const count = (str, char) => (str.match(new RegExp(char, 'gi')) ?? []).length;

count('Is this all there is?', 'is') // 3
count('Is this all there is?', 'xx') // 0
```

**31.3 RegExp 메서드**

**31.3.1 RegExp.prototype.exec**

- exec 메서드는 인수로 전달받은 문자열에 대해 정규 표현식의 패턴을 검색하여 매칭 결과를 배열로 반환함, 매칭결과가 없는 경우 null 반환
- exec 메서드는 문자열 내의 모든 패턴을 검색하는 g플래그를 지정해도 첫 번째 매칭 결과만 반환하므로 주의하기 바람

```jsx
const target = 'Is this all there is?';
const regExp = /is/;

regExp.exec(target);
// ['is', index: 5, input: 'Is this all there is?', groups: undefined]
```

**31.3.2 RegExp.prototype.test**

- 인수로 전달받은 문자열에 대해 정규 표현식의 패턴을 검색하여 매칭 결과를 불리언 값으로 반환

```jsx
const target = 'Is this all there is?';
const regExp = /is/;

regExp.test(target); // true
```

**31.3.3 String.prototype.match**

- String 표준 빌트인 객체가 제공하는 marth 메서드는 대상 문자열과 인수로 전달받은 정규 표현식과의 매칭 결과를 배열로 반환함

```jsx
const target = 'Is this all there is?';
const regExp = /is/;

target.match(regExp); 
// ['is', index: 5, input: 'Is this all there is?', groups: undefined]
```

- exec 메서드는 문자열 내의 모든 패턴을 검색하는 g플래그를 지정해도 첫 번째 매칭 결과만 반환하지만 String.prototype.match 메서드는 g플래그가 지정되면 모든 매칭 결과를 배열로 반환함

```jsx
const target = 'Is this all there is?';
const regExp = /is/g;

target.match(regExp); // ['is', 'is']
```

**31.4 플래그**

- 패턴과 함께 정규 표현식을 구성하는 플래그는 정규 표현식의 검색 방식을 설정하기 위해 사용함, 플래그는 총 6개 그 중 중요한 3개 확인

| 플래그  | 의미 | 설명 |
| --- | --- | --- |
| i | Ignore case | 대소문자를 구별하지 않고 패턴을 검색함 |
| g | Global | 대상 문자열 내에서 패턴과 일치하는 모든 문자열을 전역 검색 |
| m | Multi Line | 문자열의 행이 바뀌더라도 패턴 검색을 계속함 |
- 플래그는 옵션이므로 선택적으로 사용할 수 있으며 순서와 상관없이 하나 이상의 플래그를 동시에 설정할 수 있음
- 어떠한 플래그를 사용하지 않는 경우 대소문자를 구별하여 패턴을 검색함
- 문자열에 패턴 검색 매칭 대상이 1개 이상 존재해도 첫 번째 매칭한 대상만 검색하고 종료

```jsx
const target = 'Is this all there is?';

// target 문자열에서 is 문자열을 대소문자를 구별하여 한번만 검색
target.match(/is/);
// ['is', index: 5, input: 'Is this all there is?', groups: undefined]

// target 문자열에서 is 문자열을 대소문자를 구별하지 않고 한번만 검색
target.match(/is/i);
// ['Is', index: 0, input: 'Is this all there is?', groups: undefined]

// target 문자열에서 is 문자열을 대소문자를 구별하여 전역 검색 
target.match(/is/g);
// ['is', 'is']

// target 문자열에서 is 문자열을 대소문자를 구별하지 않고 전역 검색
target.match(/is/ig);
// ['Is', 'is', 'is']
```

**31.5 패턴**

- 정규 표현식은 일정한 규칙을 가진 문자열의 집합을 표현하기 위해 사용하는 형식 언어임
- 패턴과 플래그로 구성되며 패턴은 문자열의 일정한 규칙을 표현하기 위해 사용하며, 플래그는 검색 방식을 설정하기 위해 사용함
- 패턴은 /로 열고 닫으며 문자열의 따옴표는 생략 ⇒ 따옴표를 포함하면 따옴표까지 패턴에 포함되어 검색 됨
- 패턴은 메타문자 또는 기호로 표현할 수 있으며 어떤 문자열 내에 패턴과 일치하는 문자열이 존재할 때 ‘정규 표현식과 매치’한다고 표현함

**31.5.1 문자열 검색**

- 패턴에 문자 또는 문자열을 지정하면 검색 대상 문자열에서 패턴으로 지정한 문자 또는 문자열을 검색 함
- 정규 표현식을 생성하는 것만으로는 검색이 수행되지는 않으며 RegExp메서드를 사용하여 검색 대상 문자열과 정규 표현식의 매칭 결과를 구하면 검색이 수행됨
- 검색 대상 문자열과 플래그를 생략한 정규 표현식의 매칭 결과를 구하면 대소문자를 구별하여 정규 표현식과 매치한 첫 번째 결과만 반환함

```jsx
const target = 'Is this all there is?';

// is 문자열과 매치하는 패턴, 플래그가 생략되었으므로 대소문자를 구별함
const regExp = /is/;

// target과 정규 표현식이 매치하는지 테스트 함
regExp.test(target); // true

// target과 정규 표현식의 매칭 결과를 구함
target.match(regExp);
// ['is', index: 5, input: 'Is this all there is?', groups: undefined]
```

- 대소문자를 구별하지 않고 검색하려면 플래그 i를 사용함

```jsx
const target = 'Is this all there is?';

// is 문자열과 매치하는 패턴, 플래그 i를 추가하면 대소문자를 구별하지 않음
const regExp = /is/i;

target.match(regExp);
// ['Is', index: 0, input: 'Is this all there is?', groups: undefined]
```

- 패턴과 매치하는 모든 문자열을 전역 검색하려면 플래그 g사용

```jsx
const target = 'Is this all there is?';

// is 문자열과 매치하는 패턴, 플래그 i를 추가하면 대소문자를 구별하지 않음
const regExp = /is/ig;

target.match(regExp); // ['Is', 'is', 'is']
```

**31.5.2 임의의 문자열 검색**

- .은 임의의 문자 한개를 의미함, 문자의 내용은 무엇이든 상관없음
- 다음의 예제의 경우 .을 3개 연속된 패턴을 생성했으므로 문자의 내용과 상관없이 3자리 문자열을 매치한다

```jsx
const target = 'Is this all there is?';

// is 문자열과 매치하는 패턴, 플래그 i를 추가하면 대소문자를 구별하지 않음
const regExp = /.../g;

target.match(regExp); // ['Is ', 'thi', 's a', 'll ', 'the', 're ', 'is?']
```

**31.5.3 반복 검색**

- {m, n}은 앞선 패턴(다음 예제의 경우 A)이 최소 m번, 최대 n번 반복되는 문자열을 의미함, 콤마 뒤에 공백이 있으면 정상 동작하지 않으므로 주의

```jsx
const target = 'A AA B BB Aa Bb AAA';

// A가 최소 1번, 최대 2번 반복되는 문자열을 전역 검색함
const regExp = /A{1,2}/g;

target.match(regExp); // ['A', 'AA', 'A', 'AA', 'A']
```

- {n}은 앞선 패턴이 n번 반복되는 문자열을 의미함, 즉 {n}은 {n, n}과 같음

```jsx
const target = 'A AA B BB Aa Bb AAA';

// A가 2번 반복되는 문자열을 전역 검색함
const regExp = /A{2}/g;

target.match(regExp); // ['AA', 'AA']
```

- {n,}은 앞선 패턴이 최소 n번 반복되는 문자열을 의미함

```jsx
const target = 'A AA B BB Aa Bb AAA';

// A가 최소 2번 이상 반복되는 문자열을 전역 검색함
const regExp = /A{2,}/g;

target.match(regExp); // ['AA', 'AAA']
```

- +는 앞선 패턴이 최소 한번 이상 반복되는 문자열을 의미함, 즉 {1,}과 같음

```jsx
const target = 'A AA B BB Aa Bb AAA';

// A가 최소 1번 이상 반복되는 문자열을 전역 검색함
const regExp = /A+/g;

target.match(regExp); // ['A', 'AA', 'A', 'AAA']
```

- ?는 앞선 패턴이 최대 한 번(0번 포함) 이상 반복되는 문자열을 의미함, 즉, {0, 1}과 같음
- /colou?r/은 ‘colo’ 다음 ‘u’가 최대 한번(0번 포함) 이상 반복되고 ‘r’이 이어지는 문자열 ‘color’, ‘colour’와 매치함

```jsx
const target = 'color colour'

// 'colo'다음 'u'가 최대 한 번(0번 포함) 이상 반복되고 'r'이 이어지는 
// 문자열 'color', 'colour'를 전역 검색함
const regExp = /colou?r/g;

target.match(regExp); // ['color', 'colour']
```

**31.5.4 OR 검색**

- |은 or의 의미를 갖음, 다음 예제의 /A|B/는 ‘A’또는 ‘B’를 의미함

```jsx
const target = 'A AA B BB Aa Bb AAA';

// A또는 B를 전역 검색함
const regExp = /A|B/g;

target.match(regExp); // ['A', 'A', 'A', 'B', 'B', 'B', 'A', 'B', 'A', 'A', 'A']
```

- 분해되지 않은 단어 레벨로 검색하기 위해서는 +를 함께 사용함

```jsx
const target = 'A AA B BB Aa Bb';

// A또는 B가 한번 이상 반복되는 문자열을 전역 검색함
const regExp = /A|B+/g;

target.match(regExp); // ['A', 'AA', 'B', 'BB', 'A', 'B']
```

- 위 예제를 간단히 표현하면 다음과 같음 []내의 문자는 or로 동작함, 그 뒤에 +를 사용하면 앞선 패턴을 한번 이상 반복함

```jsx
const target = 'A AA B BB Aa Bb';

// A또는 B가 한번 이상 반복되는 문자열을 전역 검색함
const regExp = /[AB]+/g;

target.match(regExp); // ['A', 'AA', 'B', 'BB', 'A', 'B']
```

- 범위를 지정하려면 []내에 -를 사용함, 대문자 알파벳을 검색함

```jsx
const target = 'A AA BB ZZ Aa Bb';

// A ~ Z가 한번 이상 반복되는 문자열을 전역 검색함
const regExp = /[A-Z]+/g;

target.match(regExp); // ['A', 'AA', 'BB', 'ZZ', 'A', 'B']
```

- 대소문자를 구별하지 않고 알파벳을 검색하는 방법은 다음과 같음

```jsx
const target = 'AA BB Aa Bb 12';

// A ~ Z 또는 a ~ z가 한번 이상 반복되는 문자열을 전역 검색함
const regExp = /[A-Za-z]+/g;

target.match(regExp); // ['AA', 'BB', 'Aa', 'Bb']
```

- 숫자를 검색하는 방법은 다음과 같음

```jsx
const target = 'AA BB 12,345';

// 0~9가 한번 이상 반복되는 문자열을 전역 검색함
const regExp = /[0-9]+/g;

target.match(regExp); // ['12', '345']

=========================//쉼표=======================

// 쉼표 때문에 매칭 결과가 분리되므로 쉼표를 패턴에 포함시키기
const regExp2 = /[0-9,]+/g;

target.match(regExp2); // ['12,345']

=========================//간단히======================

// 위 예제를 간단히 표현하면 다음과 같음 \d는 숫자를 의미함 => [0, 9]와 같음\D는 숫자가 아닌 문자임
// 0~9 또는 ','가 한번 이상 반복되는 문자열을 전역 검색함
let regExp3 = /[\d,]+/g;

target.match(regExp3); // ['12,345']

//숫자가 아닌 문자 또는 ','가 한번 이상 반복되는 문자열을 전역 검색함
regExp3 = /[\D,]+/g;

target.match(regExp3); // ['AA BB ', ',']

```

- \w는 알파벳, 숫자, 언더스코어를 의미함 [A-Za-z0-9_]와 같음, \W는 알파벳, 숫자, 언더스코어를 제외한 문자를 의미함

```jsx
const target = 'Aa Bb 12,345 _$%&';

//  알파벳, 숫자, 언더스코어, ','가 한번 이상 반복되는 문자열을 전역 검색함
let regExp = /[\w,]+/g;

target.match(regExp); // ['Aa', 'Bb', '12,345', '_']

================================================================

// 알파벳, 숫자, 언더스코어가 아닌 문자 또는 ','가 한번 이상 반복되는 문자열을 전역 검색함
regExp = /[\W,]+/g;

target.match(regExp); // [' ', ' ', ',', ' ', '$%&']
```

**31.5.5 NOT 검색**

- […]내의 ^은 not의 의미를 갖음
- [0-9] = \d
- [^0-9] = 숫자를 제외한 문자를 의미함 = \D
- [A-Za-z0-9] = \w
- [^A-Za-z0-9] = \W

```jsx
const target = 'AA BB 12 Aa Bb';

//  숫자를 제외한 문자열을 전역 검색함
let regExp = /[^0-9]+/g;

target.match(regExp); // ['AA BB ', ' Aa Bb']
```

**31.5.6 시작 위치로 검색**

- […] 밖의 ^은 문자열의 시작을 의미함 단, […] 내의 ^은 not의 의미를 가지므로 주의

```jsx
const target = 'https://naver.com';

// https로 시작하는지 검사함
const regExp = /^https/;

regExp.test(target) // true
```

**31.5.7 마지막 위치로 검색**

- $는 문자열의 마지막을 의미함

```jsx
const target = 'https://naver.com';

const regExp = /com$/;

regExp.test(target) // true
```

**31.6 자주 사용하는 정규표현식**

**31.6.1 특정 단어로 시작하는지 검사**

- 검색 대상 문자열이 ‘http://’ 또는 ‘https://’로 시작하는지 검사함
- […]바깥의 ^은 문자열의 시작을 의미, ?은 앞선 패턴(아래 예제에서는 s)이 최대 한번(0번 포함)이상 반복되는지 의미 ⇒ 검색 대상 문자열에 앞선 패턴 ‘s’이 있어도 없어도 매치됨

```jsx
const url = 'https://naver.com';

/^https?:\/\//.test(url)

=============위 예제와 같음===========

/^(https|http):\/\//.test(url)
```

**31.6.2 특정 단어로 끝나는지 검사**

- html로 끝나는지 검사함 $은 문자열의 마지막을 의미함

```jsx
const fileName = 'index.html';

//html로 끝나는지 검사함
/html$/.test(fileName) // true
```

**31.6.3 숫자로만 이루어진 문자열인지 검사**

- […] 바깥의 ^은 문자열의 시작을 $는 문자열의 마지막을 의미함
- \d는 숫자를 의미하고 +는 앞선 패턴이 최소 한번 이상 반복되는 문자열을 의미함 ⇒ 처음과 끝이 숫자이고 최소 한번이상 반복되는 문자열과 매치함

```jsx
const target = '12345';

//숫자로만 이루어진 문자열인지 검사한다
/^\d+$/.test(target) // true
```

**31.6.4 하나 이상의 공백으로 시작하는지 검사**

- 검색 대상 문자열이 하나 이상의 공백으로 시작하는지 검사함, \s는 여러가지 공백문자를 의미함

```jsx
const target = ' Hi!'

// 하나 이상의 공백으로 시작하는지 검사함
/^[\s]+/.test(target); //true
```

**31.6.5 아이디로 사용 가능한지 검사**

- 알파벳 대소문자 또는 숫자로 시작하고 끝나며 4~10자리인지 검사함
- {4, 10}은 앞선 패턴(알파벳 대소문자 또는 숫자)이 최소 4번, 최대 10번 반복되는 문자열을 의미함 ⇒ 4~10자리로 이루어진 알파벳 대소문자 또는 숫자를 의미함

```jsx
const id = 'adc123'

// 알파벳 대소문자 또는 숫자로 시작하고 끝나며 4~10자리인지 검사
/^[A-Za-z0-9]{4, 10}$/.test(id) // true
```

**31.6.6 메일 주소 형식에 맞는지 검사**

```jsx
const email = 'yesol@gmail.com';

/^[0-9a-zA-Z]([-_\.]?[0-9a-zA-Z])*@[0-9a-zA-Z]([-_\.]?[0-9a-zA-Z])*\.[a-zA-Z]{2,3}$/.test(email) // true
```

**31.6.7 핸드폰 번호 형식에 맞는지 검사**

```jsx
const phone = '010-1234-5678'

// \d(숫자) 3자리 반복 / 3~4 숫자 / 4자리 숫자
/^\d{3}-\d{3,4}-\d{4}$/.test(phone) // true
```

**31.6.8 특수 문자 포함 여부 검사**

```jsx
const target = 'abc#123';

// 문자, 숫자를 제외 => 특수문자
(/[^A-Za-z0-9]/gi).test(target) // true

// 특수문자 제거
target.replace(/[^A-Za-z0-9]/gi, '') // abc123
```

# 32장 String

Created: November 19, 2022 11:47 PM

**32.1 String 생성자 함수**

- 표준 빌트인 객체인 String 객체는 생성자 함수 객체임
- 따라서 new 연산자와 함께 호출하여 String 인스턴스를 생성할 수 있음
- 인수를 전달하지 않고 new 연산자와 함께 호출하면 [[StringData]] 내부 슬롯에 빈 문자열을 할당한 String 래퍼 객체를 생성함

```jsx
const strObj = new String();
console.log(strObj); // String {length: 0, [[PrimitiveValue]]: ""}
```

- String 생성자 함수의 인수로 문자열을 전달하면서 new 연산자와 함께 호출하면  [[StringData]] 내부 슬롯에 인수로 전달받은 문자열을 할당한 래퍼 객체를 생성한다

```jsx
const strObj = new String('lee');
console.log(strObj); // String {0: "l", 1: "e", 2: "e", length: 3, [[PrimitiveValue]]: 'lee'}

// 유사배열객체면서 이터러블임 배열과 유사하게 인덱스를 사용하여 각 문자에 접근 가능
console.log(strObj[0]) // l

// 단, 원시값이므로 변경 불가능 => 에러가 발생하지 않음
strObj[0] = 's'
console.log(strObj) // 'lee'
```

- 함수의 인수로 문자열이 아닌 값을 전달하면 인수를 문자열로 강제 변환 후 내부 슬롯에 변환된 문자열을 할당한 객체를 생성함

```jsx
let strObj = new String(123);
console.log(strObj); // String {0: "1", 1: "2", 2: "3", length: 3, [[PrimitiveValue]]: '123'}

strObj = new String(null);
console.log(strObj); // String {0: "n", 1: "u", 2: "l", 3: "l", length: 4, [[PrimitiveValue]]: 'null'}
```

- new 연산자를 사용하지 않고 String 생성자 함수를 호출하면 String 인스턴스가 아닌 문자열을 반환함

```jsx
// 숫자 -> 문자열
String(1); // '1'
String(NaN); // 'NaN'
String(Infinity); // 'Infinity'

// 불리언 -> 문자열
String(true) // true
String(false) // false
```

**32.2 length 프로퍼티**

- length 프로퍼티는 문자열의 문자 개수를 반환, 배열과 마찬가지로 length프로퍼티를 갖으며 유사배열 객체임

```jsx
'hello'.length // 5
'안녕하세요!'.length // 6
```

**32.3 String 메서드**

- 배열에는 원본 배열을 직접 변경하는 메서드와 원본 배열을 직접 변경하지 않고 새로운 배열을 생성하여 반환하는 메서드가 있음
- String 객체에는 원본 객체를 직접 변경하는 메서드는 존재하지 않음 ⇒ 변경 불가능한 원시 값이기 때문에 String래퍼 객체도 읽기 전용 객체로 제공됨

**32.3.1 String.prototype.indexOf**

- indexOf 메서드는 대상 문자열에서 인수로 전달받은 문자열을 검색하여 첫 번째 인덱스를 반환함, 실패하면 -1반환

```jsx
const str = 'Hello World'

// 문자열 str에서 l을 검색하여 첫 번째 인덱스를 반환함
str.indexOf('l') //2

// 문자열 str에서 'or'를 검색하여 첫번째 인덱스를 반환
str.indexOf('or') // 7

// 문자열 str에서 'x'를 검색하여 첫 번째 인덱스를 반환 실패하면 -1 반환
str.indexOf('x') // -1

// 문자열 str의 인덱스 3부터 'l'을 검색하여 첫 번째 인덱스를 반환
str.indexOf('l', 3) // 3

================응용===============
if (str.indexOf('Hello') !== -1) {
		// 문자열 str에 'Hello'가 포함되어 있는 경우에 처리할 내용
}

// ES6에서 도입된 String.prototype.includes 메서드를 사용하면 가독성이 더 좋음
if(str.includes('Hello')) {
	// 문자열 str에 'Hello'가 포함되어 있는 경우에 처리할 내용
}
```

**32.3.2 String.prototype.search**

- 대상 문자열에서 인수로 전달받은 정규 표현식과 매치하는 문자열을 검색하여 일치하는 문자열의 인덱스를 반환함, 검색에 실패하면 -1을 반환

```jsx
const str = 'Hello world';

str.search(/o/) // 4
str.search(/x/) // -1
```

**32.3.3 String.prototype.includes**

- 대상 문자열에 인수로 전달받은 문자열이 포함되어 있는지 확인하여 불리언 타입으로 반환

```jsx
const str = 'Hello world';

str.includes('Hello'); // true
str.includes(''); // true
str.includes('x'); // false
str.includes(); // false

// 인덱스 3부터 'l'이 포함되어 있는지 확인
str.includes('l', 3) // true
str.includes('H', 3) // false
```

**32.3.4 String.prototype.startWith**

- 대상 문자열이 인수로 전달받은 문자열로 시작하는 확인하여 그 결과를 불리언 타입으로 반환

```jsx
const str = 'Hello world';

// 'He'로 시작하는지 확인
str.startsWith('He') // true
str.startsWith('x') // false

// 인덱스 5부터 시작하는 문자열이 ' '로 시작하는지 확인
str.startsWith(' ', 5) // true
```

**32.3.5 String.prototype.endsWith**

- 대상 문자열이 인수로 전달받은 문자열로 끝나는지 확인하여 그 결과를 불리언 타입으로 반환

```jsx
const str = 'Hello world';

// 'ld'로 끝나는지 확인
str.endsWith('ld') // true
str.endsWith('x') // false

// 처음부터 5자리까지('Hello')가 'lo'로 끝나는지 확인
str.endsWith('lo', 5) // true
```

**32.3.6 String.prototype.chartAt**

- 대상 문자열에서 인수로 전달받은 인덱스에 위치한 문자를 검색하여 반환

```jsx
const str = 'Hello';

for(let i = 0; i < str.length; i++) {
		console.log(str.charAt(i)) // H e l l o
}

// 인덱스가 문자열의 범위를 벗어난 경우 빈 문자열을 반환
str.charAt(5) // ''
```

**32.3.7 Stirng.prototype.substring**

- 대상 문자열에서 첫 번째 인수로 전달받은 인덱스에 위치하는 문자부터 두 번째 인수로 전달받은 인덱스에 위치하는 문자의 바로 이전 문자까지의 부분 문자열을 반환함

```jsx
const str = 'Hello World';

// 인덱스 1부터 인덱스 4이전까지의 부분 문자열을 반환함
str.substring(1, 4) // 'ell'

// 인덱스 1부터 마지막 문자까지 부분 문자열을 반환
str.substring(1) // 'ello World'

// 첫 번째 인수 > 두 번째 인수인 경우 두 인수는 교환됨
str.substring(4, 1) // 'ell'

// 인수 < 0 또는 NaN인 경우 0으로 취급됨
str.substring(-2) // 'Hello World'

// 인수 > 문자열의 길이인 경우 인수는 문자열의 길이로 취급됨
str.substring(1, 100) // 'ello World'
str.substring(20) // ''
```

- indexOf 메서드와 함께 사용하면 특정 문자열을 기준으로 앞뒤에 위치한 부분 문자열을 취득 할 수 있음

```jsx
const str = 'Hello World';

// 스페이스를 기준으로 앞에 있는 부분 문자열 취득
str.substring(0, str.indexOf(' ')) // 'Hello'

// 스페이스를 기준으로 뒤에 있는 부분 문자열 취득
str.substring(0, str.indexOf(' ') + 1, str.length) // 'World'
```

**32.3.8 String.prototype.slice**

- substring 메서드와 동일하게 동작함 단, slice 메서드는 음수인 인수를 전달할 수 있음
- 음수인 인수를 전달하면 대상 문자열의 가장 뒤에서부터 시작하여 문자열을 잘라내어 반환함

```jsx
const str = 'Hello World';

// substring과 동일하게 동작
str.substring(0, 5) // 'Hello'
str.slice(0, 5) // 'Hello'

//인덱스가 2인 문자부터 마지막 문자까지
str.substring(2) // 'llo World'
str.slice(2) // 'llo World'

// 인수 < 0 또는 NaN인 경우 0으로 취급됨
str.substring(-5) // 'Hello World'
// 음수인 인수를 전달할 수 있음, 뒤에서 5자리를 잘라내어 반환
str.slice(-5) // 'World'
```

**32.3.9 String.prototype.toUpperCase**

```jsx
const str = 'Hello World';

// 문자열을 대문자로 반환
str.toUpperCase(); // 'HELLO WORLD'
```

**32.3.10 String.prototype.toLowerCase**

```jsx
const str = 'Hello World';

// 문자열을 소문자로 반환
str.toUpperCase(); // 'hello world'
```

**32.3.11 String.prototype.trim**

```jsx
const str = '   foo   ';

// 공백 제거
str.trim(); // 'foo'

// 앞에 있는 공백 제거
str.trimStart() // 'foo   ';
// 뒤에 있는 공백 제거
str.trimEnd() // '   foo';
```

**32.3.12 String.prototype.repeat**

- 대상 문자열을 인수로 전달받은 정수만큼 반복해 연결한 새로운 문자열을 반환함
- 인수로 전달받은 정수가 0이면 빈 문자열을 반환하고, 음수이면 RangeError를 발생시킴, 인수를 생략하면 기본값 0

```jsx
const str = 'abc'

str.repeat(); // ''
str.repeat(0); // ''
str.repeat(1); // 'abc'
str.repeat(2.5); // 'abcabc' 2로됨
str.repeat(-1); // Uncaught RangeError
```

**32.3.13 String.prototype.replace**

- 대상 문자열에서 첫 번째 인수로 전달받은 문자열 또는 정규표현식을 검색하여 두 번째 인수로 전달한 문자열로 치환한 문자열을 반환함

```jsx
let str = 'Hello World';

// str에서 첫 번째 인수 World를 검색하여 두 번째 인수 Lee로 치환함
str.replace('World', 'Lee') // 'Hello Lee'

// 특수한 교체 패턴을 사용할 수 있음
str.replace('World', '<strong>$&<strong>') // 'Hello <strong>World<strong>'

// 대소문자를 구별하지 않고 전역 검색함
str.replace(/hello/gi, 'Lee') // 'Lee World'
```

**32.3.14 String.prototype.split**

- 대상 문자열에서 첫 번째 인수로 전달한 문자열 또는 정규 표현식을 검색하여 문자열을 구분한 후 분리된 각 문자열로 이루어진 배열을 반환함
- 인수로 빈 문자열을 전달하면 각 문자를 모두 분리하고, 인수를 생략하면 단일 요소로 하는 배열을 반환

```jsx
const str = 'How are you doing?'

str.split(' ') // ['How', 'are', 'you', 'doing?']

// \s는 여러가지 공백문자를 의미함
str.split(/\s/) // ['How', 'are', 'you', 'doing?']

// 인수로 빈 문자열을 전달하면 각 문자를 모두 분리
str.split('')
// ['H', 'o', 'w', ' ', 'a', 'r', 'e', ' ', 'y', 'o', 'u', ' ', 'd', 'o', 'i', 'n', 'g', '?']

// 인수를 생략하면 단일 요소로 배열 반환
str.split() // ['How are you doing?']

// 공백으로 구분하여 배열로 반환, 배열의 길이는 3
str.split(' ', 3) // ['How', 'are', 'you']
```

- reverse, join 메서드와 함께 사용하면 문자열을 역순으로 뒤집을 수 있음

```jsx
// 인수로 전달받은 문자열을 역순으로 뒤집는다
function reverseString(str) {
		return str.split('').reverse().join('')
}

reverseString('Hello World!') // '!dlroW olleH'
```