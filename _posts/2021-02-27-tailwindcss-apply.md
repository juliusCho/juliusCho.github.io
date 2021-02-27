---
layout: post
title: 'VSCode에서 Tailwind CSS의 @apply 디렉티브 문제 해결하기'
categories: [IDE, BugFix, CSS]
featured: true
beforetoc: '** 해당 포스팅은 제가 찾은 정보들을 바탕으로 작성된 것이며, 정보가 빈약하거나 오류가 있을 수 있습니다. 댓글로 지적 & 수정요청 해주시면 너무나 감사하겠습니다!'
---

최근 가장 핫한 CSS 라이브러리로 Tailwind CSS가 떠올랐죠. 이젠 꽤나 역사가 깊은(?) 부트스트랩과 비슷한 `선언적 클래스` 형태로써 반응형 웹페이지에 최적화된 클래스셋을 제공하는데요. 현재까지도 부트스트랩이 가장 많은 사용량을 보이고는 있지만 점차 Tailwind CSS를 지지하는 사람들이 늘어가는 이유는 아마도 부트스트랩처럼 `기본 테마가 탑재된` UI 키트를 제공하는 방식이 아닌, `좀 더 작은 CSS 인라인 스타일들의 묶음`을 CSS 표준에 근거한 직관적인 클래스 명으로 제공하기 때문이 아닐까 합니다. 다시 말해, 부트스트랩이 제공하는 여러 유용한 클래스들을 그대로 사용하려면 사용자가 원하는 세부적인 스타일링을 위해 인라인 스타일 추가 적용이나 각 클래스들의 내부 CSS값을 바꿔주어야 하지만, 애초에 사용자에게 더 세세한 클래스셋만 제공하여 `덜 opinionated 한` 라이브러리로써 역할하며 비교적 더욱 직관적인 클래스명들 및 기본 설정 변경방법, 그리고 추가적인 스타일 묶음을 만들 수 있는 기능들을 제공하기 때문이라 생각합니다.

앞서 말한 `스타일 묶음`을 사용자가 별도의 CSS 클래스로 만들어 재 사용하게 해주는 명령어가 바로 `[@apply]` 인데요. 사용 예시는 다음과 같습니다 :

```
.my-button-class {
  @apply bg-white text-black rounded-full py-2 px-3 uppercase font-bold cursor-pointer hover:opacity-50
}
```

위에서 볼 수 있듯, @apply 디렉티브는 기본 CSS 디렉티브처럼 간단히 사용할 수 있는데요. VSCode에서 해당 명령어 사용 시에 오류가 나며 빌드가 실패하는 오류를 발견해서 구글링으로 찾은 방법으로, `stylelint`라는 툴을 설치하는 방법을 공유해 볼까 합니다.

--

## stylelint

> "IDE가 Tailwind CSS, SCSS 등, 가장 최신 CSS 문법을 이해할 수 있게 하는 linter도구 입니다."

1. 다음의 명령어로 stylelint를 기본 설정값으로 설치합니다 :

```
npm i --save-dev stylelint stylelint-config-standard
또는
yarn add -D stylelint stylelint-config-standard
```

1. 프로젝트 루트 경로에 `.stylelint.json` 파일을 만들어 다음의 내용을 넣습니다 :

```
{
  "extends": "stylelint-config-standard"
}
```

---

## VSCode 설정 추가

> "에디터에서 계속해서 빨간줄이 생기는 것을 없애는 방법입니다."

1. VSCode의 `settings.json` 파일을 열어 다음의 구문을 추가합니다 :

```
...
"css.validate": false,
"less.validate": false,
"scss.validate": false,
...
```
