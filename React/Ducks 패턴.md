# Ducks 패턴
: 하나의 파일에 { Action Type, Action Creator, Reducer }를 모두 작성하는 패턴
- 작성된 하나의 파일을 **모듈**이라고 함 ^dae004
- 기능에 따라 파일이 분리됨
### 규칙
1. reducer 함수를 export default
2. action creator를 함수로 export
3. action type 이름은 module이름/action이름 형식으로 작성