1. TypeScript + Next.js
    ```shell
    $ npx create-next-app@latest opgg --typescript    
    ```
1. Emotion
    ```shell
    $ yarn add @emotion/react @emotion/styled @emotion/css @emotion/server
    ```
1. Emotion css props관련 tsconfig.json 수정
    ```json
    {
        "compilerOptions": {
            // ...
            "jsx": "preserve",
            "jsxImportSource": "@emotion/react",
            // ...
        }
        // ...
    }
    ```
1. Tailwind CSS
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
                'next/babel',
                {
                    'preset-react': {
                        runtime: 'automatic',
                        importSource: '@emotion/react',
                    },
                },
            ],
        ],
        plugins: ['@emotion/babel-plugin', 'babel-plugin-macros'],
    }
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