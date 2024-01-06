# Redux 쉽게 사용하는 방법

1. redux-action 라이브러리로 action creator 함수, reducer 작성하기
2. immer 라이브러리 사용하기
3. react-redux의 Hooks 사용해 container 컴포넌트 작성하기

# 🐣1. redux-actions

- createAction: 객체 생성 없이 간단히 action creator 함수 선언
- handleActions: action마다 업데이트 함수 설정
- payload: action에 필요한 추가 데이터

### 1. action creator 함수 작성하기
-  
    ```jsx
    // modules/todos.js
    import { createAction } from "redux-actions";
    
    const CHANGE_INPUT = "todos/CHANGE_INPUT";
    const INSERT = "todos/INSERT";
    const TOGGLE = "todos/TOGGLE";
    const REMOVE = "todos/REMOVE";
    
    export const chagneInput = createAction(CHAGNE_INPUT, (input) => input);
    
    let id = 1;
    export const insert = createAction(INSERT, (text) => ({
    id: id++,
    text,
    done: false,
    }));
    
    export const toggle = createAction(TOGGLE, (id) => id);
    export const remove = createAction(REMOVE, (id) => id);
    ```

### 2. reducer 작성하기
- 
    ```jsx
    //modules/todos.js
    import { createActions, handleActions } from 'redux-actions';
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

    const todos = handleActions(
    	{
    		[CHANGE_INPUT]: (state, {payload: input}) => ({ ...state, input }),
    		[INSERT]: (state, { payload: todo}) => ({
    			...state,
    			todos: state.todos.concat(todo),
    		}),
    		[TOGGLE]: (state, { payload: id}) => ({
    			...state,
    			todos: state.todos.map(todo =>
    				todo.id === id ? {...todo, done: !todo.done } : todo,
    			),
    		}),
    		[REMOVE]: (state, { payload: id }) => ({
    			...state,
    			todos: state.todos.filter(todo => todo.id !== id),
    		}),
    	},
    	initialState,
    );

    export default todos;
    ```

# 🐥 2. immer

- reducer에서 상태 업데이트 시 반드시 불변성 지켜야 함
- 모듈의 상태가 복잡한 경우 spread 연산자나 배열 내장 함수 사용 어려움
- 모든 함수에 immer 적용할 필요는 없음

  ```jsx
  //modules/todos.js
  //TOGGLE에만 immer 적용
  import { createActions, handleActions } from 'redux-actions';
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

  const todos = handleActions(
  	{
  		[CHANGE_INPUT]: (state, {payload: input}) => ({ ...state, input }),
  		[INSERT]: (state, { payload: todo}) => ({
  			...state,
  			todos: state.todos.concat(todo),
  		}),
  		[TOGGLE]: (state, { payload: id}) => ({
  			...state,
  			const todo = draft.todos.find(todo => todo.id === id);
  			todo.done = !todo.done;
  		}),
  		[REMOVE]: (state, { payload: id }) => ({
  			...state,
  			todos: state.todos.filter(todo => todo.id !== id),
  		}),
  	},
  	initialState,
  );

  export default todos;
  ```

# 3. react-redux의 Hooks

1. useSelector: connect 함수 사용 없이 redux의 상태 조회 가능
2. useDispatch: 컴포넌트 내부에서 store의 내장 함수인 dispatch 사용 가능
3. useStore: 컴포넌트 내부에서 redux store 객체를 직접 사용 가능

   ```jsx
   //containers/TodosContainer.js
   import { useCallback } from 'react';
   import { useSelector, useDispatch } from 'react-redux';
   import { changeInput, insert, toggle, remove } from '../modules/todos';
   import Todos from '../components/Todos';

   const TodosContainer = {
   	const { insert, todos } = useSelector(({ todos }) => ({
   		input: todos.input,
   		todos: todos.todos
   	}));
   	const dispatch = useDispatch();
   	const onChagneInput = useCallback(input => dispatch(changeInput(input)), [dispatch]);
   	const onInsert = useCallback(text => dispatch(insert(text)), [dispatch]);
   	const onToggle = useCallback(id => dispatch(toggle(id)), [dispatch]);
   	const onRemove = useCallback(id => dispatch(remove(id)), [dispatch]);

   	return (
   		<Todos
   			input={input}
   			todos={todos}
   			onChangeInput={onChangeInput}
   			onInsert={onInsert}
   			onToggle={onToggle}
   			onRemove={onRemove}
   		/>
   	);
   };

   export default TodosContainer;
   ```

### connect 함수와 react-redux의 차이점

1. connect 함수: 성능 최적화
   - 해당 컴포넌트의 부모 컴포넌트가 리렌더링 될 때, 해당 container 컴포넌트의 props가 바뀌지 않았다면 리렌더링이 자동으로 방지
2. useSelector: 최적화 작업 자동으로 안됨 -> React.memo 사용하기
   ```jsx
   //containers/TodosContainer.js
   //이 예시는 성능 최적화 필요 없지만 예시로..
   import React from 'react';
   ...
   export default React.memo(TodosContainer);
   ```
