## Instargram / naver live shopping Like기능



#### 고민할점

1. 클릭할 때 마다 컴포넌트 실행
   - 클릭할 때 page수 증가로 실행
2. 애니메이션이 끝나지 않아도 컴포넌트 실행가능
   - onAnimationEnd 함수 사용
3. 곡선으로 컴포넌트 이동
   - y축 값은 0%, 100%에서만 실행 x축만 변경
4. 최적화
   - 해결해야할 문제
   - 어느 시점이 지나면 버벅거리는 현상



#### 코드

1.좋아요 버튼

- clickCount는 향후 10개 단위로 클릭할 때 표시하기 위함

```react
import React, { useState } from 'react';
import styled from 'styled-components';
import loadable from '@loadable/component';

import SvgLike from './SvgLike';

const FloatingLike1 = loadable(
    () => import('components/common/pc/like/FloatingLike'),
);
const FloatingLike2 = loadable(
    () => import('components/common/pc/like/FloatingLike2'),
);
const FloatingLike3 = loadable(
    () => import('components/common/pc/like/FloatingLike3'),
);
const FloatingLike4 = loadable(
    () => import('components/common/pc/like/FloatingLike4'),
);

const Like: React.FC = () => {
    const [clickCount, setClickCount] = useState<number>(0);
    return (
        <Container>
            <FloatingLayout>
                {new Array(isClick)
                    .fill(0)
                    .map((obj: any, inx: number) =>
                        inx % 4 === 0 ? (
                            <FloatingLike1 key={`floating1-${inx}`} />
                        ) : inx % 4 === 1 ? (
                            <FloatingLike2 key={`floating2-${inx}`} />
                        ) : inx % 4 === 2 ? (
                            <FloatingLike3 key={`floating3-${inx}`} />
                        ) : (
                            <FloatingLike4 key={`floating4-${inx}`} />
                        ),
                    )}
            </FloatingLayout>
            <ButtonLayout onClick={() => setClickCount(clickCount + 1)}>
                <SvgLike />
            </ButtonLayout>
        </Container>
    );
};

const Container = styled.div`
    position: fixed;
    bottom: 40px;
    left: 40px;
`;

const ButtonLayout = styled.div`
    display: flex;
    justify-content: center;
    align-items: center;
    width: 40px;
    height: 40px;
    background: rgba(0, 0, 0, 0.5);
    border-radius: 100%;
    cursor: pointer;

    svg {
        width: 20px;
        height: 20px;
    }

    &:active svg {
        width: 16px;
        height: 16px;
    }
`;

const FloatingLayout = styled.div`
    width: 105px;
    height: 200px;
    position: absolute;
    left: 0;
    bottom: 40px;
    background: transparent;
`;

export default Like;

```



2.Floating component

```react
// 파랑+노랑
import React from 'react';
import styled, { css, keyframes } from 'styled-components';

import SvgLike from './SvgLike';

const FloatingLike: React.FC = () => {
    const onAnimationEnd = (e: React.AnimationEvent<HTMLSpanElement>) => {
        e.currentTarget.remove();
    };

    return (
        <>
            <Container onAnimationEnd={onAnimationEnd}>
                <SvgLike />
            </Container>
            <CircleLayout onAnimationEnd={onAnimationEnd}>
                <CircleLong rotate={0} />
                <Circle rotate={22.5} />
                <CircleLong rotate={45} />
                <Circle rotate={67.5} />
                <CircleLong rotate={90} />
                <Circle rotate={112.5} />
                <CircleLong rotate={135} />
                <Circle rotate={157.5} />
                <CircleLong rotate={180} />
                <Circle rotate={202.5} />
                <CircleLong rotate={225} />
                <Circle rotate={247.5} />
                <CircleLong rotate={270} />
                <Circle rotate={292.5} />
                <CircleLong rotate={315} />
                <Circle rotate={337.5} />
            </CircleLayout>
        </>
    );
};

const Animation = () => keyframes` 
    0% {
        bottom: 0;
        left: 13px;
        transform:  scale(0);
    }

    10% {
        transform:  scale(1.5);
    }


    30% {
        left: 25px;
        transform:  scale(1);
    }


    60% {
        left: 1px;
        transform:  scale(1.5);
    }

    90% {
        transform: scale(1);
    }

    100% {
        bottom: 200px;
        left: 13px;
        transform: scale(0);
    }
`;

const CircleLayoutAnimation = () => keyframes`
    0% {
        transform: scale(0.7);
        opacity: 0;
    }

    10%{
    opacity: 1;

    }

    100% {
        transform: scale(1);
    }
`;

const CircleLongAnimation = (rotate: number) => keyframes`
    0%{
        height: 3.5px;
    }

    10%{
    opacity: 1;

    }

    100%{
        height: 2px;
        top: ${
            rotate === 0
                ? '-10%'
                : rotate === 45
                ? '10%'
                : rotate === 90
                ? '50%'
                : rotate === 135
                ? '85%'
                : rotate === 180
                ? '110%'
                : rotate === 225
                ? '85%'
                : rotate === 270
                ? '50%'
                : '10%'
        };

        left: ${
            rotate === 0
                ? '50%'
                : rotate === 45
                ? '90%'
                : rotate === 90
                ? '105%'
                : rotate === 135
                ? '90%'
                : rotate === 180
                ? '50%'
                : rotate === 225
                ? '10%'
                : rotate === 270
                ? '-5%'
                : '10%'
        };
    }
`;

const CircleAnimation = () => keyframes`
    0%{
        opacity: 0;
    }

    40% {
        opacity: 1;
    }

    70% {
        opacity: 0;
    }

    100%{
        opacity: 1;
    }
`;

const Container = styled.span`
    display: inline-block;
    width: 14px;
    height: 14px;
    position: absolute;
    animation: ${Animation} 1.2s ease-in-out forwards;
    svg {
        width: 100%;
    }
`;

const CircleLayout = styled.div`
    width: 24px;
    height: 24px;
    position: absolute;
    bottom: 200px;
    left: 13px;
    opacity: 0;
    animation-name: ${CircleLayoutAnimation};
    animation-duration: 1s;
    animation-timing-function: ease-in-out;
    animation-delay: 1.2s;
`;

const CircleLong = styled.p<any>`
    width: 2px;
    height: 4.5px;
    position: absolute;
    background: #3bbeff;
    transform: ${(props: any) => `rotate(${props.rotate}deg)`};
    opacity: 0;
    animation-name: ${(props: any) => CircleLongAnimation(props.rotate)};
    animation-duration: 1s;
    animation-timing-function: ease-in-out;
    animation-delay: 1.2s;

    ${(props: any) =>
        props.rotate === 0
            ? css`
                  top: 0;
                  left: 50%;
              `
            : props.rotate === 45
            ? css`
                  top: 15%;
                  left: 84%;
              `
            : props.rotate === 90
            ? css`
                  top: 50%;
                  left: 100%;
              `
            : props.rotate === 135
            ? css`
                  top: 80%;
                  left: 84%;
              `
            : props.rotate === 180
            ? css`
                  top: 100%;
                  left: 50%;
              `
            : props.rotate === 225
            ? css`
                  top: 80%;
                  left: 16%;
              `
            : props.rotate === 270
            ? css`
                  top: 50%;
                  left: 0%;
              `
            : css`
                  top: 15%;
                  left: 16%;
              `}
`;

const Circle = styled.p<any>`
    width: 2px;
    height: 2px;
    position: absolute;
    background: #ffec00;
    transform: ${(props: any) => `rotate(${props.rotate}deg)`};
    opacity: 0;
    animation-name: ${CircleAnimation};
    animation-duration: 1s;
    animation-timing-function: ease-in-out;
    animation-delay: 1.2s;

    ${(props: any) =>
        props.rotate === 22.5
            ? css`
                  top: 8%;
                  left: 67%;
              `
            : props.rotate === 67.5
            ? css`
                  top: 35%;
                  left: 92%;
              `
            : props.rotate === 112.5
            ? css`
                  top: 70%;
                  left: 92%;
              `
            : props.rotate === 157.5
            ? css`
                  top: 92%;
                  left: 67%;
              `
            : props.rotate === 202.5
            ? css`
                  top: 92%;
                  left: 33%;
              `
            : props.rotate === 247.5
            ? css`
                  top: 70%;
                  left: 8%;
              `
            : props.rotate === 292.5
            ? css`
                  top: 35%;
                  left: 8%;
              `
            : css`
                  top: 8%;
                  left: 33%;
              `}
`;

export default FloatingLike;

```

