## Redux-saga란

- react 상태관리 툴
- 데이터 fetching이나 캐시에 접근하는 순수하지 않은 비동기 동작들을 쉽고 좋게 만드는 것을 목적으로하는 라이브러리
- 제네레이터 함수 
  - function* 키워드로 작성하는 함수



### 시작하기

#### 설치

``` react
yarn add redux-saga
```



#### 구현

1. mock data 생성

   - src/modules/user/data.mock.ts

   ``` react
   export const Data = [
       {
           id: 1,
           name: 'suyeon',
           age: 26,
       },
       {
           id: 2,
           name: 'suyeon2',
           age: 36,
       }
   ]
   ```

   

2. mock data Record 생성

   - src/modules/user/data.detail.ts

     ``` react
     import { Record } from 'immutable';
     
     export interface IDetailState {
         id: number;
         name: string;
         age: number;
     }
     const DetailStateRecord = Record<IDetailState>({
         id: 0,
         name: '',
         age: 0,
     })
     export class DetailState extends DetailStateRecord {}
     ```

     

3. index.ts 작성

   - src/modules/user/index.ts

     ``` react
     import { Record, List, fromJS } from 'immutable'; // fromJS - 내부객체도 Map으로 변경
     import { handleAction, Actions } from 'redux-actions';
     import { all, takeLatest, put, delay } from 'redux-saga/effects';
     
     import { DetailState } from './data.detail';
     import { Data } from './data.mock';
     
     const prefix = 'user'; // 액션타입의 중복 방지
     export const ActionTypes = {
         // fetch list
         TRY_FETCH_LIST: `${prefix}/TRY_FETCH_LIST`,
         REQUEST_FETCH_LIST: `${prefix}/REQUEST_FETCH_LIST`,
         SUCCESS_FETCH_LIST: `${prefix}/SUCCESS_FETCH_LIST`,
         FAILURE_FETCH_LIST: `${prefix}/FAILURE_FETCH_LIST`,
     }
     
     export const ActionCtor = {
         fetchList: () => {
             return {type: ActionTypes.TRY_FETCH_LIST};
         },
     }
     
     // all 함수를 통해 Saga를 하나로 묶음
     export funtion* Saga() {
         yield all ([
             yield takeLatest(ActionTypes.TRY_FETCH_LIST, FetchListAsync),
         ]);
     }
     
     // saga 생성
     function* FetchListAsync() {
         try {
             yield put({type: ActionTypes.REQUEST_FETCH_LIST});
             yield delay(100); // 1초 뒤에 나오게 하는 함수
             
             const response = [...Data];
             yield pust({type: ActionTypes.SUCCESS_FETCH_LIST, payload: fromJS(response)})
         } catch (error) {
             yield put({type: ActionTypes.FAILURE_FETCH_LIST, payload: error});
         }
     }
     
     // state
     interface IState {
         listActionType: string;
         list: List<DetailState>;
     }
     const StateRecord = Record<IState>({
         listActionType: '',
         list: List(),
     })
     export class State extends StateRecord {}
     
     const initState = new State();
     
     export default handleActions<State, any>({
         [ActionTypes.REQUEST_FETCH_LIST]: (state, action) => {
             return state
             	.set('listActionType', action.type);
         },
         [ActionTypes.SUCCESS_FETCH_LIST]: (state, action) => {
             return state
             	.set('listActionType', action.type)
             	.set('list', action.payload);
         },
         [ActionTypes.FAILURE_FETCH_LIST]: (state, action) => {
             return state
             	.set('listActionType', action.type);
         },
     }, initSaga);
     ```

     

4. Root Saga 생성

   - src/modules/saga.ts

     ``` react
     import { fork } from 'redux-saga/effects';
     
     import { Saga as UserSaga } from 'store/modules/user';
     
     export function* RootSaga() {
         yield fork(UserSaga);
     }
     ```

5. 미들웨어를 통해 연결

   - src/moduels/index.ts

     ``` react
     import { applyMiddleware, createStore, compose, Middleware, combineReducers } from 'redux';
     // action type을 예쁘게 보이게 하는 라이브러리
import { createLogger } from 'redux-logger'; 
     import createSaga from 'redux-saga';
     
     import * as Reducer from './reducer';
     import { RootSaga } from './saga';
     
     interface IWindow extends Window {
       // 크롬 확장 프로그램에 작성되어 있는 JS함수
       __REDUX_DEVTOOLS_EXTENSION_COMPOSE__?: <R>(a: R) => R;
     }
     
     const loggerMiddleware = createLogger();
     // saga 미들웨어 생성
     const sagaMiddleware = createSaga();
           
     const isDev = process.env.NODE_ENV !== 'production';
     
     let middleware: Middleware[] = [sagaMiddleware];
     if (process.env.NODE_ENV !== 'production') {
       middleware = [...middleware, loggerMiddleware];
     }
     
     export function configureStore() {
       const devTools = isDev && (window as IWindow).__REDUX_DEVTOOLS_EXTENSION_COMPOSE__;
       const composeEnhancers = devTools || compose;
     
       // store에 mount
       const store = createStore(
         combineReducers(Reducer),
         composeEnhancers(
           applyMiddleware(...middleware),
         ),
       );
       
       // saga 실행
       sagaMiddleware.run(RootSaga);
       return store;
     }
     ```
     
     

6. 적용

   - src/container/user/List.tsx

     ``` react
     import React from 'react';
     import { List } from 'immutable';
     import { connect } from 'react-redux';
     import { bindActionCreators } from 'redux';
     
     import { IStoreState } from 'store/modules/state';
     import { ActionCtor } from 'store/modules/user';
     import { DetailState } from 'store/modules/user/data.detail';
     
     interface IProps {
         list: List<DetailState>;
         
         fetchList: typeof ActionCtor.fetchList;
     }
     
     class List extends React.Componet<IProps> {
         
         componentDidMount() {
             this.fetchList();
         }
         
         fetchList = async () => {
             await this.props.fetchList();
         }
         
         render() {
             const { list } = this.props;
             
             return (
             	<div>
                 	{list.toJS().map(obj => {
                         return (
                         	<div key={`list-${obj.id}`}>
                                 <p>이름: {obj.name}</p>
                                 <p>나이: {obj.age}</p>
                             </div>
                         )
                     })}
                 </div>
             )
         }
     }
     
     export default connect(
       (state: IStoreState) => ({
         list: state.UserReducer.get('list'),
       }),
       dispatch => bindActionCreators({
         fetchList: ActionCtor.fetchList,
       }, dispatch),
     )(List);
     
     ```

     