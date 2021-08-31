## Recoil

#### 등장배경

- 기존에 많이 사용되던 Redux, MobX는 자유도가 높아 가이드가 제각각
- store 구성을 위한 action, reducer 등 기본적인 보일러 플레이트 설계
- 페이스북이 발표한 상태관리 라이브러리



#### 프로젝트 설정

1. node-modules 설치

   - typescript 기본 지원, 의존성 모듈 따로 설치 안해줘도 됨

   ``` react
   yarn add recoil
   ```

   

2. root 경로의 index.tsx로 이동 후 수정

   - RecoilRoot provider를 이용해, reacoil을 사용 가능하도록 설정

   ``` react
   import React from "react";
   import ReactDOM from "react-dom";
   import "./index.css";
   import reportWebVitals from "./reportWebVitals";
   import Routes from "./routes";
   import { RecoilRoot } from "recoil";
   
   ReactDOM.render(
     <React.StrictMode>
       <RecoilRoot>
         <Routes />
       </RecoilRoot>
     </React.StrictMode>,
     document.getElementById("root")
   );
   
   reportWebVitals();
   ```



#### Atom

- 전역 상태 관리가 되는 대상, 상태의 단위
- 고유한 key 값을 가져야하며, 기본값 default 설정 가능
- hooks 사용과 매우 유사

``` react
import { atom } from "recoil";

export interface TodoType {
  id: number;
  contents: string;
  isCompleted: boolean;
}

// atom
export const inputState = atom<string>({
  key: "inpuState",

  default: "",
});

// atom
export const todosState = atom<TodoType[]>({
  key: "todos",

  default: [
    {
      id: 1,
      contents: "Todo List 1",
      isCompleted: true,
    },
    {
      id: 2,
      contents: "Todo List 2",
      isCompleted: false,
    },
    {
      id: 3,
      contents: "Todo List 3",
      isCompleted: false,
    },
  ],
});

```



##### 적용 예시

1. useRecoilState (useState와 동일한 형태)

   ```react
   import { useRecoilState } from 'recoil';
   import { todosState, TodoType } from 'recoil/todo.ts'
   ...
   
   const [data, setData] = useRecoilState<TodoType[]>(todosState)
   ...
   ```

   

 2. useRecoilValue

    - 상태값만 반환

    ``` react
    import { useRecoilValue } from 'recoil'
    import { todosState, TodoType } from 'recoil/todo.ts'
    ...
    
    const todos = useRecoilValue<TodoType[]>(todosState);
    ...
    ```

    

3. useSetRecoilState

   - setter 함수만 반환

   ```react
   import { useSetRecoilState } from 'recoil'
   import { todosState, TodoType } from 'recoil/todo.ts'
   ...
   
   const setTodos = useSetRecoilState<TodoType[]>(todosState);
   ...
   ```



4. useResetRecoilState

   - default값으로 reset

   ```react
   import { useResetRecoilState } from 'recoil'
   import { todosState, TodoType } from 'recoil/todo.ts'
   ...
   
   const resetTodos = useResetRecoilState<TodoType[]>(todosState);
   ...
   ```

   



#### Selector

- atom에서 파생된 데이터 조각
- 비동기 처리
- 데이터를 반환하는 순수 함수
- selectorFamily - 매개변수 넘기는 함수일 때 사용
- 기본적으로 값을 캐싱 (같은 응답을 보내는 api call에 대해 추가적으로 요청 x)

```react
import { atom, selector, selectorFamily } from 'recoil'

export const countState = atom({
	key: 'count',
	default: 0,
})

export const oddEvenState = selector({
    key: 'oddEvenState',
    get: ({get} => {
        const count = get(countState);
        return count % 2 ? '홀' : '짝'
    })
})

export const asyncDataQurey = selectorFamily<Data, number>({
    key: 'asyncDataQuery',
    get: (userId: number) => async ({get}) => {
        const response = await getData(usetId)
        return response.data;
    })
})
```





#### 비동기 처리

1. suspense 사용

   ``` react
   import React, {Suspense} from 'react';
   
   ...
   return (
   	<Wrapper>
       	<Suspense fallback={<div>loading...</div>}>
               ....
           </Suspense>
       </Wrapper>
   )
   ...
   ```

   

2. Recoil의 loadable

   Loadable 객체

   - state: hasValue, hasError, loading 상태 가질 수 있음
   - contents: hasValue 일땐 value, hasError일땐 Error 객체, loading일땐 Promise

   ```react
   import {getSelector} from 'recoil/test';
   import {useRecoilState, useRecoilValueLoadable} from 'recoil';
   
   const Test: React.FC = () => {
       const loadable = useRecoilValueLoadable(getSelector);
       
       switch(loadable.state) {
           case 'hasValue':
               return (
               	...값 있을때 컴포넌트
               );
               
           case 'loading':
               return (
               	... 로딩 컴포넌트
               );
               
           case 'hasError': 
               throw loadable.contents;
       }
   }
   
   export default Test;
   ```

   



#### 단점

devtools 부재



#### 참고

https://velog.io/@juno7803/Recoil-Recoil-200-%ED%99%9C%EC%9A%A9%ED%95%98%EA%B8%B0#-reactsuspense%EC%9D%98-%EC%A7%80%EC%9B%90-%EB%B9%84%EB%8F%99%EA%B8%B0-%EC%83%81%ED%83%9C-%EC%B2%98%EB%A6%AC