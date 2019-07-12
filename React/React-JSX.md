### component

- 클래스 형식

  ``` react
  import React from 'react'; // react를 불러옴
  
  class 컴포넌트명 extends React.Component{
      // 함수 선언시 이곳에서
      render() {
          // 변수 선언시 이곳에서
          return (
              <div>
              	react
              </div>
          )
      }
  }
  
  export default 컴포넌트명; // 다른곳에서 불러와 사용하도록
  ```

  - state 또는 라이프사이클을 사용할 때 사용

- 함수 형식

  ``` react
  import React from 'react'; // react를 불러옴
  
  const 컴포넌트명 = () => {
      return (
      	<div>
              react
          </div>
      )
  }
  
  export default 컴포넌트명; // 다른곳에서 불러와 사용하도록
  ```

  - state 또는 라이프사이클을 전혀 사용하지 않을 때 사용
  - props는 사용가능



### JSX

- 두개 이상의 엘리먼트는 무조건 하나의 엘리먼트로 감싸져있어야함

  - div 태그대신 Fragment 또는 <> 태그 사용가능

  

- JSX안에서 자바스크립트 값 사용

  ``` react
  import React from 'react';
  
  class 컴포넌트명 extends React.componet {
      render() {
          const name = 'react';
          return (
              <div>
              	hello {name}!
              </div>
          )
      }
  }
  
  export default 컴포넌트명;
  ```

  - {}를 사용하여 값 전달

- class가 아닌 calssName 사용