### 업데이트 될때만 api 호출

```react
import React,{useEffect,useRef} from 'react';

const Test: React.FC = () => {
    const initRef = useRef<any>(null);
    
       useEffect(() => {
            if (initRef.current) {
                initRef.current = false;
            } else {
                업데이트할 함수
            }

        }, [업데이트할 변수]);
    return(
    	<div>hihi</div>
    )
}
```

