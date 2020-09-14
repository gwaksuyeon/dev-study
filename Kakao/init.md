## Kakao init

- 전역을 통틀어 한번만 실행되어야 함



#### public/index.html

- window.Kakao를 사용하지 않고 바로 init 가능
- 불필요한 페이지에서도 로드되어 비효율적
- lighthouse 점수 감소



#### useEffect

- 해당 페이지에서만 로드
- lighthouse 점수 향상
- window.Kakao 사용해야함

``` react
import React, {useEffect} from 'react';

// 전역으로 window.Kakao 설정
declare global {
    interface Window {
        Kakao: any;
    }
}

useEffect(() => {
    // script 태그 추가
    const script = document.createElement('script');
    script.async = true;
    script.src = 'https://developers.kakao.com/sdk/js/kakao.min.js';

    document.head.appendChild(script);

    setTimeout(() => {
        // 키값 등록 여부 체크
        if (!window.Kakao.isInitialized()) {
            window.Kakao.init('고유 키값');
        }
    }, 100);
}, []);

const KakaoTest: React.FC = () => {
    return (
        <p>카카오 테스트</p>
    )
}
export default KakaoTest;
```

