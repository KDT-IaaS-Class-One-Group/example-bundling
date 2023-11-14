# bundling

- 번들링은 마치 '포장제품'을 만드는 것과 같다.
- 번들링을 하면 여러개의 파일을 하나의 파일로 만들어준다.

- 여러개의 파일을 하나로 만드는 일뿐만 아니라, 최적화와 같은 일들도 함께 수행한다.

## 코드라기보다, git과 같은 소프트웨어에 가깝다.

- 패키지 매니저를 통해 설치해주는 과정으로 '개발환경'을 구축하는 부분에 해당한다.
- 보통의 경우 프로젝트 최초에 진행하는 편이며, '완료'된 이후에는 특별한 경우가 아니라면 거의 변동하지 않는 편이다.

## 가장 많이 사용하는 SW는

- webpack
- parcel
- rollup
- vite

총 네가지 정도로, 가장 많은 설정과 커스텀, 사용자풀을 지닌 webpack과, 빠른속도를 자랑하는 vite가 눈여겨 볼만한 도구이다.
본 튜토리얼은 webpack을 기준으로 진행한다.
설정하는 일이 번거롭긴 하지만 상당히 많은 기능을 지원하고 있기 때문이다.

## 설치 작업

npm에서 손쉽게 설치할 수 있으며, 웹팩은 '개발도구'이기 때문에, 배포용 코드와는 전혀관계가 없으므로, devDependencies에 설치해준다.

배포용 의존성 관리, dependencies와 다르다.

1. `npm init` 명령으로 초기화 한다.
2. `npm install --save-dev webpack` 소프트웨어본체를 설치한다.
    특히 이 부분이 특이한데, 소프트웨어 자체를 설치하는 일과 아래의 cli 인터페이스를 설치하는 일이 분리되어있다.

3. `npm install --save-dev webpack-cli` 명령으로 webpack-cli를 설치한다.
    이것은 webpack을 cli로 사용할 수 있게 해주는 도구이며, cli를 사용하지 않는다면 필요하지 않다.(개발자는 보통 99%의 경우 cli를 사용한다.)

# 훈련간 컨벤션

1. 많은 Webpack 정보나 튜토리얼들이 commonJS 방식을 소개하고 있으나 본 예제에서는 ESM 방식으로 진행하여 작성방식을 훈련한다.
2. webpack 공식 웹사이트와 기타 정보에있는 코드를 그대로 사용하면 에러가 발생되므로, 맹목적인 카피/페이스트는 지양한다.

---

# 정상적으로 설치 되었다면, package.json 파일에 다음과 같은 개발의존모듈이 설치된 것을 확인 할 수 있다.
  "devDependencies": {
    "webpack": "^5.89.0",
    "webpack-cli": "^5.1.4"
  }
  번들러는 express나 react와는 전혀 관계 없는 별도의 도구이므로 구분할 필요가 있다.


# webpack 초기화 하기
1. `npx webpack-cli init` 명령으로 초기화를 진행한다.

npm이 패키지 매니저라면, npx는 패키지 실행기이다.
위에서 설치한 webpack-cli 라는 개발 도구를 실행하기 위해 execute, 실행한다는 의미와 합성되어 npx 명령을 사용한다.

2. init 초기화를 진행하면 cli 환경에서 생성기를 설치할 것인지를 물으며 설치를 진행한다.
"
[webpack-cli] Would you like to install '@webpack-cli/generators' package? (That will run 'npm install -D 
@webpack-cli/generators') (Y/n) 
"

3. generators 플러그인이 설치 되면 package.json 에는 다음과 같은 내용이 추가된다.
    "@webpack-cli/generators": "^1.1.0",
    @ atSign은 패키지의 추가적인 플러그인을 의미한다.

4. 다음 질문으로 다음과 같은 질문지를 확인 할 수 있다.

? Which of the following JS solutions do you want to use? 
> none
  ES6
  Typescript

  현재는 ES6를 사용하고 있으므로, ES6를 선택한다.
  추후에는 typescript로 설치하게 될 예정

5. Do you want to use webpack-dev-server? (Y/n) 

개발용 서버를 생성하겠느냐는 질문으로, 서빙작업이 익숙해지거나 간단한 테스트를 진행할 땐 간편하게 사용할 수 있지만, 실제 배포용으로는 사용하지 않는다. 그러므로 n No를 선택한다.

6. Do you want to simplify the creation of HTML files for your bundle? (Y/n)

파일을 단순하게 만들 것이기 때문에 Y

7. ? Do you want to add PWA support? (Y/n)

PWA 라는(프로그래시브웹) 지원하느냐의 물음, 프로젝트의 특성이 PWA기술을 사용하지 않는다면,
n No를 선택한다.

8. css 셋팅을 물어본다.
   Which of the following CSS solutions do you want to use? (Use arrow keys)
> none
  CSS only
  SASS
  LESS
  Stylus

적절한 것을 선택하면 되겠지만, 현재는 none으로 진행(추후에 필요하면 추가할 예정)

9. ? Do you like to install prettier to format generated configuration? (Y/n) 
위의 설치 질답을 통한 것으로, webpack을 정의할 것인지 물음이므로 y를 선택한다.

10. ? Pick a package manager: (Use arrow keys)
> npm
  yarn
  pnpm

node package manager를 선택한다.
npm은 일반적인 패키지 매니저이며, yarn은 페이스북에서 만든 패키지 매니저이다.
pnpm은 npm의 향상된 버전으로, npm보다 빠르고, 효율적이다.(사용률이 낮다.)

11. 선택하면 충돌이 일어나는데, 
[webpack-cli] ℹ INFO  Initialising project...
 conflict package.json
? Overwrite package.json? (ynarxdeiH) 

y를 선택하면, package.json 파일이 덮어씌워진다. (프로젝트 시작인 경우, 추후에 설정을 세부적으로 할것이기 때문에 크게 문제되지 않는다.)

README.md 파일도 함께 물어보며, 적절한 경우 y를 선택한다.

12. webpack.config.js과 작동하는 주요 파일들이 생성된 것을 확인할 수 있다.
    
# 초기설정은 입맛에 맞지 않기 때문에,

1. webpack.config.js 파일에서 적절하게 수정을 진행해야 한다.
- 현재 프로젝트의 문제는 ESM 방식이 아닌, CommonJS 방식으로 작성되어 있다는 것이다.
- 다시한번 작성하는 형태로 리팩토링을 진행해야 한다.

