## Redux란

- state의 값을 중앙에서 관리해주는 라이브러리



### Redux의 규칙

1. 하나의 애플리케이션 안에는 하나의 스토어가 존재
2. 상태는 읽기전용
3. 변화를 일으키는 함수, 리듀서는 순수한 함수
   - 이전상태와 액션객체를 파라미터로 받음
   - 변화를 일으킨 새로운 상태 객체를 만들어서 반환
   - 언제나 똑같은 결과값을 반환해야함



### Redux 시작

``` react
yarn add redux
yarn add react-redux
yarn add redux-actions
```

``` react
yarn add redux-logger // actionType을 정리된 형태로 보이게하는 라이브러리
```



### 구현

1. src/store/modules/counter/index.ts 생성

2. index.ts 작성

   ``` react
   import { handleActions } from 'redux-actions';
   
   // 액션 타입 정의
   export const ActionTypes = {
       CAHNGE_NUMBER_INCREASE: "CHANGE_NUMBER_INCREASE", // 숫자증가
       CAHNGE_NUMBER_DECREASE: "CHANGE_NUMBER_DECREASE", // 숫자감소
   }
   
   // 액션 생성 함수를 만듬
   export const ActionCtor = {
       onChangeNumberIncrease: () => {
           return {type: ActionTypes.CHANGE_NUMBER_INCREASE};
       },
       onChangeNumberDecrease: () => {
           return {type: ActionTypes.CHANGE_NUMBER_DECREASEE};
       },
   }
   
   // 모듈의 초기 상태를 정의
   interface IState {
       num: number;
   }
   const StateRecord = Record<IState>({
       num: 0,
   })
   export class Sate extends StateRecord {}
   
   const initSate = new State();
   
   // reducer 생성
   export default handleActions<State, any>({
       [ActionTypes.CHANGE_NUMBER_INCREASE]: (state) => {
           return state
           	.update('num', num => number + 1)
       },
       [ActionTypes.CHANGE_NUMBER_DECREASE]: (state) => {
           return state
           	.update('num', num => number - 1)
       },
   }, initState);
   ```

3. Root reducer 생성

   - src/modules/reducer.ts

     ``` react
     // 여러 리듀서들을 한 곳에 모은 후 내보내기
     
     export { default as CounterReducer } from 'store/modules/counter'
     ```

   - src/modules/state.ts

     ``` react
     // 여러 state들을 한 곳에 모은 후 내보내기
     
     import { Sate as CounterState } from 'store/modules/counter';
     
     export interface IStoreState (
     	CounterReducer: CounterState,
     )
     ```

     

   - src/modules/index.ts

     ``` react
     import { createStore, combineReducers } from 'redux';
     
     import * as Reducer from './reducer';
     
     export function configureStore() {
         const store = createStore(
         	combineReducers(Reducer),
         );
         return store;
     }
     ```

4. redux 적용

   - src/index.tsx

     ``` react
     import React from 'react';
     import ReactDOM from 'react-dom'; // router
     import { Provider } from 'react-redux';
     
     import Root from './Root';
     import { configureStore } from 'store/modules';
     
     ReactDOM.render (
     	<Provider store={configureStore()}>
         	<Root />
         </Provider>
     );
     ```

   - src/container/counter.tsx

     ``` react
     import React from 'react';
     import { connect } from 'react-redux';
     import { bindActionCreators } from 'redux';
     
     import { IStoreState } from 'store/modules/state';
     import { ActionCtor } from 'store/modules/counter';
     
     interface IProps {
         num: number;
         
         onClickIncrease: typeof ActionCtor.onClickIncrease;
         onClickDecrease: typeof ActionCtor.onClickDecrease;
     }
     class Counter extends React.Component<IProps>{
         render() {
             const { num } = this.props;
             return (
             	<div>
                  <h1>counter</h1>
                  <p>값: {num}</p>
                  <button type="button" onClick={this.onClickIncrease}>+</button>
                  <button type="button" onClick={this.onClickDecrease}>-</button>
                 </div>
             )
         }
     }
     
     export default connet(
         (state: IStoreState) => ({
            num: state.CounterReducer.get('num'), 
         }),
         dispatch => bindActionCreators({
             onClickIncrease: ActionCtor.onClickIncrease,
             onClickDecrease: ActionCtor.onClickDecrease,
         }, dispatch)
     )(Counter);
     ```