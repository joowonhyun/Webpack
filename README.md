# Webpack

## Part1. Webpack 구조
Webpack이란?
웹팩은 오픈 소스 자바스크립트 모듈 번들러로써 여러개로 나누어져 있는 파일들을 하나의 자바스크립트 코드로 압축하고 최적화하는 라이브러리입니다.

Webpack의 장점
여러 파일의 자바스크립트 코드를 압축하여 최적화 할 수 있기 때문에 로딩에 대한 네트워크 비용을 줄일 수 있습니다.
 모듈 단위로 개발이 가능하여, 가독성과 유지보수가 쉽습니다.

Webpack의 구성요소

엔트리
의존성 그래프의 시작점을 웹팩에서는 엔트리(Entry)라고 한다. 
웹팩은 엔트리를 통해서 필요한 모듈을 로딩하고 하나의 파일로 묶는다. 
여러 개의 엔트리가 존재할 수 있다. 

아웃풋
엔트리에 설정한 자바스크립트 파일을 시작으로 하나로 묶는다. 그 후 번들된 결과물을 처리할 위치를
output에 기록한다. 

로더
웹팩은 오직 JavaScript와 Json만 이해할 수 있다. 
로더는 다른 Type의 파일(img, font, stylesheet 등)을 웹팩이 이해하고 처리 가능한 모듈(JavaScript와 Json)로 변환시키는 작업을 한다. 
(**웹팩이 웹 어플리케이션을 해석할 때 자바스크립트 파일이 아닌 웹 자원(HTML, CSS, IMG, 폰트 등)들을 변환할 수 있도록 도와주는 속성)

플러그인
로더가 파일 단위로 처리하는 반면 플러그인은 번들된 결과물을 처리한다. 
로더가 변환하는 동안 플러그인은 bundle optimization, asset management and injection of environment과 같은 일을 진행할 수 있다.
→ 플러그인은 항상 인스턴스로 생성하여 사용한다!

모듈
프로그램을 구성하는 구성 요소의 일부를 의미하며, 관련된 데이터와 함수들이 묶여서 모듈을 형성하고 파일 단위로 나뉘는 것이 일반적이다. 
모듈화 프로그래밍은 기능별로 파일을 나눠가며 프로그래밍을 하는 것으로 유지보수가 쉽다는 장점이 있다. 

## Part2. 폴더 및 파일 구조 생성


dist 폴더 생성 - index.html
	src 에 들어 있는 코드들이 배포를 위해서 정적인 에셋들로 모이게 되는 공간입니다. 
react를 사용할때는 npm run build 명령어를 사용해서 나오는 폴더와 같은 공간입니다.
결국은 dist에 있는 파일을 이용해서 화면에 UI나 기능들이 보이게 됩니다.

src 폴더 생성 - index.js
	애플리케이션을 위해 작성해야하는 코드는 이 source 폴더 안으로 들어갑니다.

webpack 설치
npm init -y (package-json 파일 생성)
npm i -D webpack webpack-cli
build 스크립트 설치 : scripts 안에 “build” : “webpack –mode production” 작성
→ npm run build 를 하면 dist 폴더 안에 main.js 파일이 생성
cf) 이미 만들어진 라이브러리 import 해보기 : npm install nanoid –save

## Part3. webpack 설정 파일 생성

webpack.config.js 파일 생성
path 경로 설정 : 파일이나 디렉터리의 경로를 다룰 때 Node,js에 있는 path모듈을 사용할 수 있습니다.
```
const path = require('path'); 
	
module.exports 생성
module.exports = {
  mode: 'development',
 
 //시작점
  entry: path.resolve(__dirname, 'src/index.js'),
  
//웹팩 작업을 통해 생성된 결과물
  output: {
    path: path.resolve(__dirname, 'dist'),
    filename: 'main.js'
  }
}
```
	
cf) 여러개의 entry point를 가질 경우
```
module.exports = {
  mode: 'development',
 
 //시작점
  entry: {
      main: path.resolve(__dirname, 'src/index.js'),
      .
      .
      .
  },
  
//웹팩 작업을 통해 생성된 결과물
  output: {
    path: path.resolve(__dirname, 'dist'),
    filename: 'main.js'
  }
}
```

Webpack Loader
css-loader (https://www.npmjs.com/package/css-loader)
css-loader는 @import 및 url()을 import/require()와 같이 해석하고 해결합니다.
 jsx처럼 자바스크립트를 이용한 마크업에 스타일링을 입힐 때는 import "style.css"와 같이 css 파일을 임포트 합니다.
 임포트 하는 것은 css 파일을 하나의 모듈로 취급하여 js 파일에서 불러 사용한다는 것입니다.

여기서 css 파일을 모듈로 부르기 위해서 사용하는 로더가 css-loader이다. 결론적으로는 js 파일에서 css 파일을 불러와서 사용할 수 있게 해주는 로더가 css-loader 입니다. 

style-loader (https://www.npmjs.com/package/style-loader)
css-loader를 이용해 웹팩 의존성 트리에 추가한 후에 css에 작성한 string 값들을 style-loader를 이용해서 돔에 <style></style>로 넣어줍니다.

style-loader는 css-loader를 이용해서 import 구문을 이용해 js 파일에 불러들여진 스타일 파일이 html 파일 안에 style 태그로 존재할 수 있게 합니다.

주로 사용하는 Styling로더
style-loader는 DOM에 스타일로 모듈 내보내기를 추가합니다.
css-loader는 리졸브된 가져오기로 CSS 파일을 로드하고 CSS 코드를 반환합니다.
less-loader는 LESS 파일을 로드하고 컴파일합니다.
sass-loader는 SASS/SCSS 파일을 로드하고 컴파일합니다.
postcss-loader는 PostCSS 을 사용해 CSS/SSS 파일을 로드하고 변환합니다.
stylus-loader는 Stylus 파일을 로드하고 컴파일합니다.

(Sass를 사용할 경우 예시) scss 파일 생성 및 import
styles 폴더 생성 - main.scss 파일생성
```
npm i -D css-loader style-loader sass sass-loader
```
webpack.config.js 파일 module 추가
```
 module : {
    (…생략…)
    rules: [
      {
        test: /\.s[ac]ss$/i,
        use: [
          // Creates `style` nodes from JS strings
          "style-loader",
          // Translates CSS into CommonJS
          "css-loader",
          // Compiles Sass to CSS
          "sass-loader",
        ]
      }
    ]
  }
```
- test : 로더를 적용할 파일 유형 (일반적으로 정규 표현식 사용)
- use: 해당 파일에 적용할 로더의 이름 배열 안에 있는 값들의 역순으로 로더들이 작동하게 되므로 style-loader  →  css-loader 순으로 작성해야함
- 작동 : css-loader →  style-loader 
- 작성 : style-loader →  style-loader
(style-loader는 css-loader가. css 확장자의 파일을 번들링에 포함한 후에 <style></style> 태그를 이용해 해당 css 값들을 html문서에 적용시킬 수 있기 때문에, 이 순서를 지켜야 합니다.)

HTML Webpack Plugin
플러그인
- 웹팩은 로더와 플러그인의 확장 기능이 있습니다. 
- 웹팩의 플러그인은 로더가 할 수 없는 다른 작업을 수행할 목적으로 제공됩니다. 
- 로더는 모듈을 output으로 만들어가는 과정에서 사용합니다. 
- 플러그인은 webpack으로 변환한 파일에 추가적인 기능을 더하고 싶을 때 사용합니다.
  (최종적인 결과물을 변형시킵니다.)
- 플러그인은 웹팩의 기본적인 동작에 추가적인 기능을 제공하는 속성입니다.

HTML Webpack Plugin 종류


HTML Webpack Plugin
HtmlWebPackPlugin 은 웹팩이 html 파일을 읽어서 html 파일을 빌드할 수 있게 해 준다. 
```
npm i -D html-webpack-plugin
```
```
plugins: [
    new HtmlWebpackPlugin({
      filename: 'index.html',
      template: 'src/index.html',
    }), 
  ],
```


플러그인을 다양한 용도로 Configuration에서 여러 번 사용할 수 있으므로 new 연산자로 호출하여 플러그인의 인스턴스를 생성해서 사용합니다.

이제는 template에 있는 src/index.html 에 있는 소스코드가  filename에 있는 dist/index.html 로 만들어집니다.

[상세 과정]
src/index.js에서 ./styles/main.scss가 import 되어있음
src/index.js → dist/main.js로,  src/index.html → dist/index.html 각각 빌드됨
빌드된 dist/main.js는 dist/index.html에서 로드됨
따라서 작성된 main.scss는 빌드 후 index.html에 정상적으로 반영될 수 있음



Webpack Cashing (웹팩 캐싱)
웹팩(Webpack) 컴파일로 생성된 파일에서 변경된 내용이 없다면 브라우저는 캐시 상태를 유지하고 그대로 사용하게 된다.

브라우저가 변경 사항을 확인하는 방법 중 하나는 파일 이름이기 때문에 파일을 생성할 때 해쉬값을 줄 수 있음.

output에 filename : ‘[name].[contenthash].js’ 추가
clean: true (**build시 생성된 아전 hash 파일 자동삭제 옵션)

```
module.exports = {
 (…생략…)
 output: {
   filename: '[name].[contenthash].js',
      clean: true,
   },
}
```

devServer 설정 방법
webpack.config.js 설정
```
module.exports = {
 (…생략…)
 devServer: {
    static: {
      directory: path.join(__dirname, 'dist'),
    },
    compress: true, //gzip 압축옵션
    port: 9000,
    open: true,
  },
}
```

package.json에 dev 명령어 추가
```
"scripts": {
    "dev": "webpack serve",
  },
```
→ webpack-dev-server' package 설치가 필요하므로 Y 누르면 됨


gzip 압축
압축은 대역폭을 절약하고 사이트 속도를 높이는 간단하고 효과적인 방법입니다.
원래는 구형 브라우저의 문제 때문에 자바스크립트 속도를 높일 때 gzip 압축을 권장하기가 힘들었습니다.
하지만 이제는 대부분 신형 브라우저를 사용하기 때문에 gzip 압축을 사용합니다.


일반 이전 index.html 대신 .zip 파일을 브라우저(index.html.zip)로 보낼 수 있다면 대역폭과 다운로드 시간을 절약할 수 있습니다.
브라우저는 압축 파일을 다운로드하고 압축을 풀고 페이지가 빠르게 로드되어 사용자에게 보여줄 수 있습니다.


Webpack Devtool
이 옵션은 소스 맵(source map)이 생성되는지 여부와 생성 방법을 제어합니다.

소스 맵(Source Map)
압축 파일 내의 코드를 소스 파일의 원래 위치로 다시 매핑하는 방법을 제공합니다. Chrome 및 Firefox 개발자 도구는 모두 소스 맵에 대한 기본 제공 지원과 함께 제공됩니다.

웹팩에서 소스맵 사용하는법
옵션 설정 https://webpack.js.org/configuration/devtool/
webpack.config.js 설정

devtool : "옵션",
```
module.exports = {
 (…생략…)
 devtool: "source-map"  
}
```
npm run build를 진행하면 .map 확장자의 소스맵 파일이 하나 더 생기게 됨



Babel Loader
ES6이상의 자바스크립트 코드는 인터넷 익스플로러 혹은 구버전 브라우저에서 지원이 안되는 경우가 있습니다.
그렇기 때문에 이러한 구버전 브라우저에서도 최신 자바스크립트 코드로 이루어진 앱을 이용할 수 있게 ES5 이하의 코드로 프랜스파일링 하도록 하는 기능이 바벨 입니다. 
그리고 웹팩으로 파일을 번들링(bundling)할 때도 바벨을 사용할수 있게 해주는 것이 babel-loader 입니다.
설치 : 
```
npm install -D babel-loader @babel/core @babel/preset-env
```
```
webpack.config.js 설정 (module - rules 안에 작성)
  module : {
    rules: [
    (…생략…)
      {
        test: /\.js$/,
        exclude: /node_modules/, //제외할 항목
        use: {
          loader: 'babel-loader',
          options: {
            presets: ['@babel/preset-env']
          }
        }
      },
    ],
  },
```

Resource Asset
	현재 png, svg, jpeg등의 에셋들을 사용하려고 하면 에러가 납니다. 
 
src/assets 폴더 생성

src/assets 폴더 안에 랜덤 이미지 하나 생성 (https://unsplash.com/ko)

src/index.html 파일 내 img src속성 없이 id, alt 속성만 입력

src/index.js에서 이미지 import 하기
 	import 3)에서 입력한 id from ‘이미지 경로’
→ import mainImage from './assets/image.jpg'

src/index.js 변수 선언 및 src 속성 지정
```
const img = document.getElementById('mainImage');
img.src = mainImage;
```

```
webpack.config.js 설정 (module - rules 안에 작성)
  module : {
    rules: [
    (…생략…)
      {
        test: /\.(png|svg|jpg|jpeg|gif)$/,
        type: 'asset/resource',
      },
    ],
  },
```
```
파일 이름 원래 이름으로 나오게 하기
  output: {
    assetModuleFilename: '[name][ext]'
  },
```

bundle analyzer (https://github.com/webpack-contrib/webpack-bundle-analyzer)
설치 : npm install webpack-bundle-analyzer –save-dev
webpack.config.js 파일 설정
```
const { BundleAnalyzerPlugin } = require('webpack-bundle-analyzer');
```
```
module.exports = {
  (…생략…)
  plugins: [
    new BundleAnalyzerPlugin()
  ]
}
```

이 모듈 사용의 장점
- 번들 내부에 무엇이 있는지 파악
- 크기를 가장 많이 차지하는 모듈 알아보기
- 최적화
- 축소된 번들을 지원
- 번들 모듈의 실제 크기를 얻기 위해 구문 분석
- gzipped 크기를 확인
