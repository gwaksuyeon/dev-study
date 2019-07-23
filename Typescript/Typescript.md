## Typescript

- javascript에서 Type을 지정해주는 방식
- 코드만 보고 어떤 props 또는 파라미터를 넣어줘야하는 지 알 수 있음
- 실수를 줄일 수 있음
- 협업할 때 유용



### 환경준비

``` react
yarn global add typescript
```



### 기본 타입

1. let

   - 변할 수 있는 변수를 선언할 때 사용

   ``` react
   // red, orange, blue 중 하나
   let color: 'red'|'orange'|'blue' = 'red';
   ```

2. const

   - 변하지 않는 상수를 선언할 때 사용

   ``` react
   // 숫자배열
   const numbers: number[] = [1, 2, 3];
   ```



### 함수에서 타입 정의하기

```react
// 파라미터의 타입, 함수의 타입 설정
sum = (x: number, y: number): number = {
    return x + y;
}
```





### interface

- 클래스 또는 객체를 위한 타입을 지정할 때 사용

``` react
import React from 'react';

interface IUserState {
    id: number;
    name: string;
}

class UserList extends React.Component {
    render() {
        const person: IUserState = [
            {
            	id: 1,
            	name: 'suyeon',
        	},
            {
                id: 2,
                name: 'suyeon2',
            }
        ]
        return (
        	<ul>
                {person.map((obj, inx) => {
                    return (
                    	<li key={`list-${inx}`}>
                        	{obj.name}
                        </li>
                    )
                })}
            </ul>
        )
    }
}

export default UserList;
```

