## Paging

- 스크롤이 어느정도 위치에 도달하거나 더보기 버튼 등을 클릭하여 다음 리스트의 내용을 봄



```REACT
import React,{ useState, useEffect } from 'react';
import { ApiData } from './api';
import { useScroll } from './hooks';

const Page: React.FC = () => {
    const { isScrollEnd } = useScroll();
    const [data, setDate] = useState<object[]>([]);
    const [page, setPage] = useState<number>(0);
    const [isEndPage, setIsEndPage] = useState<boolean>(false);
    const [isPaging, setIsPaging] = useState<boolean>(false);
    
    const onRefresh = () => {
        setPage(0);
        setIsEndPage(false);
        setIsPaging(true);
        setData([]);
    }
    
    const onData = (add: boolean = false) => {
        setIsPaging(true);
        ApiData({
            page: add ? page : 0;
        }).then((res) => onCheckEndPage(add, res));
        
        if (add) {
            setPage(page + 1);
        } else {
            setPage(1);
        }
    };
    
    const onCheckEndPage(add: boolean, res: any) => {
        if (res.length < 10) {
            setIsEndPage(true);
        }
        
        setData(add ? data.concat(res) : res);
        setTimeout(() => {
            setPaging(false);
        }, 100);
    };
    
    useEffect(() => {
        onRefresh();
        setTimeout(() => {
            onData();
        }, 100)
    }, []);
    
    useEffect(() => {
        if (isScrollEnd && !isPaging && !isEndPage) {
            onData(true);
        }
    }, [isScrollEnd]);
    
    return (
    	<div>
        	{data.map((obj:any, inx: number) => (
            	<p>data 반복</p>
            ))}
        </div>
    )
}
```



#### Scroll 최하단 체크

```REACT
import { useState, useEffect, useMemo } from 'react';

export const useScroll = () => {
    const [scrollTop, setScrollTop] = useState<number>(0);
    const [scrollHeight, setScrollHeight] = useState<number>(0);
    const [innerHeight, setInnerHeight] = useState<number>(0);
    const [prevScroll, setPrevScroll] = useState<number>(0);
    const isScrollEnd = useMemo(
        () => scrollHeight !== 0 && scrollHeight - innerHeight - scrollTop < 1,
        [scrollHeight, innerHeight, scrollTop],
    );
    const onScroll = () => {
        setInnerHeight(window.innerHeight);
        setScrollHeight(document.body.scrollHeight);
        setPrevScroll(scrollTop);
        const top =
            window.scrollY ||
            (document.documentElement && document.documentElement.scrollTop) ||
            document.body.scrollTop;
        setScrollTop(top);
    };

    useEffect(() => {
        window.addEventListener('scroll', onScroll, false);

        return () => {
            window.removeEventListener('scroll', onScroll, false);
        };
    });

    return {
        scrollTop,
        scrollHeight,
        innerHeight,
        prevScroll,
        isScrollEnd,
    };
};
```

