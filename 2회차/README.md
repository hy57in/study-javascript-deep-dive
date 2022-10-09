# 2회차

## 1. 히든 클래스

객체에서 프로퍼티에 접근할 때 히든 클래스를 사용해서 빠른 속도를 보장한다.

### 1-1. 관련 자료

- [Fast properties in V8](https://v8.dev/blog/fast-properties)
- [V8의 히든 클래스 이야기](https://engineering.linecorp.com/ko/blog/v8-hidden-class)
- [자바스크립트 엔진의 최적화 기법 (2) - Hidden class, Inline Caching](https://meetup.toast.com/posts/78)
- [Breaking the JavaScript Speed Limit with V8](https://www.youtube.com/watch?v=UJPdhx5zTaw)
- [JS의 객체는 hash table이 아닙니다!](https://velog.io/@wongue_shin/JS의-객체는-hash-table이-아닙니다)

## 2. React에서 함수 선언문/표현식 사용하는 이유

### 2-1. 선언문

1. React 공식 문서에서 사용한다.

2. React.FC를 사용하지 않고 Props에 대한 타입만 설정하기 위해 사용한다.
   - 취향 차이: 이렇게 사용하면 Children을 명시적으로 작성하는 것이 귀찮다.
   - 추가 의견: 비교적 최근에 사용한다.

### 2-2. 표현식

1. `useMemo`, `useCallbak`을 사용할 때 화살표 함수가 편리하다.

2. React.FC를 사용할 때 편리하다.
   - 취향 차이: 이렇게 사용하면 Children을 안 써도 암묵적으로 포함되어 있는 것이 신경쓰인다.
   - 추가 의견: 비교적 과거에 사용했다.

### 2-3. 추가 내용

- 회사 컨벤션에 따라 사용한다.
- 호이스팅 때문에 선언식이 문제가 되는 경우가 있다. 그래서 요즘은 표현식을 많이 사용한다.

> 추가 과제: 히든 클래스 공부하기
