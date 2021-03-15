## SPA에서 SSR을 구축하지 않고 SEO 최적화



### SSR vs CSR

- SSR
  - 초기 로딩 속도 빠름
  - mata 태그가 미리 정의되어 SEO에 유리
- CSR
  - 로딩시 필요한 파일크기는 크지만 다 받으면 동적으로 빠르게 렌더링
  - 하나의 HTML파일로 모든 페이지 구성



### meta 태그 제어

- react-helmet 사용

-> 동적으로 메타태그가 변경되지 않음

-> react-snap 사용



### react-snap

```react
// package.json

...
...
"scripts": {
  ...
  ...
  "postbuild": "react-snap"
}
...
```

```react
// index.js
import React from 'react';
import { render, hydrate } from 'react-dom';
import App from './App';
import * as serviceWorker from './serviceWorker';
import { BrowserRouter } from 'react-router-dom';

const rootElement = document.getElementById('root');

if (rootElement.hasChildNodes()) {
  hydrate(
    <BrowserRouter>
      <React.StrictMode>
        <App />
      </React.StrictMode>
    </BrowserRouter>,
    rootElement
  );
} else {
  render(
    <BrowserRouter>
      <React.StrictMode>
        <App />
      </React.StrictMode>
    </BrowserRouter>,
    rootElement
  );
}

// If you want your app to work offline and load faster, you can change
// unregister() to register() below. Note this comes with some pitfalls.
// Learn more about service workers: https://bit.ly/CRA-PWA
serviceWorker.unregister();
```

빌드후

![image-20210315141657548](C:\Users\USER\AppData\Roaming\Typora\typora-user-images\image-20210315141657548.png)

