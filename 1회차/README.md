### 5장 표현식과 문 - 완료값에 관하여
- 면접에서 자주 출제되므로 잘 익혀봅시다.
- p.58 참고

### 숫자 구분자 추가 Numeric Separotor
- [참고 링크](https://dev.to/suprabhasupi/numeric-separators-in-javascript-3jec)

### 타입 캐스팅 더 알아보기
- 타입 캐시팅 시 생성자 함수에 new 키워드를 사용한다면? -> Object화 된다!
- Number와 parseInt의 차이점
  - Number('10000원') -> NaN
  - parseInt('10000원') -> 10000 (숫자인 접두사만 치환)

### 연산자 더 알아보기
- 비트 연산자? 어디서 쓸까
  - 리액트 코드를 뜯어보니 비트연산을 이용해 우선순위를 결정 -> 리액트 렌더링을 빠르게 해준다
  - 여러개 업적을 변수 하나(비트)로 처리 0과 1을 이용하여 업적 미달성/달성 (저장 공간을 줄일 수 있는 장점을 가짐)

### null과 undefined
- undefined 보다 null이 좀 더 빠르다
- undefined는 메모리를 3바이트 더 먹는다
