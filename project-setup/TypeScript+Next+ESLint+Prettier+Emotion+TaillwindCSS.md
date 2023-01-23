1. TypeScript + Next.js 설치
   ```shell
   $ npx create-next-app@latest next-app --typescript
   ```
1. Prettier 설치
   ```shell
   $ yarn add -D prettier eslint-plugin-prettier eslint-config-prettier
   ```
1. Prettier관련 .eslintrc.json 수정
   ```json
   {
     "env": {
       "browser": true,
       "es2021": true,
       "node": true
     },
     "extends": [
       "eslint:recommended",
       "next",
       "next/core-web-vitals",
       "plugin:prettier/recommended"
     ]
   }
   ```
1. Prettier관련 .prettier.json 생성
   ```json
   {
     "semi": true,
     "trailingComma": "es5",
     "singleQuote": true,
     "tabWidth": 2,
     "useTabs": false
   }
   ```
1. VSCode에서 Prettier Plugin 설치 후, setting.json 수정
   ```json
   {
     // ...
     "editor.fontWeight": "normal",
     "editor.codeActionsOnSave": {
       "source.fixAll.eslint": true
     },
     "editor.formatOnSave": true,
     "editor.defaultFormatter": "esbenp.prettier-vscode"
   }
   ```
1. Emotion 설치
   ```shell
   $ yarn add @emotion/react @emotion/styled @emotion/css @emotion/server
   ```
1. Emotion css props관련 tsconfig.json 수정
   ```json
   {
     "compilerOptions": {
       // ...
       "jsx": "preserve",
       "jsxImportSource": "@emotion/react"
       // ...
     }
     // ...
   }
   ```
1. Tailwind CSS 설치
   ```shell
   $ yarn add -D twin.macro tailwindcss @emotion/babel-plugin babel-plugin-macros
   ```
1. tailwind.config.js 생성
   ```shell
   $ npx tailwindcss-cli@latest init --full
   ```
1. .babelrc.js 생성
   ```javascript
   module.exports = {
     presets: [
       [
         "next/babel",
         {
           "preset-react": {
             runtime: "automatic",
             importSource: "@emotion/react",
           },
         },
       ],
     ],
     plugins: ["@emotion/babel-plugin", "babel-plugin-macros"],
   };
   ```
1. src 디렉토리 생성 및 각종 디렉토리 이동
   ```shell
   $ mkdir src && mv ./pages ./src/pages && mv ./styles ./src/styles
   ```
1. 절대경로관련 tsconfig.json 수정
   ```json
   {
     "compilerOptions": {
       // ...
       "incremental": true,
       "baseUrl": ".",
       "paths": {
         "~assets/*": ["./src/assets/*"],
         "~components/*": ["./src/components/*"],
         "~constants/*": ["./src/constants/*"],
         "~contexts/*": ["./src/contexts/*"],
         "~helpers/*": ["./src/helpers/*"],
         "~hooks/*": ["./src/hooks/*"],
         "~i18n/*": ["./src/i18n/*"],
         "~layouts/*": ["./src/layouts/*"],
         "~pages/*": ["./src/pages/*"],
         "~styles/*": ["./src/styles/*"],
         "~services/*": ["./src/services/*"],
         "~types/*": ["./src/types/*"],
         "~utils/*": ["./src/utils/*"],
         "~public/*": ["./public/*"],
         "~src/*": ["./src/*"],
         "~/*": ["./*"]
       }
     }
     // ...
   }
   ```
