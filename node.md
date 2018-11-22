# NPM 프로젝트 시작하기

`NPM` : 자바스크립트를 위한 공유, 코드 재사용을 위한 라이브러리들 공유
Npm init 으로 새 프로젝트 생성
Node based webserver 중 하나인 express 

`Npm install express —save`

설치하면 Node_modules 폴더가 생기고 package.json에 express가 자동으로 업데이트 됨
—save 추가 시 설치된 항목을 자동으로 pakage.json에 담아준다. 이는 package.json을 통해 파일을 받는 사람이 같은 환경을 설정할 수 있게 해줌.

# Express 기반 웹서버 구동

```
var express = require('express'); // (1)
var app = express();
app.listen(3000, function(){ // (2)
    console.log("start: express server on port 3000")
});

console.log('end of Server code...')
```

(1) `require`를 통해 node_modules에 있는 express 관련 파일을 모두 가져온다.
     express 내의 함수를 가져온다고 생각하면 되는데 express 자체가 함수이므로 바로 실행된다.
(2) express를  실행함으로서 listin 이라는 함수가 실행 됐을 때 3000 이라는 포트를 기반으로 함수를 실행시켜 준다.
    포트값을 변경하여 특정한 포트로 서버를 변경 가능하다.

```
var express = require('express');
var app = express();
app.listen(3000, function(){
    console.log("start: express server on port 3000")
});

for(var i=0; i<10000; i++) {
    console.log('end of Server code...')
}
```

노드는 비동기로 동작 되기때문에 위의 코드를 실행시 `enn of server code` 메시지가 먼저 뜬다.
서버가 동작이 될때까지 기다리는 것이 아니라 바로 아래 라인을 실행 시킴.
유의미한 코드는 아니고 동기가 먼저 실행 되는 것만 확인하는 코드.


노드의 파일 변화를 감지해서 자동으로 내렸다가 올려주는 패키지로 Nodemon이 있다.
`npm install nodemon --save -g`

디렉토리에서만 설치 시 디렉토리에서만 사용 가능하지만 -g 옵션 사용시 pc의 어느 디렉토리에서나 사용할 수 있다. 
콘솔에 `nodemon app.js(실행파일)`을 실행시켜 코드의 변경에 따라 실시간으로 서버가 수정되고 자동으로 실행되는 것을 확인 할 수 있다.
