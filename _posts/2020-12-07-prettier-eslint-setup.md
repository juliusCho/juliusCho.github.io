---
layout: post
title: 'VSCode에서 Typescript프로젝트 Prettier + ESLint 자동화 설정하기'
categories: [CodeQuality, Frontend]
featured: true
beforetoc: '** 해당 포스팅은 제가 찾은 정보들을 바탕으로 작성된 것이며, 정보가 빈약하거나 오류가 있을 수 있습니다. 댓글로 지적 & 수정요청 해주시면 너무나 감사하겠습니다!'
---

`Prettier + ESLint` 로 Javascript 코드 포멧팅과 품질 관리를 하는 사용자가 한국에도 아주 많아지고 있죠. 그리고 점차 프론트앤드 서비스를 별도의 마이크로 앱으로 설계하는 경우가 많아짐과 동시에 VSCode와 같은 빠르고 경량의 IDE의 인기가 높아졌습니다. 저도 IntelliJ를 사용하다 얼마전부터 VSCode로 넘어왔는데, `무료 IDE임에도 불구하고 엄청난 plugin 생태계` 덕분에 값비싼(?) Jet Brains 사의 제품들과 비교해도 손색없는 사용도를 보여줘서 만족하고 있습니다.

그래서 이번엔 `VSCode에 Prettier와 ESLint를 설정`하는 법을 알아볼텐데요. 각각 `npm으로 설치 가능하며 터미널 커맨드로 제어 가능한 라이브러리`이기 때문에 굳이 IDE에 별도 세팅하지 않아도 수동으로 실행시키고 코드 검사와 포멧팅 맞춤을 할 수 있습니다. 하지만 사용하는 IDE에서 코드를 짜다가 저장 시에 바로 포멧팅을 맞춰주고 코드 품질 검사까지 해준다면 더욱 생산성이 올라가겠죠?

에어비앤비와 같은 기업들이 세팅한 eslint 파일이 플러그인으로 제공되고 있기도 하지만, 여기서는 처음부터 설정하는 것을 가정하여 작성합니다. 설정 순서는 Prettier, ESLint, 그리고 각각 수동 실행 방법으로 나열해 보겠습니다.

---

## Prettier

> "코드 포멧팅만을 지원하며, 간편한 설정과 가장 널리 쓰이는 포멧팅 라이브러리로써 구글에서 정말 많은 레퍼런스를 찾을 수 있습니다."

1. 프로젝트 root 경로에 `.prettierrc.js`(확장자는 js / json / yaml / toml 모두 지원합니다) 파일을 생성합니다.
1. .prettierrc.js 파일 내 포멧팅 설정을 작성합니다. (참고: https://prettier.io/docs/en/configuration.html)
1. 프로젝트 root 경로에 .prettierignore 파일을 생성합니다.
1. .prettierignore 파일 내 포멧팅 적용에 제외시킬 파일 또는 경로를 작성합니다. (참고: https://prettier.io/docs/en/ignore.html)
1. 터미널에서 프로젝트 경로로 이동 뒤, 다음의 명령어로 prettier를 설치합니다 :
   ```
   npm i -D prettier
   또는
   yarn add -D prettier
   ```
   `* 로컬 OS에 global로 설치하여 사용도 가능합니다`
1. VSCode의 좌측 `Extenstions` 텝에서 "prettier"라는 키워드로 검색하여 아마도 최상단에 뜰 `"Prettier - Code formatter"` 플러그인을 설치합니다.
1. VSCode의 좌측 맨 하단의 톱니바퀴 모양을 클릭 후 [Settings] 메뉴를 클릭합니다.
1. 표시되는 창의 맨 상단의 검색창에 `settings.json`을 검색하여 나오는 결과 목록의 맨 하단으로 스크롤합니다.
1. [Edit in settings.json] 문구를 클릭하여 VSCode의 설정파일인 `settings.json`을 실행시킵니다.
1. 해당 파일에 다음의 구문을 추가합니다:
   ```
   {
     ...,
     "editor.defaultFormatter": "esbenp.prettier-vscode",
     "[javascript]": {
       "editor.defaultFormatter": "esbenp.prettier-vscode"
     },
     "[javascriptreact]": {
       "editor.defaultFormatter": "esbenp.prettier-vscode"
     },
     "[typescript]": {
       "editor.defaultFormatter": "esbenp.prettier-vscode"
     },
     "[typescriptreact]": {
       "editor.defaultFormatter": "esbenp.prettier-vscode"
     },
     "[css]": {
       "editor.defaultFormatter": "esbenp.prettier-vscode"
     },
     "[scss]": {
       "editor.defaultFormatter": "esbenp.prettier-vscode"
     },
     "editor.formatOnSave": true
     ,...
   }
   ```
1. 조금 전 .prettierrc.js 파일 내 설정한 포멧팅 룰을 해당 파일에 추가합니다. 예시:
   ```
   {
     ...,
     "prettier.bracketSpacing": true,
     "prettier.singleQuote": true,
     "prettier.trailingComma": "all"
     ,...
   }
   ```

---

## ESLint

> "코드 포멧팅과 품질 체크 라이브러리. Javascript(+Typescript) 언어만 지원하지만 해당 언어 사용자들이 가장 많이 사용하는 만큼 레퍼런스가 아주 많으며, 에어비앤비, 마이크로소프트와 같은 유명 기업들이 제공하는 코드 컨벤션이 많이 있습니다."

1. 터미널에서 프로젝트 경로로 이동 후, 다음의 명령어로 필요한 라이브러리를 설치합니다:
   ```
   npm i -D eslint
   npm i -D @typescript-eslint/eslint-plugin
   npm i -D @typescript-eslint/parser
   npm i -D eslint-config-prettier
   npm i -D eslint-plugin-prettier
   또는
   yarn add -D eslint
   yarn add -D @typescript-eslint/eslint-plugin
   yarn add -D @typescript-eslint/parser
   yarn add -D eslint-config-prettier
   yarn add -D eslint-plugin-prettier
   ```
   `* 로컬 OS에 global로 설치하여 사용도 가능합니다`
1. 프로젝트 root 경로에 .eslintrc.json 파일을 생성합니다.
1. 해당 파일 내 eslint 설정을 작성합니다. (참고: https://eslint.org/docs/user-guide/configuring)
1. 프로젝트 root 경로에 .eslintignore 파일을 작성합니다.
1. 해당 파일 내 eslint 룰 적용을 제외시킬 파일명 또는 경로를 작성합니다.
1. VSCode Extention 탭에서 "eslint"로 검색하여 나타나는 목록의 최상단 플러그인, `ESLint`를 설치합니다.
1. 위에서와 같이, VSCode의 settings.json 파일을 열어, 다음의 구문들을 추가합니다:
   ```
   {
     ...,
     "editor.formatOnSave": true,
     "editor.codeActionsOnSave": {
       "source.fixAll.eslint": true
     },
     "eslint.run": "onSave"
     ,...
   }
   ```

---

## Prettier / ESLint 수동 실행

#### Prettier

1. 터미널에서, Prettier를 적용시키고픈 소스들이 위치한 폴더로 이동한 다음, 다음의 명령어로 코드 포멧팅을 적용합니다:
   ```
   npx prettier --write [경로. 예시: **/*.js]
   ```
1. node 프로젝트라면, package.json의 스크립트 명령어에 추가하여 사용할 수도 있습니다: 예시:
   ```
   "scripts": {
     ...,
     "prettier": "npx prettier --write **/*.ts, **/*.css"
     ,...
   }
   ```

#### ESLint

1. 터미널에서 ESLint 체크를 할 소스들이 위치한 폴더로 이동한 다음, 다음의 명령어를 실행합니다:
   ```
   eslint "[경로. 예시: **/*.{js,ts}]"
   ```
1. 터미널에서 ESLint 자동 수정을 적용할 소스들이 위치한 폴더로 이동한 다음, 다음의 명령어를 실행합니다:
   ```
   eslint "[경로. 예시: **/*.{js,ts}]" --fix
   ```
1. node 프로젝트라면, package.json의 스크립트 명령어에 추가하여 사용할 수도 있습니다: 예시:
   ```
   "scripts": {
     ...
     "lint": "eslint \"src/**/*.{js,ts,tsx,css,scss}\"",
     "lint:fix": "eslint \"src/**/*.{js,ts,tsx,css,scss}\" --fix"
     ,...
   }
   ```
