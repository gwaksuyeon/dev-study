## Scroll

### scroll

- body나 부모에 overflow: hidden 존재x

- 스크롤바 보임 여부

  ```scss
  ::-webkit-scrollbar {
      display: none;
  }
  ```

- 컨텐츠 scroll시 onScroll 사용



### scrollTop

- window.scrollY 
- document.documentElement && document.documentElement.scrollTop
- document.body.scrollTop;



### onScroll - scrollTop

- e.target.scrollTop === e.target.scrollHeight - e.target.clientHeight;

