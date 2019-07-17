## React event 관리

- button, input, textarea등 상태 관리



### onClick

- 마우스 클릭 시 나타나는 이벤트

``` react
import React from 'react';

class Button extends React.Component {
    
    onClick = (e: React.MouseEvent) => {
        // 현재 클릭된 속성을 가져오는 이벤트
        const data = e.currentTarget.getAttribute('data-label');
        console.log(data)
    }
    
    render() {
        return (
        	<button
              type="button"
              data-label="button"
              onClick={this.onClick}
            >
                버튼
            </button>
        )
    }
}

export default Button;
```



### onChange

- 현재의 값을 읽어올 수 있음
- input, textarea, checkBox, radio 등에 사용

``` react
import React from 'react';

class Input extends React.Componet {
    
    onChange = (e: React.ChangeEvent<HTMLInputElement>) => {
        const value = e.currentTarget.value;
        console.log(value);
    }
    
    render() {
        return (
        	<form>
            	<input
                  palceholder="이름"
                  onChange={this.onChange}
                />
            </form>
        )
    }
}

export default Input;
```