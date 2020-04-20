## Styled-Components

css코드를 사용하여 자바스크립트 파일안에 스타일을 선언하는 'CSS-in-Js'방식



### sass(scss) 문제점

1. 정해진 가이드가 없으면 구조가 복잡
2. CSS 클래스명에 대한 고민
3. 스크롤 지옥
4. CSS 클래스로 조건부 스타일링



### Styled-Components

#### 설치

```react
yarn add styled-components
```



#### 기존 css와의 비교

##### 순수 css

``` react
title {
    font-size: 15px;
    color: white;
}
...
<h1 className="title">Hello World</h1>
```

##### Styled-Components

``` react
import styled from 'styled-components';
...
// 클래스명 = styled.태그명
const Title = styled.h1`
	font-size: 15px;
	color: white;
`;
...
<Title>Hello World</Title>
```



#### Props를 통한 조건부 스타일링

- props로 조건부 스타일링 가능
- scss와 같이 &사용 가능

``` react
import React from 'react';
import styled from 'styled-components';

const Button = styled.button`
	color: white;
	min-width: 120px
	background-color: ${props => props.color || "blue"};
	&:hover {
		background-color: black;
	}
`;

const App = () => {
    return (
    	<Button color="red">버튼</Button>
    )
}
```



#### 컴포넌트 재활용

``` react
const Button = styled.button`
	background-color: yellow;
	font-size: 14px;
`;

const SmallButton = styled(Button)`
	background-color: blue;
`;
```



#### Mixin

``` react
const flexRow = css`
	display: flex;
    flex-direction: row;
	flex-wrap: none;
`;

const Nav = styled.nav`
	${flexRow};
	background-color: red;
`;
```