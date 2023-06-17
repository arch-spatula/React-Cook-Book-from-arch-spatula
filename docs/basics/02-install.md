---
sidebar_position: 2
---

# 설치

리액트는 역시 자바스크립트 답게 설치 전략이 상당히 다양합니다.

## vite

vite으로 설치할 이유는 CRA를 더이상 권장하지 않기 때문입니다.

```sh
npm create vite@latest
```

마법사의 제안에 따라 읽고 설치하면 됩니다.

```sh
yarn create vite . --template react-swc-ts
```

하지만 저는 위 명령 스타일을 선호합니다.

## Create React App a.k.a. CRA

CRA의 장점은 Test, Web Vitals가 같이 설정되어 있습니다. vite 보다 분명히 더 무거울 것입니다. 하지만 테스트관련 설정을 덜 할 수 있는 점이 장점입니다.

```sh
npx create-react-app .
```

위 명령으로 간단하게 만들 수 있습니다.

[Create React App is Finally Dead](https://www.youtube.com/watch?v=M4CLvtCS2YU)

```sh
yarn create react-app . --template typescript
```

<!--

TODO: CDN 설치법 추가하기
---

## CDN

CDN을 활용해서 리액트를 활용하는 것도 가능합니다. 하지만 치명적인 단점은 자동완성을 지원하지 않습니다.

TODO: Next.js 설치법
TODO: Remix.js 설치법
TODO: Nx.js 설치법
TODO: Astro.js 설치법
TODO: Gatsby.js 설치법
TODO: DIY 개별 설치법

[7 better ways to create a React app](https://www.youtube.com/watch?v=2OTq15A5s0Y)

-->
