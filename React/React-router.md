## Router

### SPA(Single Page Application)

- 기존의 화면과 비교하여 변경된 부분만 렌더링

##### 장점

- 필요한 데이터만 전달받아 트래픽 낭비 방지
- 보여지는 속도가 빠름

##### 단점

- 앱의 규모가 커지면 자바스크립트 파일크기가 너무 커짐
  - code splitting으로 개선 가능



### 프로젝트 구성

#### 라이브러리 설치

``` react
yarn add react-router-dom
```



#### 기초설정

1. Root 컴포넌트 생성 - BrowserRouter 적용

   ``` react
   import React from 'react';
   import { BrowserRouter } from 'react-router-dom';
   import App from './App';
   
   const Root = () => {
       <BrowserRouter>
       	<App/>
       </BrowserRouter>
   }
   
   export default Root;
   ```

2.  index.tsx 수정

   ``` react
   import React from 'react';
   import ReactDOM from 'react-router-dom';
   
   import Root from './Root';
   
   ReactDOM.render(
       <Root/>, 
       document.getElementById('root')
   );
   ```

   

### Route

1. 기본 라우트 준비

   - src/pages/home/home.tsx

     ``` react
     import React from 'react';
     
     const Home = () => {
         return (
         	<div>
             	home
             </div>
         )
     }
     
     export default Home;
     ```

   - src/pages/about/about.tsx

     ``` react
     import React from 'react';
     
     const About = () => {
         return (
         	<div>
             	About
             </div>
         )
     }
     
     export default About;
     ```

2. Router 설정

   - src/App.tsx

     ``` react
     import React 'react';
     import { Route } from 'react-router-dom';
     
     import Home from 'page/home';
     import About from 'page/about';
     
     class App extends React.Component {
         render() {
             return (
             	<div id="App">
                     <Route exact={true} path="/home" component={Home} />
                     <Route exact={true} path="/about" component={About} />
                 </div>
             )
         }
     }
     
     export default App;
     ```

   - exact - 해당하는 URL이 정확하면 보여줌
   - patch - 해당 URL
   - component - 보여줄 component



### 라우트 파라미터

- history
  - push, replace를 통해 페이지 경로 이동 또는 앞뒤 페이지 전환 가능
- location
  - 현재경로에 대한 정보 가짐
  - URL쿼리 정보를 지님
    - /about?foo=bar
- march
  - 어떤 라우트에 매칭이 되었는지에 대한 정보
  - params 정보를 지님
    - /about/:name



### 라우트 이동

- Link 컴포넌트

  - 새로고침을 막고 화면 변환
  - src/pages/menu/menu.tsx

  ``` react
  import React from 'react';
  import { Link } from 'react-router-dom';
  
  const Menu = () => {
      return (
          <div>
          	<ul>
              	<li><Link to="/">Home</Link></li>
                  <li><Link to="/about">About</Link></li>
              </ul>
          </div>
      )
  }
  ```

- NavLink 컴포넌트

  - Link 컴포넌트와 비슷하지만 활성화 시 스타일 또는 클래스 지정 가능
  - src/pages/menu/menu.tsx

  ``` react
  import React from 'react';
  import { NavLink } from 'react-router-dom';
  
  const Menu = () => {
      const activeStyle = {
          color: 'green',
          fontSize: '14px',
      }
      return (
          <div>
          	<ul>
              	<li><NavLink to="/" activeStyle={activeStyle}>Home</NavLink></li>
                  <li><NavLink to="/about" activeStyle={activeStyle}>About</NavLink></li>
              </ul>
          </div>
      )
  }
  ```