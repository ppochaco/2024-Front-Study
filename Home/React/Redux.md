---
sticker: emoji//1f31e
---
# 🌱 Redux란?
: 리액트 <span style='color:#eb3b5a'>상태 관리</span> 라이브러리
### 👍🏻 장점
1. 컴포넌트의 상태 업데이트를 다른 파일로 **분리**
2. 동일한 컴포넌트의 상태를 공유
3. 전역 상태 효율적으로 관리
4. 개발자 도구, 미들웨어로 비동기 작업 효율적으로 관리
### Redux 이외의 상태 관리 방법
- Context API
### 특징
1. React에서만 사용되는 라이브러리 아님
	- angular, vue에서 사용 가능
# ☘️ Redux의 키워드
## action
: action 발생 시 상태 변화 일어남
- 형태
```javascript
{
	type: 'TOGGLE_VALUE'
}
```
- 특징
	- action 객체는 반드시 type field를 가져야 함
	- field의 값은 aciton의 이름
	- 상태 업데이트 시 필요한 type 추가하면 됨
## action creator
: action 객체를 만들어 주는 함수
```js
const changeInput = text => ({
	type: 'CHANGE_INPUT',
	text
});
```
## reducer
: 변화를 일으키는 함수
- 특징
	- action을 만들면 reducer가 현재 상태와 액션 객체를 파라미터로 받아와 새로운 상태 반환
```js
const initialState = {
	counter: 1
};
function reducer(state = initialState, action) {
	switch (action.type) {
		case INCREMENT:
		return {
			counter: state.counter + 1
		}
	}
}
```
## store
: 프로젝트에 redux를 적용하는 역할
- 특징
	- 한 프로젝트는 단 하나의 store를 가짐
	- store 안에 현재 어플리케이션 상태와 reducer, 내장 함수가 있음
## dispatch
: store의 내장 함수, action을 발생시키는 것
- action 객체를 파라미터로 넣어 호출하면 store는 reducer 함수를 실행 해 새로운 상태 반환함
## subscribe
: store의 내장 함수
- listener 함수를 파라미터로 넣어 호출하면, listener 함수가 action이 dispatch되어 상태가 업데이트 될 때마다 호출됨
```js
const listener = () => {
	console.log('상태 업데이트');
}
const unsubscribe = store.subscribe(listener);

unsubscribe();
```
# 🍀 Redux 사용하기
1. 초깃값 설정
2. reducer 함수 정의
	- 상태의 불변성을 유지하면서 데이터에 변화를 일으켜야 함
		- spread 연산자 사용(...)
		- immer 라이브러리 사용
3. store 만들기
	- createSore 함수 사용
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