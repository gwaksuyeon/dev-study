# React-build
만들어진 결과물을 배포하기 위함
여기서는 github에 배포

## build 과정
1. gh-pages 설치
``` javascript
  yarn add gh-pages --dev
```
2. package.json 수정
``` javascript
{
  ...
  "homepage": "https://사용자아이디.github.io/프로젝트명",
  "script" : {
    ...
    "predeploy": "yarn build",
    "deploy": "gh-pages -d build",
  }
}  
```
3. yarn build && yarn run deploy
``` javascript
  yarn build
  yarn run deploy
```

## 404 Error
github-page는 SPA를 지원하지 않음. 따라서 router 사용시 추가 설정 필요

1. 404.html 추가
 - public 폴더에 404.html 추가
 ``` html
 <!DOCTYPE html>
  <html>
    <head>
      <meta charset="utf-8">
      <title>Single Page Apps for GitHub Pages</title>
      <script type="text/javascript">
        // Single Page Apps for GitHub Pages
        // https://github.com/rafrex/spa-github-pages
        // Copyright (c) 2016 Rafael Pedicini,  licensed under the MIT License
        //  ---------------------------------------------- ------------------------
        // This script takes the current url and  converts the path and query
        // string into just a query string, and then  redirects the browser
        // to the new url with only a query string  and hash fragment,
        // e.g. http://www.foo.tld/one/two?a=b& c=d#qwe, becomes
        // http://www.foo.tld/?p=/one/two&  q=a=b~and~c=d#qwe
        // Note: this 404.html file must be at least  512 bytes for it to work
        // with Internet Explorer (it is currently >  512 bytes)

        // If you're creating a Project Pages site  and NOT using a custom domain,
        // then set segmentCount to 1 (enterprise   users may need to set it to > 1).
        // This way the code will only replace the  route part of the path, and not
        // the real directory in which the app  resides, for example:
        //  https://username.github.io/repo-name/one/two?  a=b&c=d#qwe becomes
        // https://username.github.io/repo-name/? p=/one/two&q=a=b~and~c=d#qwe
        // Otherwise, leave segmentCount as 0.
        var segmentCount = 1;

        var l = window.location;
        l.replace(
            l.protocol + '//' + l.hostname + (l.port ?   ':' + l.port : '') +
            l.pathname.split('/').slice(0, 1 +  segmentCount).join('/') + '/?p=/' +
            l.pathname.slice(1).split('/').slice  (segmentCount).join('/').replace(/&/g,  '~and~') +
            (l.search ? '&q=' + l.search.slice(1) .replace(/&/g, '~and~') : '') +
            l.hash
        );
      </script>
    </head>
  <body>
  </body>
</html>
 ```
 
 2. public/index.html 코드 수정
 ``` html
<head>
  ...
  <!-- Start Single Page Apps for GitHub Pages -->
  <script type="text/javascript">
    // Single Page Apps for GitHub Pages
    // https://github.com/rafrex/spa-github-pages
    // Copyright (c) 2016 Rafael Pedicini, licensed under the MIT License
    // ----------------------------------------------------------------------
    // This script checks to see if a redirect is present in the query string
    // and converts it back into the correct url and adds it to the
    // browser's history using window.history.replaceState(...),
    // which won't cause the browser to attempt to load the new url.
    // When the single page app is loaded further down in this file,
    // the correct url will be waiting in the browser's history for
    // the single page app to route accordingly.
    (function (l) {
      if (l.search) {
        var q = {};
        l.search.slice(1).split('&').forEach(function (v) {
          var a = v.split('=');
          q[a[0]] = a.slice(1).join('=').replace(/~and~/g, '&');
        });
        if (q.p !== undefined) {
          window.history.replaceState(null, null,
            l.pathname.slice(0, -1) + (q.p || '') +
            (q.q ? ('?' + q.q) : '') +
            l.hash
          );
        }
      }
    }(window.location))
  </script>
</head>
 
 ```
 
 3. basename 설정
 - src/App.tsx 혹은 src/Root.tsx
 
 ``` javascript
  <BrowserRouter basename={"/프로젝트명"}>
    <App/>
  </BrowserRouter>
 ```