## React-native

안드로이드, iOS를 동시에 개발할 수 있음



### 시작

1. expo-cli 설치

   ``` react
   yarn global expo-cli
   expo init 프로젝트명
   ```

2. yarn start / expo start
   - expo 어플을 깔아서 QR코드로 연결



### 규칙

1. felx-direction의 default는 column

   - 모바일은 대부분 위에서 아래로 구성되어 있기 때문
   - 웹의 default는 row

2. <View>는 <div>와 같은 역할

3. <Text>는 <span>,<p>과 같은 역할

4. style 설정

   ``` react
   <View style={styles.작성한 style명} />
   
   // or
   
   <View style={{flex: 1, backgroundColor: "blue"}}
   ```

5. 전체 공간을 다 차지하고 싶으면 felx: 1

6. width/height보다는 flex 사용 권장 

   - 각 모바일의 크기가 다르고, 회전할 때 마다 변경되기 때문