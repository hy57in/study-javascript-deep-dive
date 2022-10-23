# 19장 프로토타입

**19.1 객체지향 프로그래밍**

- 객체지향 프로그래밍은 프로그램을 명령어 또는 함수의 목록으로 보는 전통적인 명령향 프로그래밍의 절차지향적 관점에서 벗어나 여러 개의 독립적 단위, 즉 객체의 집합으로 프로그램을 표현하려는 프로그래밍 패러다임을 말함
- 실체는 특징이나 성질을 나타내는 **속성**을 가지고 있고 이를 통해 실체를 인식하거나 구별할 수 있음
- 다양한 속성 중 프로그램에 필요한 속성만 간추려 내어 표현하는 것을 **추상화**라 함

```jsx
// name과 address 속성을 갖는 객체
const person = {
	name: "Lee",
	address: "Seoul"
};

console.log(person); // {name: "Lee", address: "Seoul"}
```

→ 위 예제와 같이 속성을 통해 여러 개의 값을 하나의 단위로 구성한 복합적인 자료구조를 객체라 함

```jsx
const circle = {
	radius: 5, // 반지름 : 원의 상태를 나타내는 데이터임

	// 원의 지름, 둘레 등을 구하는 것은 동작임
	getDiameter() {
		return 2 * this.radius;
	},

	// 원의 둘레 : 2파이r
	getPerimeter() {
		return 2 * Meth.PI * this.radius;
	} 
};
```

→ 객체지향 프로그래밍은 객체의 상태를 나타내는 데이터와 상태 데이터를 조작할 수 있는 동작을 하나의 논리적인 단위로 묶어 생각함 ⇒ 따라서 객체는 상태 데이터와 동작을 하나의 논리적인 단위로 묶은 복합적인 자료구조라고 할 수 있음 ⇒ 이때 객체의 상태 데이터를 프로퍼티 / 동작을 메서드라 부름

**19.2 상속과 프로토타입**

- **상속**은 객체지향 프로그래밍의 핵심 개념으로 어떤 객체의 프로퍼티 또는 메서드를 다른 객체가 상속받아 그대로 사용할 수 있는 것을 말함
- 자바스크립트는 프로토타입을 기반으로 상속을 구현하여 불필요한 중복을 제거하는데 **중복을 제거하는 방법은 기존의 코드를 적극적으로 재사용**하는 것임 ⇒ 코드 재사용은 개발 비용을 현저히 줄일 수 있는 잠재력이 있으므로 매우 중요함

```jsx
// 생성자 함수
function Circle(radius){
  this.radius = radius;
  this.getArea = function () {
    // Math.PI는 원주율을 나타내는 상수
    return Math.PI * this.radius **2
  }
}

const circle1 = new Circle(1); // 반지름이 1인 인스턴스 생성
const circle2 = new Circle(2); // 반지름이 2인 인스턴스 생성

// Circle생성자 함수는 인스턴스를 생성할 때마다 동일한 동작을 하는 
// getArea 메소드를 중복 생성하고 모든 인스턴스가 중복 소유 
// 단 하나만 생성하여 모든 인스턴스가 공유하는 것이 바람직
// 메모리를 불필요하게 낭비하고 퍼포먼스에도 악영향을 줌
console.log(circle1.getArea === circle2.getArea); // false

console.log(circle1.getArea())
console.log(circle2.getArea())
```

→ 위 예제의 생성자 함수는 동일한 프로퍼티(메서드포함) 구조를 갖는 객체를 여러 개 생성할 때 유용하지만 문제가 있음

→ Circle 생성자 함수가 생성하는 모든 객체는 radius 프로퍼티와 getArea메서드를 갖는데 radius 프로퍼티 값은 일반적으로 인스턴스마다 다른데 getArea 메서드는 모든 인스턴스가 동일한 내용의 메서드를 사용하므로 단 하나만 생성하여 모든 인스턴스가 공유해서 사용하는 것이 바람직함 

⇒ 하지만 위의 예제는 인스턴스를 생성할 때마다 getArea 메서드를 중복 생성하고 모든 인스턴스가 중복 소유 하기 때문에 문제가 있음 

- 동일한 메서드를 중복 소유하는 것은 메모리를 불필요하게 낭비하고 인스턴스를 생성할 때마다 메서드를 생성하므로 퍼포먼스에도 악영향을 줌
- 이를 상속을 통해 불필요한 중복을 제거 할 수 있음
- 자바스크립트는 프로토타입을 기반으로 상속을 구현함

```jsx
function Circle(radius) {
  this.radius = radius;
}

// Circle 생성자 함수가 생성한 모든 인스턴스를 getArea 메소드를 
// 공유해서 사용할 있도록 프로토타입에 추가
// 프로토타입은 Circle 생성자 함수의 prototype 프로퍼티에 바인딩 되어 있음
Circle.prototype.getArea = function () {
  return Math.PI * this.radius ** 2;
};

// 인스턴스 생성
const circle1 = new Circle(1);
const circle2 = new Circle(2);

// Circle 생성자 함수가 생성한 모든 인스턴스는 부모 객체의 역할을 하는
// 프로토 타입 Circle.prototype으로부터 getArea 메소드를 상속 받음
// 즉, Circle생성자 함수가 생성하는 모든 인스턴스는 하나의 getArea 메소드를 공유
console.log(circle1.getArea === circle2.getArea); // true

console.log(circle1.getArea())
console.log(circle2.getArea())
```

→ Circle 생성자 함수가 생성한 모든 인스턴스는 자신의 프로토타입, 즉 상위 객체 역할을 하는 Circle.prototype의 모든 프로퍼티와 메서드를 상속 받음

→ getArea 메서드는 단 하나만 생성되어 프로포타입은 Circle.prototype의 메서드로 할당되어 있어 Circle 생성자 함수가 생성하는 모든 인스턴스는 getArea메서드를 상속받아 사용할 수 있음

→ 상속은 코드의 재사용이란 관점에서 매우 유용함 ⇒ 생성자 함수가 생성할 모든 인스턴스가 공통적으로 사용 할 프로퍼티나 메서드를 프로토타입에 미리 구현해 두면 별도의 구현없이 상위 객체인 프로토타입의 자산을 공유하여 사용 할 수 있음

**19.3 프로토타입 객체**

- 프로토타입 객체란 객체지향 프로그래밍의 근간을 이루는 객체 간 상속을 구현하기 위해 사용 됨
    
    ⇒ 어떤 객체의 상위 객체의 역할을 하는 객체로서 다른 객체에 공유 프로퍼티를 제공함 
    
    ⇒ 프로토타입을 상속받은 하위 객체는 상위 객체의 프로퍼티를 자신의 프로퍼티처럼 자유롭게 사용할 수 있음
    
- 모든 객체는 [[Prototype]]이라는 내부 슬롯을 가지며 객체 생성 방식에 따라 결정되고 저장됨
- 모든 객체는 하나의 프로토타입을 갖으며 내부 슬롯에는 직접 접근할 수 없지만 __proto__접근자 프로퍼티를 통해 자신의 내부 슬롯이 가리키는 프로토타입에 간접적으로 접근 할 수 있음

**19.3.1** _ _ ***proto*_ _ 접근자 프로퍼티**

- 모든 객체는 __ proto __ 접근자 프로퍼티를 통해 자신의 프로터타입 내부 슬롯에 간접 접근 됨

```jsx
const person = {name : "Lee"};
```

![Untitled](19%E1%84%8C%E1%85%A1%E1%86%BC%20%E1%84%91%E1%85%B3%E1%84%85%E1%85%A9%E1%84%90%E1%85%A9%E1%84%90%E1%85%A1%E1%84%8B%E1%85%B5%E1%86%B8%20d4f010db3da84b41b1e5fbeb2e87f4a7/Untitled.png)

_**_proto__는 접근자 프로퍼티다**

→ 내부 슬롯은 프로퍼티가 아니기 때문에 내부 슬롯과 내부 메서드에 직접적으로 접근 및 호출할 수 없음 단, 일부 내부 슬롯과 내부 메서드에 한하여 간접적으로 접근할 수 있는 수단을 제공함 

⇒ __proto__ 접근자 프로퍼티를 통해 간접적으로 [[prototype]]내부 슬롯의 값, 즉 프로토타입에 접근 할 수 있음

⇒ __proto__는 getter/setter 함수라고 부르는 접근자 함수를 통해 내부 슬롯의 값인 프로토타입을 취득하거나 할당함 

```jsx
const obj = {};
const parent= { x: 1 }

// getter 함수인 get__proto__가 호출되어 obj 객체의 프로토타입 취득
obj.__proto__;

// setter 하수인 set__proto__가 호출되어 프로토타입을 교체
obj.__proto__ = parent;

console.log(obj.x) // 1
```

**__proto__ 접근자 프로퍼티는 상속을 통해 사용된다**

- 객체가 직접 소유하는 프로퍼티가 아니라 Object.prototype의 프로퍼티로 모든 객체는 상속을 통해 Object.prototype.__proto__ 접근자 프로퍼티를 사용할 수 있음

```jsx
const person = {name : "Lee"};

// person 객체는 __proto__ 프로퍼티를 소유하지 않음
console.log(person.hasOwnProperty('__proto__')); false

// __proto__ 프로퍼티는 모든 객체의 프로토타입 객체인 Object.prototype의 접근자 프로퍼티임
console.log(Object.getOwnPropertyDescriptor(Object.prototype, '__proto__'))
// {enumerable: false, configurable: true, get: ƒ, set: ƒ}

// 모든 객체는  Object.prototype의 접근자 프로퍼티 __proto__를 상속받아 사용할 수 있음
console.log({}.__proto__ === Object.prototype); // true
```

_**_proto__ 접근자 프로퍼티를 통해 프로토타입에 접근하는 이유**

- 상호 참조에 의해 프로토타입 체인이 생성되는 것을 방지하기 위해서임

```jsx
const parent = {};
const child = {};

// child의 프로토타입을 parent로 설정
child.__proto__ = parent;

// parent의 프로토타입을 child로 설정
parent.__proto__ = child; // Uncaught TypeError: Cyclic __proto__ value
```

→ 서로가 자신의 프로토타입이 되는 비정상적인 프로토타입 체인이 만들어지기 때문에 에러를 발생 

→ 프로토타입 체인은 단방향 링크드 리스트로 구현되어야 하는데 위 예시처럼 순환 참조하는 체인이 만들어지면 체인 종점이 존재하지 않기 때문에 무한 루프에 빠지게 됨 ⇒ 따라서 __proto__접근자 프로퍼티를 통해 접근하고 교체하도록 구현되어 있음

_**_proto__ 접근자 프로퍼티를 코드 내에서 직접 사용하는 것은 권장하지 않음**

- 모든 객체가 __proto__ 접근자 프로퍼티를 사용할 수 있는 것이 아니기 때문에 직접 사용하는 것은 권장하지 않음

```jsx
// obj는 프로토타입 체인의 종점이라 Object.__proto__를 상속 받을 수 없음
const obj = Object.create(null);

// obj는 Object.__proto__를 상속받을 수 없음
console.log(obj.__proto__) // undefined

// 따라서 __proto__ 보다 Object.getPrototyleOf 메서드를 사용하는 편이 좋음
console.log(Object.getPrototypeOf(obj)) // null
```

```jsx
const obj = {};
const parent= { x: 1 }

// obj 객체의 프로토타입을 취득
Object.getPrototypeOf(obj);

// obj 객체의 프로토타입을 교체
Object.setPrototypeOf(obj, parent);

console.log(obj.x) // 1
```

→ __ proto __ 접근자대신 프로토타입의 참조를 취득할 경우에는 object.getPrototypeof 메소드 사용

→ 프로토타입을 교체하고 싶은 경우에는 object.setPrototypeof 메소드를 사용할 것을 권장

**19.3.2 함수 객체의 prototype 프로퍼티**

- 함수 객체만의 소유하는 prototype 프로퍼티는 생성자 함수가 생성할 인스턴스의 프로토타입을 가리킴
- prototype 프로퍼티는 생성자 함수가 생성할 객체의 프로토타입을 가리킴 따라서 생성자 함수로서 호출할 수 없는 함수 즉 non-constructor인 화살표 함수와 메서드 축약 표현으로 정의한 메서드는 prototype프로퍼티를 소유하지 않으며 프로토타입도 생성하지 않음

```jsx
// 화살표 함수는 non-constructor
const Person = name => {
	this.name = name;
}

// ES6 메서드 축약 표현으로 정의한 메서드는 non-constructor
const obj = {
	foo() {}
}

// non-constructor는 prototype 프로퍼티를 소유하지 않으며 프로토타입을 생성하지 않음
console.log(Person.hasOwnProperty('prototype')) // false
console.log(Person.prototype) // undefined

console.log(obj.foo.hasOwnProperty('prototype')) // false
console.log(obj.foo.prototype) // undefined
```

→ 생성자 함수로 호출하기 위해 정의하지 않은 일반함수도 prototype 프로퍼티를 소유하지만 객체를 생성하지 않는 일반 함수의 prototype프로퍼티는 아무런 의미가 없음

→ 모든 객체가 가지고 있는 **proto**접근자 프로퍼티와 함수 객체만이 가지고 있는 prototype프로퍼티는 결국 동일한 프로토타입을 가리킴 ⇒ 하지만 사용하는 주체가 다름

```jsx
function Person(name) {
	this.name = name;
}

const me = new Person('Lee');

// 결국 Person.prototype과 me.__proto__는 동일한 프로토타입을 가리킴
console.log(Person.prototype === me.__proto__); // true
```

**19.3.3 프로토타입의 constructor 프로퍼티와 생성자 함수**

- 모든 프로토타입은 constructor 프로퍼티를 갖음 이 constructor 프로퍼티는 prototype 프로퍼티로 자신이 참조하고 있는 생성자 함수를 가리킴

```jsx
function Person(name) {
	this.name = name;
}

const me = new Person("Lee")

//me 객체의 생성자 함수는 person
console.log(me.constructor === Person); //true
```

→ 위 예제에서 Person 생성자 함수는 me 객체를 생성했는데 이 객체는 프로토타입의 constructor 프로퍼티를 통해 생성자 함수와 연결됨 ⇒ 따라서 me객체는 프로토타입인 Person.prototype의 constructor 프로퍼티를 상속받아 사용할 수 있음

**19.4 리터럴 표기법에 의해 생성된 객체의 생성자 함수와 프로토타입**

- 생성자 함수에 의해 생성된 인스턴스는 프로토타입의 constructor 프로퍼티에 의해 생성자 함수와 연결되며 이때 constructor 프로퍼티가 가르키는 생성자 함수는 인스턴스를 생성한 생성자 함수이다

```jsx
// obj 객체를 생성한 생성자 함수는 Object임
const obj = new Object();
console.log(obj.constructor === Object); // true

// add 함수 객체를 생성한 생성자 함수는 function이다
const add = new Fuctnion('a', 'b', 'return a+b');
console.log(add.constructor === Fuction); // true

// 생성자 함수
function Person(name){
	this.name = name; 
}

// me 객체를 생성한 생성자 함수는 Person이다
const me = new Person('Lee');
console.log(me.constructor === Person); // true
```

→ 리터럴 표기법에 의한 객체 생성 방식과 같이 명시적으로 new 연산자와 함께 생성자 함수를 호출하여 인스턴스를 생성하지 않는 객체 생성 방식도 있음

```jsx
// 객체 리터럴
const obj = {};

// 함수 리터럴
const add = function (a, b) {return a+b};

// 배열 리터럴
const arr = [1,2,3]

// 정규 표현식 리터럴
const regexp = /is/ig;
```

→ 리터럴 표기법에 의해 생성된 객체는 생성자 함수에 의해 생성된 객체는 아니지만 생성자 함수로 생성한 객체와 본질적인 면에서 큰 차이는 없음 ⇒ 따라서 프로토타입의 constructor프로퍼티를 통해 연결되어 있는 생성자 함수의 리터럴 표기법으로 생성한 객체를 생성한 생성자 함수로 생각해도 됨

**19.5 프로토타입의 생성 시점**

- 객체는 리터럴 표기법 또는 생성자 함수에 의해 생성되므로 결국 모든 객체는 생성자 함수와 연결되어 있음 ⇒ ****프로토타입은 생성자 함수가 생성되는 시점에 더불어 생성됨

**19.5.1 사용자 정의 생성자 함수와 프로토타입 생성 시점**

- 일반 함수로 정의한 함수 객체는 new 연산자와 함께 생성자 함수로서 호출할 수 있지만 화살표 함수나 ES6의 메서드 축약 표현으로 정의한 함수  non-constructor함수는 프로토타입이 생성되지 않음
- 생성자 함수로서 호출할 수 있는 함수 즉, constructor는 함수 정의가 평가되어 함수 객체를 생성하는 시점에 프로토타입도 더불어 생성됨

**19.5.2 빌트인 생성자 함수와 프로토타입 생성 시점**

- Object, String, Number, Function, Array, RegExp, Date, Promise 등과 같은 빌트인 생성자 함수도 빌트인 생성자 함수가 생성되는 시점에 프로토타입이 생성되며 모든 빌트인 생성자 함수는 전역 객체가 생성되는 시점에 생성됨
- 생성된 프로토타입은 빌트인 생성자 함수의 prototype프로퍼티에 바인딩 됨
- 객체가 생성되기 이전에 생성자 함수와 프로토타입은 이미 객체화 되어 존재하며 이후 생성자 함수 또는 리터럴 표기법으로 객체를 생성하면 프로토타입은 생성된 객체의 [[Prototype]] 내부 슬롯에 할당됨

**19.6 객체 생성 방식과 프로토타입의 결정** 

- 객체는 다음과 같이 다양한 생성 방법이 있음
    
    **객체리터럴 / Object 생성자 함수 / 생성자 함수 / Object.create메서드 / 클래스**
    
- 추상 연산에 의해 생성된다는 공통점이 있음
    
     → 추상 연산은 필수적으로 자신이 생성할 객체의 프로토타입을 인수로 전달받고 자신이 생성할 객체에 추가할 프로퍼티 목록을 옵션으로 전달할 수 있음 
    
    → 추상연산은 빈 객체를 생성한 후, 객체에 추가할 프로퍼티 목록이 인수로 전달될 경우 프로퍼티를 객체에 추가함 ⇒ 그리고 인수로 전달받은 프로토타입을 자신이 생성한 객체의 [[prototype]]내부 슬롯에 할당한 다음 생성한 객체를 반환함
    
    → 즉, 프로토타입은 추상연산에 전달되는 인수에 의해 결정되며 이 인수는 객체가 생성되는 시점에 객체 생성 방식에 의해 결정됨
    

**19.6.1 객체 리터럴에 의해 생성된 객체의 프로토타입**

- 자바스크립트 엔진은 객체 리터럴을 평가하여 객체를 생성할 때 추상 연산을 호출하는데 이때 추상 연산에 전달되는 프로토타입은 Object.prototype이다 ⇒ 즉, 객체 리터럴에 의해 생성되는 객체의 프로토타입은 Object.prototype임

```jsx
const obj = { x: 1}

console.log(obj.constructor === Object); // true
console.log(obj.hasOwnProterty('x')) // true
```

**19.6.2 Object 생성자 함수에 의해 생성된 객체의 프로토타입**

- object 생성자 함수를 인수 없이 호출하면 빈 객체가 생성되는데 생성자 함수를 호출하면 객체 리터럴과 마찬가지로 추상연산이 호출됨
- Object 생성자 함수에 의해 생성된 obj객체는 Object.prototype을 프로토 타입으로 갖게 되며 이로써 Object.prototype을 상속받음

**19.7 프로토타입 체인**

```jsx
fuctnion Person(name) {
	this.name = name;
}

// 프로토타입 메서드
Person.prototype.sayHello = function () {
	console.log(`my name is ${this.name}`)
}

const me = new Person ('Lee')

// hasOwnProperty는 Object.prototype의 메서드임
console.log(me.hasOwnProperty('name')); // true

object.getPrototypeOf(me) === Person.prototype // true
object.getPrototypeOf(Person.prototype) === Object.prototype // true
```

→ Person 생성자 함수에 의해 생성된 me 객체는 Object.prototype의 메서드인 hasOwnProperty를 호출할 수 있으며 이것은 me객체가 Person.prototype뿐만 아니라 Object.prototype도 상속받았다는 것을 의미함 

⇒ me객체의 프로토타입은 Person.prototype임

→ 자바스크립트는 객체의 프로퍼티에 접근하려고 할 때 해당 객체에 접근하려는 프로퍼티가 없다면 [[Prototype]] 내부 슬롯의 참초를 따라 자신의 부모 역할을 하는 프로토타입의 프로퍼티를 순차적으로 검색함 

⇒ 이를 프로토타입 체인이라 함 

⇒ 프로토타입 체인은 자바스크립트가 객체지향 프로그래밍의 상속을 구현하는 메커니즘임

```jsx
// hasOwnPropertye은 Object.prototype의 메서드임
// me 객체는 프로토타입 체인을 따라hasOwnPropertye 메서드를 검색하여 사용 함 
me.hasOwnProperty('name') // true
```

→ 위의 메소드를 호출하면 다음과 같은 과정을 거쳐 메서드를 검색함

1. hasOwnProperty 메서드를 호출한 me객체에서 해당 메서드를 검색 후에 me객체에는 해당 메서드가 없기 때문에 프로토타입 체인을 따라 [[prototype]]내부 슬롯에 바인딩되어 있는 프로토타입으로 이동하여 해당 메서드를 검색함
2. Person.prototype 에도 해당 메서드가 없으므로 프로토타입 체인을 따라 Object.prototype으로 다시 이동하여 해당 메서드를 검색함
3. Object.prototype에는 해당 메서드가 존재하기 때문에 Object.prototype.hasOwnProperty 메서드를 호출 하게 되고 this에는 me 객체가 바인딩 됨

→ 프로토타입 체인의 최상위에 위치하는 객체는 언제나  Object.prototype이므로 모든 객체는  Object.prototype를 상속받는다 

⇒  Object.prototype을 프로토타입 체인의 종점이라 하며  Object.prototype의 프로토타입 내부슬롯의 값은 null이다

⇒ 따라서 프로토타입 체인은 상속과 프로퍼티 검색을 위한 메커니즘이라고 할 수 있음

⇒ 프로퍼티가 아닌 식별자는 스코프 체인에서 검색하는데 따라서 스코프 체인은 식별자 검색을 위한 메커니즘이라고 할 수 있음

→ 상위 예제의 경우 먼저 스코프 체인에서 me식별자를 검색 후 me 객체의 프로토타입 체인에서 hasOwnProperty 메서드를 검색하는데 이처럼 스코프 체인과 프로토타입 체인은 서로 협력하여 식별자와 프로퍼티를 검색하는데 사용됨

**19.8 오버라이팅과 프로퍼티 섀도잉**

```jsx
const Person = (function () {
	function Person(name) {
		this.name = name;
	}

	// 프로토타입 메서드
	Person.prototype.sayHello = function () {
		console.log(`my name is ${this.name}`);
	}
	
	return Person;

}());

const me = new Person('Lee');

me.sayHello = function () {
		console.log(`hey my name is ${this.name}`)
};

me.sayHello(); //hey my name is Lee
```

→ 프로토타입 프로퍼티와 같은 이름의 프로퍼티를 인스턴스에 추가하면 프로토타입 체인을 따라 프로타입 프로퍼티를 검색하여 프로토타입 프로퍼티를 덮어쓰는 것이 아니라 인스턴스 프로퍼티로 추가함 

⇒ 이때 인스턴스 메서드 sayHello는 프로토타입 메서드 sayHello를 오버라이딩했고 프로토타입 메서드 sayHello는 가려진다(상위 클래스가 가지고 있는 메서드를 하위 클래스가 재정의 하여 사용하는 방식을 오버라이딩이라 함)

⇒ 이처럼 상속 관계에 의해 포로퍼티가 가려지는 현상을 프로퍼티 섀도잉이라 함

→ 단, 하위 객체를 통해 프로토타입의 프로퍼티를 변경 또는 삭제 하는 것은 불가능 함

⇒ 하위 객체를 통해 프로토타입에 get은 허용되나 set은 허용되지 않음

**19.8 프로토타입의 교체**

**19.9.1 생성자 함수에 의한 프로토타입의 교체**

```jsx
const Person = (function () {
	function Person(name) {
		this.name = name;
	}

	// 1. 생성자 함수의 prototype 프로퍼티를 통해 프로토타입을 교체
	Person.prototype = {
		constructor: Person, // constructor 프로퍼티와 생성자 함수 간의 연결을 설정
		sayHello() {
			console.log(`my name is ${this.name}`);
		}
	}
	return Person;
}());

const me = new Person('Lee');

// 프로토타입을 교체하면 constructor 프로퍼티와 생성자 함수 간의 연결이 파괴됨
console.log(me.constructor === Person); // false;
// 프로토타입 체인을 따라 Object.prototype의 constructor 프로퍼티가 검색됨
console.log(me.constructor === Object); // true;
```

**19.9.2 인스턴스에 의한 프로토타입의 교체**

- 프로토타입은 인스턴스의 __proto__접근자 프로퍼티를 통해 접근할 수 있으며 따라서 프로토타입을 교체 할 수 있음

```jsx
function Person(name) {
	this.name = name;
}

const me = new Person('Lee');

const parent = {
	sayHello() {
		console.log(`my name is ${this.name}`)
	}
}

// 1. me 객체의 프로토타입을 parent 객체로 교체한다
Object.setPrototypeOf(me, parent);
// 위 코드는 아래의 코드와 동일하게 동작함
// me.__proto__ = parent

me.sayHello(); // my name is Lee

// 프로토타입을 교체하면 constructor 프로퍼티와 생성자 함수 간의 연결이 파괴됨
console.log(me.constructor === Person); // false;
// 프로토타입 체인을 따라 Object.prototype의 constructor 프로퍼티가 검색됨
console.log(me.constructor === Object); // true;
```

→ 프로토타입 교체를 통해 객체 간의 상속 관계를 동적으로 변경하는 것은 꽤나 번거로운 일이라 프로토타입은 직접 교체하지 않는 것이 좋음

**19.10 instanceof 연산자**

- instanceof 연산자는 생성자 함수의 prototype에 바인딩된 객체가 프로토타입 체인 상에 존재하는지 확인함
- constructor 프로퍼티와 생성자 함수 간의 연결이 파괴되어도 instanceof는 아무 영향도 받지 않음

**19.11 직접 상속**

- 자바스크립트의 모든 객체는 자신의 부모 역할을 담당하는 객체와 연결되어 있음
- 이것은 마치 객체 지향의 상속 개념과 같이 부모 객체의 프로퍼티 또는 메소드를 상속받아 사용할 수 있게 함
- 이러한 부모 객체를 Prototype(프로토타입) 객체 또는 줄여서 Prototype(프로토타입)이라 함

**19.11.1 Object.create에 의한 직접 상속**

- 첫 번째 매개변수엔 (생성할 객체의) 프로토타입으로 지정할 객체
- 두 번째 매개변수(옵셔널)엔 (생성할 객체의) 프로퍼티를 갖는 객체
- 위 프로퍼티를 갖는 객체는 프로퍼티 키와 프로퍼티 디스크립터 객체를 묶은 객체를 의미함
- 장점
    
    → new 없이도 객체 생성 가능
    
    → 프로토타입 지정하면서 객체 만들 수 있음
    
    → 객체 리터럴로 만든 객체를 상속받을 수 있음
    

**19.11.2 객체 리터럴 내부에서 proto에 의한 직접 상속**

- 객체 리터럴 내부에서 `__proto__`접근자 프로퍼티 이용해 직접 상속을 구현할 수 있음

**19.12 정적 프로퍼티/메서드**

- **정적 프로퍼티/메소드**: 생성자 함수로 인스턴스를 만들지 않아도 참조할 수 있는 프로퍼티/메소드
- 정적 프로퍼티/메소드는 생성자 함수로 만든 인스턴스로 호출할 수 없음
- 정적 프로퍼티/메소드는 인스턴스가 속한 프로퍼티 체인에 속하지 않아 인스턴스로 호출할 수 없음
- 정적 메소드는 인스턴스를 만들지 않아도 호출할 수 있음
- 정적 프로퍼티/메소드와 프로토타입 프로퍼티/메소드를 구분

**19.13 프로퍼티 존재 확인**

**19.13.1 in 연산자** 

- in 연산자는 객체 내에 특정 프로퍼티가 존재 여부를 확인함

```jsx
// key : 프로퍼티 키를 나타내는 문자열
// object : 객체로 평가되는 표현식

key in object

const person = {name: "lee", address: "Seoul"};
console.log('name' in person); // true
console.log('age' in person); // false
console.log('toStrong' in person); // true
```

- ES6에 생긴 Reflect.has()는 in과 동일한 역할을 함

```jsx
const person = {name: "lee"};
console.log(Reflect.has(person, 'name)); // true
```

**19.13.2 Object.prototype.hasOwnProperty 메서드**

- 객체에 프로퍼티가 존재하는지 알 수 있지만 해당 객체의 고유한 프로퍼티일 때만 true를 반환하고 아니면 (프로토타입 프로퍼티이거나) false를 반환함

**19.14 프로퍼티 열거**

**19.14.1 for…in 문**

- 객체의 모든 프로퍼티를 순회하며 열거 하려면 for…in문을 사용함

```jsx
for(변수 in 객체)

const person = {
	name: "lee",
	address: "Seoul"
}

console.log('toString' in Person) // true

// 객체가 상속받은 모든 프로토타입의 프로퍼티를 열거하지만
// toString과 같은 Object.prototype의 프로퍼티가 열거되지 않음
for(const key in person) {
	console.log(key + ': ' + person[key]);
}

// name : lee
// address: Seoul
```

→ 변수선언문에 key 할당

→ 프로토타입 프로퍼티까지 열거 하지만 위 예제의 경우 toString과 같은 Object.proeotype의 프로퍼티가 열거 되지 않음

⇒ toString메서드는 `[[Enumerable]]`이라는 프로퍼티 어트리뷰트가 false이기 때문이다

- for…in문은 객체의 프로토타입 체인 상에 존재하는 모든 프로토타입의 프로퍼티 중에서 프로퍼티 어트리뷰드  `[[Enumerable]]` 값이 true인 프로퍼티를 순회하며 열거 함
- 프로퍼티 키가 symbol인 것은 열거하지 않음
- instance.hasOwnProperty()를 이용하여 객체 자신 고유의 프로퍼티인지 확인할 수 있음
- for ... in 문은 프로퍼티를 열거할 때 순서를 보장해주지 않을 수 있으니 유의해야 함
- 배열을 순회하여 열거하고 싶을 때 for ... in 보다는 일반 for 문, for ... of, 혹은 forEach를 사용하는 것이 권고 됨
- 왜냐하면 배열에 for ... in 을 사용하면 배열 안의 요소 뿐만 아니라 프로퍼티 또한 열거하기 때문임

**19.14.2 Object.keys/values/entries 메서드**

- for ... in 은 객체 자신의 프로퍼티 뿐만 아니라 상속받은 프로퍼티도 열거함
    
    → 따라서 Object.prototype.hasOwnProperty()를 이용해 객체 자신의 프로퍼티인지 확인하는 추가 처리가 필요
    
    → Object.keys 메서드는 객체 자신의 열거 가능한 프로퍼티 키를 배열로 반환함 
    
    ```jsx
    const person = {
    	name: 'lee',
    	address: 'Seoul',
    	__proto__: {age: 20}
    }
    
    console.log(Object.keys(person)) // ['name', 'address']
    console.log(Object.values(person)) // ['lee', 'Seoul']
    console.log(Object.entries(person)) // [Array(2), Array(2)]
    ```

	
    
# 20장 strict mode

**20.1 strict mode란?**

- **암묵적 선언**: strict mode를 사용하지 않을 때 자바스크립트 코드 안에 변수 선언을 하지 않고 할당하더라도 이를 참조할 때 `암묵적 선언`이 되어 reference error를 일으키지 않음

**20.2 strict mode의 적용**

- 전역의 선두 또는 함수 몸체의 선두에  `'use strict';` 를 추가함

```jsx
'use strict';

function foo() {
	x = 10; // Uncaught ReferenceError: x is not defined
}

foo() 
```

**20.3 전역에 strict mode를 적용하는 것은 피하자**

- 왜냐하면 외부 서드 파티 라이브러리가 non-strict mode를 사용할 수도 있기 때문

**20.4 함수 단위로 strict mode를 적용하는 것도 피하자**

- IIFE(즉시 실행 함수) 단위에 적용하는 게 바람직 함
- 모든 함수에 다 strict mode 적용하는 게 번거롭고 놓칠 시에 문제가 발생 하기 때문

**20.5 strict mode가 발생시키는 에러**

**20.5.1 암묵적 전역**

- 선언하지 않은 변수 참조 시 ReferenceError 발생

**20.5.2 변수, 함수, 매개변수의 삭제**

- delete 연산자로 변수, 함수, 매개변수 삭제 시 SyntaxError 발생

**20.5.3 매개변수 이름의 중복**

- 중복된 이름의 매개변수 사용시 SyntaxError 발생

**20.5.4 with 문의 사용**

- with문 사용 시 Syntax Error 발생
- with문은 전달된 객체를 스코프 체인에 추가 함
- with문은 동일한 객체의 프로퍼티를 반복해서 사용할 때 객체 이름을 생략할 수 있어 코드가 간단해 지지만 성능과 가독성이 나빠지는 문제가 있음 ⇒ 사용하지 않는 것이 좋음

**20.6 strict mode 적용에 의한 변화**

**20.6.1 일반 함수의 this**

- strict mode 안에서 함수를 일반 함수로 호출하면 this에 undefined가 바인딩 됨
- 왜냐하면 생성자 함수 제외하곤 this를 사용할 필요가 없기 때문 임

**20.6.2 arguments 객체**

- 함수의 매개변수를 재할당해도 arguments 객체에 반영되지 않음

# 21장 빌트인 객체

**21.1 자바스크립트 객체의 분류**

- 표준 빌트인 객체
    
    → ECMAScript 사양에 정의된 객체
    
    → 전역 객체의 프로퍼티로 사용 가능
    
- 호스트 객체
    
    → ECMAScript 사양에 정의되어 있진 않지만 자바스크립트 실행 환경에서 추가로 제공되는 객체를 말함
    
    - 브라우저 환경 : 클라이언트 사이드 Web API를 호스트 객체로 제공
    - Node.js 환경에서는 Node.js 고유의 API를 호스트 객체로 제공함
- 사용자 정의 객체
    
    → 사용자가 직접 정의한 객체
    

**21.2 표준 빌트인 객체**

- 40여개의 빌트인 객체가 존재
- Math, Reflect, JSON을 제외
- 생성자 함수 객체임
- 프로토타입 메소드와 정적 메소드를 제공 함
- 예를 들어 Number 생성자 함수를 통해 만든 number 객체는 Number.prototype이라는 프로토타입 메소드를 가지고 있음
- 그리고 인스턴스 없이 정적으로 호출할 수 있는 정적 메소드를 제공 함

**21.3 원시값과 래퍼 객체**

- 원시값은 우리가 아는 primitive value를 의미
- **래퍼 객체**는 우리가 원시값을 객체처럼 사용할 때 (ex. str.length) JS 엔진이 암묵적으로 생성한 연관된 객체를 의미 함
- (당연하게도?) 책의 예시처럼 String 생성자 함수를 통해 만들어진 인스턴스는 String.prototype의 프로퍼티를 사용할 수 있음
- 만약 식별자를 객체처럼 사용한다고 하면, 래퍼 객체가 생성되고, 내부 슬롯에 기존 값이 할당 됨
- 래퍼 객체 처리가 완료되면, 식별자가 원시값으로 되돌리고, 래퍼 객체는 가비지 컬렉션 대상이 됨

**21.4 전역 객체 (Global Object)**

- 클라이언트 사이드에선 window라는 전역 객체가 존재
- 서버 사이드에선 global이라는 전역 객체가 존재
- ES11부터 globalThis라는 전역 객체로 통합
- 전역 객체는 최상위 객체로 어느 객체에도 속하지 않음
- 전역 객체는 표준 빌트인 객체, 호스트 객체, 그리고 var로 선언한 전역 변수와 함수를 프로퍼티로 갖음
- var로 선언한 전역 변수, 그리고 선언하지 않은 변수에 값을 할당한 **암묵적 전역**을 프로퍼티로 갖음
- let, const로 선언한 변수는 포함되지 않음
- 자바스크립트 파일이 분리되어 있어도 하나의 전역을 공유 함
- window/global 키워드를 생략하고 프로퍼티를 부를 수 있음

**21.4.1 빌트인 전역 프로퍼티**

1. Infinity
2. NaN
3. undefined

**21.4.2 빌트인 전역 함수**

- 전역 객체의 메소드임

**eval**

- 인자로 들어오는 표현식이나 문을 평가 함
- 표현식이 들어오면 런타임 때 값을 생성 함
- 문이 들어오면 런타임 때 실행 함
- 함수 안에 eval() 함수가 있다고 가정
- 런타임 이전에 함수 내부의 선언문을 모두 실행하고 그 함수의 스코프를 생성 함
- 런타임 때 eval() 함수를 읽고 동적으로 함수의 스코프를 수정 함
- 결국엔 eval() 함수에 인자로 들어온 선언문이 원래 거기 있는 것 마냥 실행 됨
- 엄격 모드가 함수에 적용된 경우, eval() 함수의 자체적인 스코프가 만들어짐
- eval() 함수 내에 let이나 const의 변수가 있으면 자체적으로 엄격 모드가 적용 됨

**isFinite**

- 들어오는 인자가 유한수인지 무한수인지 boolean으로 검증해 줌
- 유한수이면 true 리턴
- 무한수이면 false 리턴
- NaN이면 false 리턴
- null은 참고로 0으로 평가된다

**isNaN**

- 들어오는 인자가 NaN이면 true를 반환
- empty string ('' 혹은 ' ')은 0으로 평가 됨
- undefined는 NaN으로 평가 됨

**parseFloat**

- string 인자를 parse해 floating point number로 바꿔 줌
    
    → 숫자로 된 문자열을 실수로 바꿔 줌
    
    → 여러 숫자가 공백으로 나뉘어진 문자열은 첫 번째 숫자만 실수로 바꿔 줌
    
    → 숫자가 아닌 게 공백으로 나뉘어진 문자열은 NaN을 리턴 함 (맨 첫 토큰만 확인한다고 보면 된다)
    
    → 숫자로된 문자열이 공백이 있으면 공백은 무시하고 실수를 리턴 함
    

**parseInt**

- string 인자를 정수로 바꿔 줌
    
    → 정수, 실수로 된 문자열을 정수로 리턴 함
    
    → 인자로 문자열이 아니고 실제 정수나 실수가 들어오면 문자열로 바꾼 후에 정수로 리턴 함
    

```jsx
parseInt(Number type) => parseInt(String type) => return Int
```

→ `parseInt(string, 기수)`: 정수를 어떤 진수로 바꿀 것인지 정할 수 있음

→ Number.prototype.toString()을 이용해 사용자 지정의 해당 기수로 숫자 인자를 숫자 문자열로 반환할 수 있음

→  parseInt()는 2진수와 8진수 리터럴을 잘 이해하지 못 함

→ 매개변수로 첫 번째 인자로 들어온 문자열의 첫 번째 문자가 지정한 기수로 변환될 수 없으면 NaN을 리턴

```jsx
parseInt('20', 2); //2진수는 0과 1만 가능
```

→ 첫 인자로 들어온 문자열의 첫 번째 문자는 괜찮고 이후 문자가 기수에 해당되지 않으면 괜찮은 문자만 parseInt 함
8]spacing으로 구분된 여러 토큰 인자에서 첫 번째 토큰만 괜찮으면 그것이 평가되어 정수로 리턴된다 (이후에 NaN으로 평가될 토큰이 존재하더라도)

**encodeURI/decodeURI**

- (인코딩 === 이스케이프 처리)
- uri를 인코딩하여 이스케이프 처리하기 위한 함수
- 이스케이프 처리를 하면 어느 시스템에서도 읽을 수 있는 ASCII 문자 세트로 변환 됨
- 이스케이프 처리는 왜 할까?
- URL 문법 형식 표준 (RFC3986)에 따르면 URL은 ASCII 문자 세트로만 구성되어 있어야 함
- 한글, 외국어, (ASCII에 없는) 특수문자는 URL에 포함될 수 없음
- 야기될 수 있는 문제를 방지하기 위해 이스케이프 처리 함

**encodeURIComponent/decodeURIComponent**

- URI는 여러 요소로 구성되어 있음
- protocol | domain | port | path | query | fragment
- 매개변수로 URI 구성 요소를 전달 함
- encodeURIComponent()는 인자를 쿼리 스트링의 일부로 간주하기 때문에 =, ?, &도 인코딩 함
- encodeURI()는 인자를 쿼리 스트링의 일부로 간주하지 않기 때문에 =, ?, &를 인코딩하지 않음

**21.4.3 암묵적 전역**

- **암묵적 전역(implicit global)**: 선언되지 않은 변수가 JS 엔진에 의해 암묵적으로 전역 변수의 프로퍼티로 동적 생성되는 것을 의미
- implicit global -> "implicitly" became a property of the "global" variable
- 우리가 window.setTimeOut()을 window를 생략하고 setTimeOut()으로 할 수 있는 것처럼 JS 엔진도 선언되지 않은 변수를 `window.변수 = OO;` 으로 이해 함
- 선언되지 않은 변수는 결국 변수가 아님
- 전역의 프로퍼티이기 때문에 변수 호이스팅이 일어나지 않음
- 전역 변수는 delete로 삭제 안되지만 선언되지 않은 변수는 단지 프로퍼티이기에 delete 연산자로 삭제가 가능 함
