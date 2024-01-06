# connect 함수
- react-redux에서 제공
- connect 함수 호출 시 다른 함수 반환
### 형태
> connect(mapStateToProps, mapDispatchToProps)(연동할 컴포넌트)
- mapStateToProps: redux store 안의 상태를 컴포넌트의 props로 넘겨줌
- mapDispatchToProps: action creator 함수를 컴포넌트의 props로 넘겨줌
### 사용
- 반환된 함수에 컴포넌트를 파라미터로 넣으면 redux와 연동된 컴포넌트 생성
```js
const makeContainer = connect(mapStateToProps, mapDispatchToProps)
makeContainer(타깃 컴포넌트)
```
# 컴포넌트에서 action을 dispatch하는 방법(counter 예시)
### 1. dispatch 함수 사용하기
```jsx
// containers/CounterContainer.jsx
	import { connect } from 'react-redux';
	import Counter from '../components/Counter';
	import { increase, decrease } from '../modules/counter';
	
	const CounterContainer = ({ number, increase, decrease }) => {
		return(
			<Counter number={number} onIncrease={increase} onDecrease={decrease} />
		);
	};

	const mapStateToProps = state => ({
	number: state.counter.number,
	});
	const mapDispatchToProps = dispatch => ({
		increase: () => {
			dispatch(increase());
		},
		decrease: () => {
			dispatch(decrease());
		},
	});
	
	export default connect(
		state => ({
		mapStatetoProps,
		mapDispatchToProps,
	)(CounterContainer);
```
### 2. bindActionCreators 사용하기
- action creator 함수의 개수 많을 때 유용
```jsx
	// containers/CounterContainer.jsx
	import { bindActionCreators } from 'redux';
	import { connect } from 'react-redux';
	import Counter from '../components/Counter';
	import { increase, decrease } from '../modules/counter';
	
	const CounterContainer = ({ number, increase, decrease }) => {
		return(
			<Counter number={number} onIncrease={increase} onDecrease={decrease} />
		);
	};
	
	export default connect(
		state => ({
			number: state.counter.number,
		}),
		dispatch =>
			bindActionCreators({
			increase,
			decrease,
			}, dispatch),
	)(CounterContainer);
	```
### 3. mapDispatchToProps에 객체 형태의 action creator 함수 넣기
   - connect 함수가 내부적으로 bindActionCreators 작업을 대신 수행함
``` jsx
	// containers/CounterContainer.js
	import { connect } from 'react-redux';
	import Counter from '../components/Counter';
	import { increase, decrease } from '../modules/counter';
	
	const CounterContainer = ({ number, increase, decrease }) => {
		return(
			<Counter number={number} onIncrease={increase} onDecrease={decrease} />
		);
	};
	
	export default connect(
		state => ({
			number: state.counter.number,
		}),
		{
			increase,
			decrease,
		},
	)(CounterContainer);
```