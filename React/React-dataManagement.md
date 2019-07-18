## React 데이터 관리(배열, 리스트)

- 기존의 데이터를 수정하면 안되며 불변성을 유지를 해야함
  - 원본은 수정하면 안된다는 말



### Immutable.js

- 데이터의 불변성을 쉽게 작성할 수있는 라이브러리

#### Immutable의 규칙

1. 객체는 Map
2. 배열은 List
3. 설정(변경)할 땐 set
4. 값을 읽어올 땐 get
5. 읽은 후 설정(변경)할 땐 update
6. 깊숙한 곳에 있는것을 설정/읽어올 땐 In을 붙임
   - setIn, getIn, updateIn
7. 일반 자바스크립트 객채로 변환할땐 toJS 사용
8. List엔 배열 내장함수와 비슷한 함수 존재
   - push, slice, filter, sort, concat...
9. 특정 key를 지울 땐 delete 사용



#### list 생성

``` react
state = {
    data: Data({
        user: List([
            User({
                id: 1,
                username: 'suyeon',
            }),
            User({
                id: 2,
                username: 'suyeon2',
            }),
        ])
    })

}
```



#### get, getIn 대신 Record 사용

```react
import React from 'react';
import { Map, List, Record } from 'immutable';

const User = Record({
    id: null,
    username: null
})

const Data = Record({
    users: List();
})
```