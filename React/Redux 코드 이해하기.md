---
sticker: emoji//1f332
tags:
  - main
  - redux
---
# 🌱1. [[Redux 기본 개념|Redux]] 만들기
### 작성 해야 하는 코드
- [[action#^6e763e|Action Type]]
- [[action#^631203|Action Creator]]
- [[reducer]]
### 코드 분류 방법
1. 각각 기능에 따라 다른 파일에 작성하기
2. 기능별로 묶어 하나의 파일에 작성하기(**[[Ducks 패턴]]**)
### ✏️ 코드 작성 예시(Ducks 패턴, todoList)
1. [[Container 컴포넌트와 Presentational 컴포넌트|Presentational 컴포넌트 만들기]]
	```jsx
	//App.jsx
	import Todos from './components/Todos';
	import Counter from './components/Counter';
	
	const App = () => {
		return (
			<div>
				<Todos />
				<br/>
				<Counter number={0}/>
			</div>
		);
	}
	```

	```jsx
	//components/Todos.jsx
	const TodoItems = ({ todo, onToggle, onRemove }) => {
		return (
		<div>
			<input type="checkbox"/>
			<span>할 일을 입력하세요</span>
			<button>삭제</button>
		</div>
		);
	};
	
	const Todos = ({
		input,
		todos,
		onChangeInput,
		onInsert,
		onToggle,
		onRemove,
	}) => {
		const onSubmitHandler = e => {
			e.preventDefault();
		};
		
		return (
			<div>
				<form onSubmit={onSubmitHandler}>
					<input/>
					<button type="submit">등록</button>
				</form>
				<div>
					<TodoItem />
				</div>
			</div>
		);
	};
	
	export default Todos;
	```
	
	```jsx
	//components/Counter.jsx
	const Counter = ({ number, onIncrease, onDecrease}) => {
		return (
			<div>
				<h1>number</h1>
				<div>
					<button onClick={onIncrease}>+1</button>
					<button onClick={onDecrease}>-1</button>
				</div>
			</div>
		);
	};
	
	export default Counter;
	```
2. [[Ducks 패턴#^dae004|모듈]] 작성하기
	1. action type
		```jsx
		// modules/todos.jsx
		const CHANGE_INPUT = 'todos/CHANGE_INPUT';
		const INSERT = 'todos/INSERT';
		const TOGGLE = 'todos/TOGGLE';
		const REMOVE = 'todos/REMOVE';
		```
	2. action creator
		```jsx
		//modules/todos.jsx
		...
		export const changeInput = input => ({
			type: CHANGE_INPUT,
			input
		});
		
		let id = 1; // 테스트용 하나는 먼저 넣어 둘 거임
		export const insert = text => ({
			type: INSERT,
			todo: {
				id: id++,
				text,
				done: false
			}
		});
		
		export const toggle = id => ({
			type: TOGGLE,
			id
		});
		
		export const remove = id => ({
			type: REMOVE,
			id
		});
		```
	3. reducer
		```jsx
		//modules/todos.jsx
		...
		const initialState = {
			input: '',
			todos: [
				{
					id: 1,
					text: '테스트 용',
					done: true
				},
			]
		};
		
		function todos(state = initialState, action) {
			switch(action.type) {
				case CHANGE_INPUT:
					return {
						...state,
						input: action.input
					};
				case INSERT:
					return {
						...state,
						todos: state.todos.concat(action.todo);
					};
				case TOGGLE:
					return {
						...state,
						todos: state.todos.map(todo =>
						todo.id === cation.id ? {...todo, done: !todo.done} : todo
						)
					};
				case REMOVE:
					return {
						...state,
						todos: state.todos.filter(todo => todo.id !== action.id)
					};
				default:
					return state;
			}
		}
		
		export default todos;
		```
3. root reducer 만들기
	- 한 프로젝트에서 reducer가 여러 개 일 때 하나로 묶어 주어야 함
	- combineReducers 함수 사용
	```jsx
	//modules/index.jsx
	import { combineReducers } from 'redux';
	import counter from './counter';
	import todos from './todos';
	
	const rootReducer = combineReducers({
		counter,
		todos,
	});

	export default rootReducer;
	```
# ☘️ 2. React에 Redux 연결하기
### 작업 방법
1. index.js에서 store를 만들기
2. provider 컴포넌트로 App 컴포넌트 감싸기
	- store를 props로 전달해야 함
### ✏️ 코드 작성 예시(todoList 이어서)
1. store 만들기
	```jsx
	//src/index.jsx
	import React from 'react';
	import ReactDOM from 'react-dom/client';
	import App from './App';
	import { createStore } from 'redux';
	import rootReducer from './modules';
	
	const store = createStore(rootReducer);
	
	const root = ReactDOM.createRoot(document.getElementById('root'));
	root.render(<App/>);
	```
2. provider로 프로젝트에 Redux 적용하기
	```jsx
	//src/index.jsx
	import React from 'react';
	import ReactDOM from 'react-dom/client';
	import App from './App';
	import { createStore } from 'redux';
	import rootReducer from './modules';
	
	const store = createStore(rootReducer);
	
	const root = ReactDOM.createRoot(document.getElementById('root'));
	root.render(
	<Provider store={store}>
		<App/>
	</Provider>
	);
	```
# 🍀 3. [[Container 컴포넌트와 Presentational 컴포넌트|Container 컴포넌트]] 만들기
### 작업 방법
1. 해당 container 만들기
2. App 컴포넌트에 container 컴포넌트 연결하기
### ✏️ 코드 작성 예시(todoList 이어서)
1. container 파일 만들기
	- **[[connect 함수]]** 사용
	```jsx
	// containers/TodosContainer.js
	import { connect } from 'react-redux';
	import { changeInput, insert, toggle, remove } from '../modules/todos';
	import Todos from '../components/Todos';
	
	const TodosContainer = ({
		input,
		todos,
		changeInput,
		insert,
		toggle,
		remove,
	}) => {
		return (
			<Todos input={input} todos={todos} onChangeInput={changeInput} onInsert={insert} onToggle={toggle} onRemove={remove}/>
		);
	};
	
	export default connect(
		({ todos }) => ({
			input: todos.input,
			todos: todos.todos,
		}),
		{
			changeInput,
			insert,
			toggle,
			remove,
		},
	)(TodosContainer);

	```
2. App 컴포넌트에 container 컴포넌트 추가하기
```jsx
// App.js
import CounterContainer from './containers/CounterContainer';
import TodosContainer from './containers/TodosContainer';

const App = () => {
	return (
		<div>
			<TodosContainer />
			<hr />
			<CounterContainer />
		</div>
		);
}

export default App;
```
3. Presentational 컴포넌트 수정하기
```jsx
// components/Todos.js

const TodoItem = ({ todo, onToggle, onRemove }) => {
	return (
		<div>
			<input 
				type="checkbox"
				onClick={() => onToggle(todo.id)}
				checked={todo.done}
				readOnly={true}
			/>
			<span style={{ textDecoration: todo.done ? 'line-through' : 'none' }}>
				{todo.text}
			</span>
			<button onClick={() => onRemove(todo.id)}>삭제</button>
		</div>
		);
	};
	
	const Todos = ({
		input,
		todos,
		onChangeInput,
		onInsert,
		onToggle,
		onRemove,
	}) => {
		const onSubmit = e => {
			e.preventDefault();
			onInsert(input);
			onChagneInput('');
		};
		const onChange = e => onChagneInput(e.target.value);
		
		return (
			<div>
				<form onSubmit={onSubmit}>
					<input value={input} onChange={onChange} />
					<button type="submit">등록</button>
				</form>
				<div>
					{todos.map(todo => (
						<TodoItem
							todo={todo}
							key={todo.id}
							onToggle={onToggle}
							onRemove={onRemove}
						/>
					))}
				</div>
			</div>
		);
	};
	
	export default Todos;
```