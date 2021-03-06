# 익스프레스 (ExpressJS) 기초

### 익스프레스 (ExpressJS)
- 익스프레스: Node.JS로 만들어진 웹 프레임워크 ([공식 사이트](https://expressjs.com/))

### 어플리케이션
- 어플리케이션: 익스프레스 객체(인스턴스)
```
const express = require('express');
const app = express(); // app이 어플리케이션 객체
```
- 어플리케이션의 역할
  1. 서버에 필요한 기능인 미들웨어 추가
  2. 라우팅 설정
  3. 서버를 요청 대기 상태로 생성
  ```
    app.listen(3000, function() {
      console.log('Server is running');
    })
  ```
  
### 미들웨어
- 미들웨어: 함수들의 연속 (use 함수를 사용하여 미들웨어 추가)
- 미들웨어의 파라미터
  일반 미들웨어 | 에러 미들웨어
  ---------- | ----------
  (request, response, next) | (error, request, response, next)
- 미들웨어는 순차적으로 실행 (다음 함수를 실행시키기 위하여 next 함수를 호출해야 함)
  ```
    function logger(req, res, next) {
      console.log("i am logger");
      next();
    }
    
    function logger2(req, res, next) {
      console.log("i am logger2");
      next(); 
    }
    
    app.use(logger); // use 함수를 사용하여 logger 미들웨어 추가
    app.use(logger2);
  ```
  실행결과
  ```
    i am logger i am logger2
  ```
- 서드파티 미들웨어 사용하기 
  - npm 사이트를 참조하여 미들웨어 install 하기 ([npm 공식사이트](https://www.npmjs.com/))
  ```
    $ npm install morgan // 1. npm install 을 사용하여 미들웨어 설치
  ```
  ```
    const express = require('express');
    const morgan = require('morgan'); // 2. 미들웨어 가져오기
    const app = express();
    
    app.use(morgan('dev')); 3. 미들웨어 추가하기
  ```
- 에러 미들웨어 사용하기
  ```
    const express = require('express');
    const app = express();
    
    function commommw(req, res, next) {
      console.log('commonmw');
      next(new Error('error ouccered'));
    }
    
    function errormw(err, req, res, next) { // 에러 미들웨어
      console.log(err.message);
      next();
    }
    
    app.use(commonmw);
    app.use(errormw);
  ```
  실행결과
  ```
    commonmw error ouccered
  ```
