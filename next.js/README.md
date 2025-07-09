# Next.js

* [Rendering](#rendering)
* [Caching](#caching)
* [Image Optimization](#image-optimization)
* [Font Optimization](#font-optimization)

## Rendering
1. Server Components
    - 정의: 서버에서 렌더링되며 캐싱이 가능한 컴포넌트
    - 이점
        - Data Fetching
        - Security
        - Caching
        - Bundle Sizes
        - Initial Page Load and First Contentful Paint (FCP)
        - Search Engine Optimization and Social Network Shareability
        - Streaming
1. Client Components
    - Update later..
1. Composition Patterns
    - Update later..

[메인으로 가기](https://github.com/sekhyuni/frontend-basic-concept)</br>
[맨 위로 가기](#nextjs)
## Caching
1. Request Memoization: 특정 페이지 내에서 같은 URL과 같은 options의 fetch API를 사용하는 경우, 페이지가 렌더링되는 동안 해당 요청은 캐싱되며 페이지 렌더링이 완료되면 캐싱되어 있던 모든 요청은 삭제됨  
![Request Memoization](./assets/img/request-memoization.avif)
    - React 기능
    - GET 요청만 가능
1. Data Cache: 특정 페이지 내에서 fetch API를 사용하는 경우, cache와 next.revalidate 옵션을 통해 캐싱 동작을 설정 가능  
![Data Cache](./assets/img/data-cache.avif)
    - 속성
        ```typescript
        const response = await fetch('/api/endpoint', {
            cache: 'force-cache' | 'no-store' // 기본값: 'force-cache'
            next: {
                revalidate: false | 0 | number,
            },
        });
        ```
    - revalidate 동작
        - cache가 'force-cache'인 경우, revalidate는 0이 아닌 값으로 설정되어야 함
        - revalidate 동안은 캐싱된 데이터를 사용하며, revalidate가 지나면 서버 측에서 유효성 검사를 해야 함
        - revalidate가 지나 서버 측에서 유효성 검사를 진행하는 동안에도 여전히 캐싱된 데이터를 사용하며, 데이터가 변경된 경우 Next.js는 새로운 데이터를 캐싱함 (새로운 데이터는 그다음 요청에서 사용됨)
1. Full Route Cache
    - Update later..
1. Router Cache
    - Update later..
![Router Cache](./assets/img/router-cache.avif)

[메인으로 가기](https://github.com/sekhyuni/frontend-basic-concept)</br>
[맨 위로 가기](#nextjs)
## Image Optimization
### Next.js Image Optimizer
- Next.js의 `<Image>` 컴포넌트는 image optimizer를 통해 PNG, JPEG 등을 WebP 등으로 변환하고 리사이징하여 이미지 용량을 자동으로 최적화하며, on-demand 캐싱으로 반복 요청 시 성능을 향상시킴
### CDN + Custom Image Optimizer
- 이미지가 대량이거나 트래픽이 많은 경우 Next.js Image Optimizer보다 성능상 유리할 수 있음

[메인으로 가기](https://github.com/sekhyuni/frontend-basic-concept)</br>
[맨 위로 가기](#nextjs)
## Font Optimization
- `next/font/local`은 빌드 타임에 `<link rel="preload" href="/_next/static/media/ff840cfebfb63b0c-s.p.woff2" as="font" crossorigin="" type="font/woff2">`와 같은 preload 태그를 HTML `<head>`에 삽입하여 @font-face가 선언된 css 파일과 함께 font 파일이 병렬적으로 로드되게 함으로써 FOUT이 발생할 가능성을 낮춤 (일반적으로는 @font-face가 선언된 css 파일이 로드된 후 순차적으로 font 파일이 로드됨)
- `next/font/google`은 빌드 타임에 google font 파일을 가져와 정적 자산화한 뒤 `next/font/local`이 수행하는 작업을 동일하게 수행함

[메인으로 가기](https://github.com/sekhyuni/frontend-basic-concept)</br>
[맨 위로 가기](#nextjs)