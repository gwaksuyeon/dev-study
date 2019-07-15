## Props

- 부모 컴포넌트가 자식 컴포넌트에게 전달하는 값
- 자식 컴포넌트에서는 props를 받기만하고 직접 수정은 불가능



``` react
// 부모 컴포넌트
import React from 'react';
import MyName from './MyName'; // 자식컴포넌트 불러오기

class App extends Component {
    render() {
        return (
        	<Myname
              name="리액트"    
            />
        )
    }
}

export default App;
```



#### 클래스형 컴포넌트에서의 Props 사용

``` react
// 자식 컴포넌트
import React from 'react';

// props의 타입을 지정
interface IProps{
    name: string;
}
class MyName extends React.Componet<IProps> {
    render() {
        const { name } = this.props;
        return (
        	<div>
            	안녕하세요 제 이름은 {name}입니다.
            </div>
        )
    }
}

export default MyName;
```



#### 함수형 컴포넌트에서의 Props 사용

``` react
import React from 'react';

// props의 타입을 지정
interface IProps {
    name: string;
}
const MyName = ({
    name,
}: IProps) => {
    return (
    	<div>
        	안녕하세요 제 이름은 {name}입니다.
        </div>
    )
}

export default Myname;
```



## State

- 동적인 데이터를 다룰 때 사용
- state의 값을 변경할 땐 this.setState를 꼭 사용

``` react
import React from 'react';

class Counter extends React.Component {
    state = {
        number: 0,
    }

	// 숫자 증가
	increaseNumber = () => {
        const { number } = this.state;
        this.setState({
            number: number + 1;
        })
    }
    
    // 숫자 감소
    decreaseNumber = () => {
        const { number } = this.state;
        this.setState({
            number: number - 1;
        })
    }
    
    render() {
        const { number } = this.state;
        return (
        	<div>
            	<h2>카운터</h2>
                <p>값: {number}</p>
                <button onClick={this.increaseNumber}>+</button>
                <button onClick={this.decreaseNumber}>-</button>
            </div>
        )
    }
}

export default Counter;
```

