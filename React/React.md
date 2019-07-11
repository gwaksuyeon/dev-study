## React란

- javascript 프레임워크 중 하나
- 페이스북에서 제작
- virtual DOM 사용 
  - 가상의 DOM, 변화가 일어나면 기존의 DOM과 비교 후, 변화가 필요한 곳만 변화

### React project 시작

1. Node.js 설치

   - Webpack, Bable등을 사용하기 위함
   - https://nodejs.org/ko/download

2.  Yarn 설치

   - npm의 개선된 버전
   - 라이브러리를 설치하고 라이브러리의 버전을 관리함
   - https://classic.yarnpkg.com/en/docs/install#windows-stable

3.  create-react-app 설치 및 사용

   1. 리액트 앱을 만들어주는 create-react-app 설치

      ``` javascript
      yarn global add create-react-app
      ```

   2. 사용

      ``` javascript
      create-react-app 생성할 폴더명
      ```

4. 시작

   ``` javascript
   cd 생성할 폴더명 // 생성한 곳으로 이동
   yarn start
   ```

   