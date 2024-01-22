# create-next-app 없이 구축

1. package.json을 만들기 위해 npm init 실행
2. Next.js 프로젝트를 실행하는데 필요한 핵심 라이브러리인 React, React-dom, next 설치 (npm i react react-dom next3)
3. devDependencies에 필요한 패키지 설치 (npm i @types/node @types/react @types/react-dom eslint eslint-config-next typescript --save-dev)
4. tsconfig.json 작성

```
// tsconfig.json
{
  "$schema": "https://json.schemastore.org/tsconfig", // $schema는 schemaStore에서 제공해 주는 정보로, 해당 JSON 파일이 무엇을 의미하는지, 또 어떤 키와 어떤 값이 들어갈 수 있는지 알려주는 도구
  "compilerOptions": {
    "target": "ESNext",                               // typescript가 변환을 목표로 하는 언어의 버전을 의미
    "lib": ["dom", "dom.iterable", "esnext"],
    "allowJs": true,
    "skipLibCheck": true,                             // 라이브러리에서 제공하는 d.ts에 대한 검사 여부를 결정
    "strict": true,                                   // 엄격모드 제어
    "forceConsistentCasingInFileNames": true,         // 파일 이름의 대소문자 구분
    "noEmit": true,                                   // 컴파일을 하지 않고 타입만 체크
    "esModuleInterop": true,                          // CommonJS 방식으로 보낸 모듈을 ES 모듈 방식의 import로 가져올 수 있게 해준다.
    "module": "esnext",                               // 모듈 시스템을 설정한다. 대표적으로, commonjs와 esnext가 있다.
    "moduleResolution": "node",                       // 모듈을 해석하는 방식 설정한다. node는 node_modules를 기준으로 모듈을 해석하고 classic은 tsconfig.json이 있는 디렉터리를 기준으로 모듈을 해석한다.
    "resolveJsonModule": true,                        // JSON 파일을 import할 수 있게 해준다.
    "isolatedModules": true,                          // 타입스크립트 컴파일러는 파일에 import나 export가 없다면 단순 스크립트 파일로 인식해 이러한 파일이 생성되지 않도록 막는다.
    "jsx": "preserve",                                // .tsx 파일 내부에 있는 JSX를 어떻게 컴파일할지 설정한다. (react, react-jsx, preserve 등이 있다.)
    "incremental": true,
    "baseUrl": "src",                                 // 모듈을 찾을때 기준이 되는 디렉터리를 지정한다.
    "paths": {                                        // 별칭을 지정하여 복잡한 상대경로 구조의 파일의 경롤를 쉽게 읽기 좋다.
      "@/*": ["./src/*"]
    },
  },
  "exclude": ["node_modules"],                        // 타입스크립트 컴파일 대상에서 제외시킬 파일 목록을 의미한다.
  "include": [                                        // 타입스크립트 컴파일 대상에서 포함시킬 파일 목록을 의미한다.
    "next-env.d.ts",
    "**/*.ts",
    "**/*.tsx",
  ],
```

5. nextconfig.js 작성

```
// next.config.js
/** @type {import('next').Nextconfig} */
const nextConfig = {
  reactStrictMode: true,  // 리액트의 엄격 모드 제어
  poweredByHeader: false, // 일반적으로 보안 취약점으로 취급되는 X-Powered-By 헤더를 제거
  eslint: {
    ignoreDuringBuilds: true // 빌드 시에 ESLint를 무시한다.
  },
}

module.exports = nextConfig
```

6. package.json 스크립트 작성

```
{
  // ...생략
  "scripts": {
    "dev": "next dev",
    "start": "next start",
    "build": "next build",
    // 생략..
  }
}
```
