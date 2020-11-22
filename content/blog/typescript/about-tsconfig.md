---
title: 'tsconfig.json 알아보기'
date: 2020-11-12 14:22:13
category: 'typescript'
draft: true
---

tsconfig.json의 옵션에 대해 알아보자.

먼저 CRA로 만든 TypeScript의 가장 기본 `tsconfig.json` 뼈대를 보자.

```json
{
  "compilerOptions": {
    "target": "es5", // ECMAScript 버전 설정
    "lib": ["dom", "dom.iterable", "esnext"],
    "allowJs": true, // 자바스크립트 파일 컴파일 허용 여부
    "skipLibCheck": true, // 정의 파일의 타입 확인을 건너 뛸 지 여부
    "esModuleInterop": true, // 모든 imports에 대한 namespace 생성을 통해 CommonJS와 ES Modules 간의 상호 운용성이 생기게할 지 여부, 'allowSyntheticDefaultImports'를 암시적으로 승인
    "allowSyntheticDefaultImports": true, // default export이 아닌 모듈에서도 default import가 가능하게 할 지 여부, 해당 설정은 코드 추출에 영향은 주지 않고, 타입확인에만 영향을 줍니다.
    "strict": true, // 모든 엄격한 타입-체킹 옵션 활성화 여부
    "forceConsistentCasingInFileNames": true, // 같은 파일에 대한 일관되지 않은 참조를 허용하지 않을 지 여부
    "noFallthroughCasesInSwitch": true, // switch문에서 fallthrough 케이스에 대한 에러보고 여부
    "module": "esnext", // 모듈을 위한 코드 생성 설정
    "moduleResolution": "node", // 모듈 해석 방법 설정: 'node' (Node.js) 혹은 'classic' (TypeScript pre-1.6).
    "resolveJsonModule": true,
    "isolatedModules": true, // 각 파일을 분리된 모듈로 트랜스파일 ('ts.transpileModule'과 비슷합니다).
    "noEmit": true, // 결과 파일 내보낼지 여부
    "jsx": "react", // JSX 코드 생성 설정: 'preserve', 'react-native', 혹은 'react'.
    "strictNullChecks": true // 엄격한 null 확인 여부
  },
  "include": ["src"]
}
```

<br />

**참고**

<div style="font-size: 12px;">

- https://www.staging-typescript.org/tsconfig

- https://geonlee.tistory.com/214

</div>
