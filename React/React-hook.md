## React Hook

- React v16.8에 새로 도입된 기능
- 함수형 컴포넌트에서 할 수 없었던 다양한 작업 가능하게 함



### useState

- 가장 기본적인 Hook
- 함수형 컴포넌트에서 상태관리 가능

``` react
import React, { useState } from 'react';

const Counter = () => {
    /* class형 컴포넌트에서의 state 사용법
        state = {
            value: 0
        }
	*/
    const [value, setValue] = useState<number>(0);
    
    return (
    	<div>
            <p>현재 카운터 값은 <b>{value}</b></p> 입니다.
        	<button onClick={() => setValue(value + 1)}>+1</button>
        	<button onClick={() => setValue(value - 1)}>-1</button>
        </div>	
    )
}
```

- 하나의 useState함수는 하나의 상태값만 관리 가능. 여러 상태값 관리시 여러번 사용

  ``` react
  ...
  const [name, setName] = useState<string>('');
  const [email, setEmail] = useState<string>('');
  ...
  ```

  

### useEffect

- 컴포넌트가 렌더링 될 때마다 특정 작업을 수행하도록 설정
- componentDidMount와 componentDidUpdate를 합친 형태와 비슷

``` react
import React, { useState, useEffect } from 'react';

const Info = () => {
    
    const [name, setName] = useState<string>('');
    const [email, setEmail] = useState<string>('');
    
    useEffect(() => {
        console.log('렌더링 완료');
        console.log({name, email});
    })
    
    const onChangeName = (e: React.ChangeEvent) => {
        setName(e.target.value);
    }
    const onChangeEmail = (e: React.ChangeEvent) => {
        setEmail(e.target.value);
    }
    
    return (
    	....	
    )
}
```



1. 마운트 될 때만 실행하고 싶을 때

   - 가장 처음 렌더링 될 때만 실행되고 업데이트 할 경우에는 실행할 필요 없을 때
   - 두번째 파라미터에 비어있는 배열을 넣어줌

   ``` react
   ...
   useEffect(() => {
       console.log('마운트 될 때만 실행');
   }, []);
   ...
   ```

   

2. 특정값이 업데이트 될 때만 실행하고 싶을 때

   - 두번째 파라미터에 검사하고 싶은 값을 넣어줌

   ``` react
   // 클래스형 컴포넌트일 때
   ...
   componentDidUpdate(prevProps, prevState) {
       if (prevProps.value !== this.props.value) {
           실행
       }
   }
   ...
   
   // 함수형 컴포넌트일 때
   ...
   useEffect(() => {
       console.log(name);
   }, [name]);
   ...
   ```

   

3. 언마운트되기 전이나 업데이트 되기 직전에 실행하고 싶을 때 (cleanup 함수)

   - 언마운트 될 때만 뒷정리 함수 호출하려면 두번째 파라미터에 비어있는 배열 추가

   ```react
   ...
   useEffect(() => {
       console.log('effect');
       console.log(name);
       
       return () => {
           console.log('cleanup');
           conosle.log(name);
       }
   })
   ...
   ```