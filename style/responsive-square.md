## 반응형 정사각형 및 사각형

### 1. 정사각형

1. 배경만 있을 때

   ```scss
   .box {
       width: 20%;
       padding-bottom: 20%;
   }
   ```

2.  내용이 있을 때

   ```scss
   .box {
       width: 20%;
       position: relative;
       padding-bottom: 20%;
   }
   
   .contents {
       position: absolute;
   }
   ```

   

### 2. 직사각형

- padding-bottom 조정



### 3. flex-wrap

- flex-wrap: wrap 일 경우 내용이 적어 공간이 비어보이는 경우 사용

  ```scss
  align-content: flex-start
  ```

  