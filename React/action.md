---
tags:
  - redux
---
# action

^6e763e

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

^631203

: action 객체를 만들어 주는 함수
```js
const changeInput = text => ({
	type: 'CHANGE_INPUT',
	text
});
```