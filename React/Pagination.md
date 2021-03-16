### Pagination

#### Props

```react
interface Props { 
	const totalCount; // 총 데이터 수
	const perLimit; // 한 페이지에 나타낼 데이터 수
	const pageLimit; // 한 화면에 나타낼 페이지 수
	const page; // 현재 페이지
	const onClickPage = (page: number) => void;
}
```



#### State

```react
const totalPageCount = Math.ceil(totalCount / perLimit); // 총 페이지 수

const [pageGroup, setPageGroup] = useState<number>(1);
const [lastPageNumber, setLastPageNumber] = useState<number>(0);
const [firstPageNumber, setFirstPageNumber] = useState<number>(0);
const [nextPageNumber, setNextPageNumber] = useState<number>(0);
const [prevPageNumber, setPrevPageNumber] = useState<number>(0);
```



#### Function

```react
const onCalculation = (currentPage: number) => {
    const pageGroupCnt = Math.ceil(currentPage / pageLimit);

    let last = pageGroupCnt * pageLimit;
    let first = (pageGroupCnt - 1) * pageLimit + 1;
    if (last > totalPageCount) {
        last = totalPageCount;
    }

    let next = last + 1;
    let prev = first - 1;

    setPageGroup(pageGroupCnt);
    setLastPageNumber(last);
    setFirstPageNumber(first);
    setNextPageNumber(next);
    setPrevPageNumber(prev);
};

const onClickPrevButton = () => {
    if (prevPageNumber === 0) {
        onClickPage(0);
        onCalculation(1);
    } else {
        onClickPage(firstPageNumber - pageLimit - 1);
        onCalculation(prevPageNumber);
    }
};

const onClickNextButton = () => {
    if (nextPageNumber > totalPageCount) {
        onClickPage(totalPageCount - 1);
        onCalculation(totalPageCount);
    } else {
        onClickPage(nextPageNumber - 1);
        onCalculation(nextPageNumber);
    }
};

const onClickStartButton = () => {
    onClickPage(0);
    onCalculation(1);
};

const onClickEndButton = () => {
    onClickPage(totalPageCount - 1);
    onCalculation(totalPageCount);
};

useEffect(() => {
    if (totalPageCount) {
        onCalculation(page + 1);
    }
    // eslint-disable-next-line react-hooks/exhaustive-deps
}, [totalPageCount]);
```



#### Page numbering

```react
<PagingListLayout>
    {new Array(lastPageNumber)
        .fill(0)
        .slice(firstPageNumber - 1)
        .map((obj: any, inx: number) => (
        <PagingButton
            key={`page-${inx}`}
            active={page === inx + (pageGroup - 1) * pageLimit}
            onClick={
                page === inx + (pageGroup - 1) * pageLimit
                    ? () => {}
                : () =>
                onClickPage(
                    inx + (pageGroup - 1) * pageLimit,
                )
            }>
            {inx + 1 + (pageGroup - 1) * pageLimit}
        </PagingButton>
    ))}
</PagingListLayout>
```

