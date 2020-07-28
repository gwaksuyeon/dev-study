## Immer

immutable과 같은 상태관리 라이브러리



#### 설치

```react
yarn add immer
```



#### 사용 - Reducer

```react
import { createAction, ActionType, createReducer } from 'typesafe-actions';
import produce from 'immer';

// Action Type
export const ACTIONS = 'actions/ACTIONS';
export const ACTIONS_LOAD_START = 'actions/ACTIONS_LOAD_START';
export const ACTIONS_SUCCESS = 'actions/ACTIONS_SUCCESS';
export const ACTIONS_LOAD_END = 'actions/ACTIONS_LOAD_END';
export const ACTIONS_RESET = 'actions/ACTIONS_RESET'
export const ACTIONS_ADD = 'actions/ACTIONS_ADD';
export const ACTIONS_ADD_SUCCESS = 'actions/ACTIONS_ADD_SUCCESS';

// Actions
export const actionsList = creacteAction(ACTIONS)<any>();
export const actionsAddList = creacteAction(ACTIONS_ADD)<any>();
export const actionsReset = creacteAction(ACTIONS_RESET)<any>();
export const actionsLoadStart = creacteAction(ACTIONS_LOAD_START)();
export const actionsLoadEnd = creacteAction(ACTIONS_LOAD_END)();

// Sagas
export const actionsSuccess = createAction(ACTIONS_SUCCESS)<any | null>();
export const actionsAddSuccess = createAction(ACTIONS_ADD_SUCCESS)<any | null>();

const actions = {
    actionsList,
    actionsAddList,
    actionsReset,
    actionsLoadStart,
    actionsLoadEnd,
    actionsSuccess,
    actionsAddSuccess,
};

type Actions = ActionType<typeof actions>;

interface State {
    list: object[];
    limit: number;
    page: number;
    isLoading: boolean;
    isEndPage: boolean;
}
const initialSate: State = {
    list: [],
    limit: 10,
    page: 0,
    isLoading: true,
    isEndPage: false,
}

// Reducer
const Reducer = createReducer<State, Actions>(initialState, {
    [ACTIONS_LOAD_START]: (state, action) =>
    	produce(state, (draft) => {
            draft.isLoading = true;
        }),
    [ACTIONS_LOAD_END]: (sate, action) =>
    	produce(state, (draft) => {
            draft.isLoading = false;
        }),
    [ACTIONS_RESET]: (state, action) => 
    	produce(state, (draft) => {
            draft.list = [];
            draft.isEndPage = false;
            draft.page = 0;
        }),
    [ACTIONS_SUCCESS]: (state, action) => 
    	produce(state, (draft) => {
            const { data } = action.payload;
            draft.list = data;
            if (list.length < state.limit) {
                draft.isEndPage = true;
            }
        }),
    [ACTIONS_ADD_SUCCESS]: (state, action) =>
    	produce(state, (draft) => {
            const { data } = action.payload;
            if (list.length > 0) {
                draft.list = state.list.concat(data);
                draft.page = state.page + 1;
            }
            if (list.length < state.limit) {
                draft.isEndPage = true;
            }
        })
})

export default Reducer;
```



#### 사용 - Saga

```react
import { takeEvery, put, delay } from 'redux-saga/effects';
import {
    ACTIONS,
    ACTIONS_SUCCESS,
    ACTIONS_ADD,
    ACTIONS_ADD_SUCCESS,
    ACTIONS_RESET,
    ACTIONS_LOAD_START,
    ACTIONS_LOAD_END
} from 'reducer가 있는 곳';
import {ApiActions} from 'api있는곳';

// data load
function* actions(action: any) {
    const { type } = actions.payload;
    yield put({type: ACTIONS_LOAD_START});
    yield put({type: ACTIONS_RESET});
    const data = yield ApiActions();
    
    yield put({
        type: ACTIONS_SUCCESS,
        payload: { data }
    });
    
    yield delay(100);
    yield put({type: ACTIONS_LOAD_END})
}

// data paging add
function* actionsAdd(action: any) {
    const { page } = action.payload;
    yield put({type: ACTIONS_LOAD_START});
    const data = yield ApiActions({page});
    yield put({
        type: ACTIONS_ADD_SUCCESS,
        payload: { data }
    });
    
    yield delay(100);
    yield put({ type: ACTIONS_LOAD_END });
}

export function* watchActionsAdd() {
    yield takeEvery(ACTIONS_ADD, actionsAdd);
}
```



#### Immutable과의 비교(느낀점)

- Immutable보다 코드가 간결해서 작성하기 쉬움
- get, set 사용하지 않고 쓸 수 있음