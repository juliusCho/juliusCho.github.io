---
layout: post
title:  "Javascript 대표 테스트 라이브러리 몇가지"
categories: [ Frontend ]
beforetoc: "** 해당 포스팅은 제가 찾은 정보들을 바탕으로 작성된 것이며, 정보가 빈약하거나 오류가 있을 수 있습니다. 댓글로 지적 & 수정요청 해주시면 너무나 감사하겠습니다!"
featured: true
---
특히 국내에서는 웹개발 시 Java와 Javascript를 기본 베이스로 개발을 하시는 분들이 많으리라 생각합니다. 저도 그 중 한명으로 Java로 백엔드 코드를 짤 때는 TDD까지는 아니더라도 종종 JUnit으로 단위 테스트를 짜보기는 했지만, 프론트 코드는 브라우저에서 바로 바로 동작을 해볼 수 있고, 크롬 검사 도구가 워낙 잘 되어 있어 테스트에 대한 필요성을 크게 못 느꼈습니다.

저처럼 `아직도` Javascript 소스에 대한 테스트를 하지 않은 분들이 국내에는 제법 되는 것을 보고 안도의 한숨을 쉬고는 있지만..ㅎㅎ 이직을 하며 거의 대부분의 개발 업무를 프론트 단에서 하게 되며(특히나 리액트를 사용하게 되며) 점차 복잡도가 늘어나는 프론트 코드에 대한 제어권을 잃어가는 자신을 발견하게 되었습니다(..)

결국 제가 짠 코드에 제가 잠식되는 기분을 한껏 맡보고 나서야(?) 다음과 같은 결론을 내릴 수 있었습니다 :
>"코드 수정 시 내가 이미 짠 걸 다시 망치는 짓을 멈추기 위해선 소스에 대한 철저한 관심사의 분리와 유효성 검사에 대한 기록이 필요하겠다"

따라서 테스트 도구 도입 전에 여러 라이브러리들을 찾아본 결과, 단위 테스트에선 가장 많이 쓰이며 높은 안정성과 많은 레퍼런스를 보유한 Jest, 리액트의 가상 DOM 엘리먼트를 캡처하는 Enzyme, 그리고 통합 테스트 용으로는 손쉬운 사용법과 다양한 플러그인으로 활용성이 높은 TestCafe 라는 툴을 도입하기로 했습니다. 각 툴/라이브러리 별 특징과 설명, 그리고 예시를 정리해 보았습니다.

## Jest
* 공식 DOCS: https://jestjs.io/
* Javascript 단위 테스트 라이브러리
* Typescript 지원
* 단위 테스트를 scope 별로 관리 가능
* Auto Reload 지원
* describe, test, expect, beforeEach.. 등등 직관적인 함수명과 손쉬운 사용
* 예시코드
```
describe('홈페이지 함수 동작 테스트 scope', () => {
  beforeAll(() => {
    console.log('홈페이지 함수 동작 테스트 시작')
  })

  test('변수 초기 값 테스트', () => {
    expect(variable).toBe('0')
  })

  test('변수 값 증가 함수 호출 테스트', () => {
    increment()
    expect(variable).toBe('1')
  })
})
```

## Enzyme
* 공식 DOCS: https://enzymejs.github.io/enzyme/
* React JSX Element를 캡처하여 테스트 용 객체로 반환해 주는 라이브러리
* Typescript 지원
* `React 17 버전 현재 미지원`
* 17버전을 위해 대체용 오픈소스, `@wojtekmaj/enzyme-adapter-react-17` 를 사용할 수 있지만, 해당 소스는 타입스크립트 미지원
* enzyme-adapter-react-16을 사용해도 되지만, 테스트시 다음과 같은 오류가 뜸 :
>"TypeError: Cannot read property \"child\" of undefined"


* 손쉬운 사용과 리액트 버전에 맞는 어댑터 라이브러리르 쓰면 높은 안정성으로, 리액트 에플리케이션의 단위 테스트로 Jest와 함께 많이 사용된다
* 예시코드

```
Enzyme.configure({ adapter: new EnzymeAdapter() })

test('myDiv 찾기', () => {
  const wrapper = shallow(<App />).find('[data-test="myDiv"]')
  expect(wrapper).toBeTruthy()
})
```

## TestCafe
* 통합 테스트 자동화를 위한 node.js 툴
* 다양한 OS와 브라우저를 지원하며, 터미널 커맨드 라인으로 동작 제어
* 브라우저의 URL 별 동작에 대한 단위, 통합 테스트 가능 (Headless mode 지원)
* Endpoint 테스트를 지원하기 때문에 Javascript 함수별 동작 테스트는 불가
* 다양한 리포팅과 문서화 플러그인을 지원하기 때문에 테스트 결과 문서화에 용이
* 예시코드

```
[JS]
fixture `테스트 페이지 테스트`
  .page `http://localhost:80/test`

test('버튼 동작 여부 테스트', async t => {
  await t
    .expect(Selector('#number').innerText).eql('0')
    .click('#incrementButton')
    .expect(Selector('#number').innerText).eql('1')
})

[Terminal]
testcafe chrome test.js
```