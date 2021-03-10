---
layout: post
title: 'NextJS에 Jest 세팅하기'
categories: [Testing, Frontend]
beforetoc: '** 해당 포스팅은 제가 찾은 정보들을 바탕으로 작성된 것이며, 정보가 빈약하거나 오류가 있을 수 있습니다. 댓글로 지적 & 수정요청 해주시면 너무나 감사하겠습니다!'
featured: true
---

SPA에서도 SEO 최적화 및 사용자 경험을 위한 `Server Side Rendering`이 중요함에 따라, 리액트 생태계에서도 점차 CRA보다 NextJS를 선호하는 개발자가 많아지게 되었습니다. NextJS가 제공하는 기본 프레임워크 구조 Rule과 편리한 라우팅 시스템과 같은 다양한 기능들은, Server Side Rendering 뿐이 아니어도 충분히 매력적인 개발 환경을 제공해 준다 생각합니다. 하지만 프레임워크 별로 테스트 도구의 설정은 또 달라져야겠죠. 여기서는 Jest 라이브러리의 세팅 법을 알아보겠습니다.

---

## 필요 Dependencies 설치

> "각 디펜던시 라이브러리는 npm 또는 yarn을 이용하여 설치할 수 있습니다. 모두 테스트 용 도구이기 때문에 devDependencies로 설치하여도 무방합니다."

1. jest
1. jest-dom
1. @testing-library/react (리액트 컴포넌트의 랜더링에 필요)
1. @testing-library/jest-dom (렌더링 된 컴포넌트의 검사 용)
1. @testing-library/user-event (사용자의 동작을 모방하여 dom 제어 가능)
1. @testing-library/dom (VDOM이 아닌, 실제 DOM을 검사하는 용도)
1. babel-jest (jest 구문의 javascript 컴파일 용)
1. identity-obj-proxy (테스트 시 ES6 문법의 import/export 등의 문법 해석 용)
1. @types/jest (타입스크립트 사용 시에만 필요)

## 프로젝트 Root 경로에 필요 설정 파일들 생성

1. setupTests.js
   - @testing-library/jest-dom 패키지 내 리엑트 컴포넌트 테스트에 필요한 추가 객체들을 로딩합니다
   - 내용 :
     ```
     import '@testing-library/jest-dom/extend-expect'
     ```
1. .babelrc
   - NextJS 프로젝트에 babel을 적용시키도록 설정합니다
   - 내용 :
     ```
     {
       "presets": ["next/babel"]
     }
     ```
1. jest.config.js
   - jest 사용에 대한 기본적인 설명을 합니다.
     1. 테스트 검사를 실행시키지 않을 경로를 지정합니다. NextJS 앱이 컴파일 된 결과물이 위치할 .next 폴더와 디펜던시 페키지들이 위치할 node_modules 내 모든 경로들을 지정했습니다.
     1. 앞서 생성한 setupTests.js (해당 파일명은 다르게 만들어도 무방) 가, 테스트 실행 전에 구동되도록 설정합니다.
     1. 프로젝트 내 모든 자바스크립트/타입스크립트 파일들을 babel-jest 파일로 읽을 수 있도록 하고, 모든 css 파일들을 identity-obj-proxy를 사용하여 읽도록 합니다.
   - 내용 :
     ```
     module.exports = {
       testPathIgnorePatterns: ['<rootDir>/.next/', '<rootDir>/node_modules/'],
       setupFilesAfterEnv: ['<rootDir>/setupTests.js'],
       transform: {
         '^.+\\.(js|jsx|ts|tsx)$': '<rootDir>/node_modules/babel-jest',
         '\\.(css|less|scss|sass)$': '<rootDir>/node_modules/identity-obj-proxy',
       },
     }
     ```

---

## 테스트 파일 작성 후 테스트

> "테스트 파일들은 어떤 경로에 위치하든 상관없으나(위 설정에서 테스트 제외 경로로 설정한 경로 제외), pages폴더 내에는 위치할 수 없습니다. NextJS는 pages폴더 내 파일들을 모두 자동 라우팅 대상으로 인식하기 때문입니다."

1. 테스트 파일 작성(예시)

   ```
   import * as TLReact from '@testing-library/react'
   import Home from '../../pages'

   describe('Home', () => {
     it('renders Home without error', () => {
       const { container } = TLReact.render(<Home />)

       expect(container).toHaveTextContent('Hello W')
     })
   })
   ```

1. package.json에 테스트 스크립트 추가 후 테스트 실행

   ```
   ...,
   "scripts": {
     ...,
     "test": "jest"
     ...
   },
   ...
   ```

1. 테스트 실패 확인 후, 해당 컴포넌트를 테스트 성공하도록 수정 후 테스트 재실행
