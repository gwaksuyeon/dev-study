## TDD(Test Driven Development) - 테스트 주도 개발

- 테스트가 개발을 이끌어 나가는 형태의 개발론
- 선 테스트 코드 작성, 후 구현



### TDD의 3가지 절차

1. 실패
   - 실패하는 테스트 케이스를 먼저 만든다
   - 가장 먼저 구현할 기능 하나씪 테스트 케이스 작성
2. 성공
   - 코드를 작성하여 테스트를 통과
3. 리팩토링
   - 중복되는 코드나 개선할 방법이 있다면 리팩토링 진행



### TDD의 장점

- 프로젝트의 퀄리티를 높이기에 좋은 환경 구성
- 협업할 때 매우 중요
- 버그에 시간 낭비 최소화
- 구현한 기능이 요구사항을 충족하는지 쉽게 확인 가능



### React-testing-library

- Enzyme와 달리 모든 테스트를 DOM위주로 진행
- props / state 조회하지 않음



#### 설치

``` react
yarn add @testing-library/react
yarn add @testing-library/jest-dom
```



#### 기초설정

src/setupTest.js

```react
import '@testing-library/react/cleanup-after-each';
import '@testing-library/jest-dom/extend-expect';
```

- cleanup-after-each
  - 각 테스트 케이스가 끝날 때 마다 기존 가상화면에 남아있는 UI정리
- jest-dom/extend-expect
  - jest에서 DOM관련 matcher를 사용 가능하게 함



#### 코드작성

```react
import React from 'react';
import { Router } from 'react-router-dom'
import { render, fireEvent } from '@testing-library/react'
import { createMemoryHistory } from 'history'
import GoLoginBtnContainer from './GoLoginBtn';

// router 설정
const renderWithRouter = (component: React.ReactNode) => {
  const history = createMemoryHistory()
  return { 
      ...render (
      <Router history={history}>
          {component}
      </Router>
    )
  }
}

describe('<GoLoginBtnContainer />', () => {
  // snapshot 여부
  it('matches snapshot', () => {
    const utils = renderWithRouter(<GoLoginBtnContainer text="Login" />);
    expect(utils.container).toMatchSnapshot();
  });

  // text가 있는지
  it('has a text', () => {
    const utils = renderWithRouter(<GoLoginBtnContainer text="Login" />);
    utils.getByText('Login');
  });

  // click 함수가 있는 지
  it('onClick', () => {
    const utils = renderWithRouter(<GoLoginBtnContainer text="Login" />);
    const text = utils.getByText('Login');

    fireEvent.click(text);
  })
});
```
