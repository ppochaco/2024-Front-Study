# 🌱 Redux란?
: 리액트 상태 관리 라이브러리
### 👍🏻 장점
1. 컴포넌트의 상태 업데이트를 다른 파일로 분리
2. 동일한 컴포넌트의 상태를 공유
3. 전역 상태 효율적으로 관리
4. 개발자 도구, 미들웨어로 비동기 작업 효율적으로 관리
### Redux 이외의 상태 관리 방법
- Context API
### 특징
1. React에서만 사용되는 라이브러리 아님
	- angular, vue에서 사용 가능
# ☘️ Redux의 키워드
### [action](./action.md)
### [reducer](./reducer.md)
### [store](./store.md)
### [dispatch](./dispatch.md)
### [subscribe](./subscribe.md)
# 🍀 Redux 사용하기
1. 초깃값 설정
2. reducer함수 정의
	- 상태의 불변성을 유지하면서 데이터에 변화를 일으켜야 함
		- spread 연산자 사용(...)
		- immer 라이브러리 사용
3. store 만들기
	- createStore 함수 사용
		```js
		import { createStore } from 'redux';
		
		const store = createStore(reducer);
		```
4. render 함수 만들기
	- 상태가 업데이트 될 때마다 호출
5. subscribe
	- react-redux 라이브러가 컴포넌트에서 redux의 상태를 조회하는 과정에서 수행함
6. action 발생시키기
	- store의 내장 함수인 dispatch 사용
# 🚨Redux의 3가지 규칙
1. 단일 store
2. 읽기 전용 상태
	- 객체의 변화 감지 시 얕은 비교로 성능이 좋지만 불변성을 유지해야 한다
	- 불변성 유지 방법: state 업데이트 시 기존 객체 냅두고 새로운 객체 생성
3. reducer는 순수한 함수
	- 순수한 함수
		- reducer 함수는 이전 state와 action 객체를 파라미터로 받음
		- 파라미터 이외의 값에 의존하면 안됨
		- 이전 상태는 절대로 건들지 않음
		- 변화를 준 새로운 state 객체를 만들어 반환
		- 똑같은 파라미터로 호출된 reducer 함수는 언제나 같은 결과 값 반환
	- reducer 함수에서 하면 안되는 일들
		- 랜덤 값 만들기
		- Date 함수로 현재 시간 가져오기
		- 네트워크 요청하기
		- (미들웨어에서 처리하기)
