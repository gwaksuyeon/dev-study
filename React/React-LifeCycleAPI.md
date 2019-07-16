## LifeCycleAPI

- 컴포넌트가 브러우저에서 나타나고 사라질때, 업데이트 될 때 호출되는 API



### component 초기생성

- constructor

  - 컴포넌트 생성자 함수
  - 컴포넌트가 새로 만들어 질 때마다 호출

  ``` react
  constructor(props) {
      super(props);
  }
  ```

- componetDidMount

  - 컴포넌트가 화면에 나타나게 됐을 때 호출
  - 외부 라이브러리 연동이나 ajax 요청, DOM속성을 읽거나 직접 변경하는 작업 시 사용

  ``` react
  componentDidMount() {
      
  }
  ```



### component 업데이트

- shouldComponentUpdate

  - 컴포넌트를 최적화 하는 작업에서 유용하게 사용
  - 불필요한 렌더링 줄여줌
  - 조건이 false를 반환 하면 해당 조건에서는 render함수 호출하지 않음

  ``` react
  shouldComponetUpdate(nextProps, nextState) {
      /// 현재 props와 변경된 props의 값이 같지 않으면 업데이트
      if (this.props.check !== nextProps.check) {
          return true;
      }
      
      return false;
  }
  ```



- componetWillUpdate

  - shouldComponentUpdate에서 true를 반환했을때만 호출
  - 애니메이션 효과를 초기화 하거나 이벤트 리스터를 없애는 작업

  ``` react
  componentWillUpdate(nextProps, nextState) {
      
  }
  ```



- componetDidUpdate 

  - render()를 호출하고 난 뒤에 발생
  - 현재 props와 state가 변경되어 있는 상태
  - 이전 값 prevProps, prevState를 조회할 수 있음

  ``` react
  componentDidUpdate() {
      if (prevProps.check === this.props.check) {
          console.log('props가 이전값과 다르지 않음')
      } else {
          console.log('props가 이전값과 다름')
      }
  }
  ```



### component 제거

- componentWillUnmount

  - 등록했던 이벤트 제거, setTimeout 제거, 외부라이브러리 인스턴스 제거

  ``` react
  componentWillUnmount() {
      
  }
  ```