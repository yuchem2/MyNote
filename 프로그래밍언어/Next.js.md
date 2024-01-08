웹 애플리케이션을 만들 수 있는 빌딩 블록을 제공하는 유여한 [[React]] Framework

[[React]]에 필요한 도구 및 구성을 처리하고 애플리케이션에 대한 추가 구조, 기능 및 최적화를 제공

SSR(Server-Side-Rendering) 방식을 수행
## Installation
+ [[Setting]]
## Features
### SSR
+ User가 브라우저를 통해 앱에 접속하면 서버에서 리액트를 실행
+ 랜더링된 결과를 통해 브라우저에게 html 문서 제공
+ 브라우저는 [[React]] 코드가 있는 js 파일을 다운받고 실행. CSR 동작을 하게 되고 이 과정을 hydration이라고 함

즉 서버에서 UI를 모두 구성한 후 유저에게 응답해 화면을 보여주는 방식. 화면이 pre-rendering되어 인터넷 속도와 상관없이 화면에 UI가 로딩된다.

※ hydration: [[React]]가 브라우저에 이미 존재하는 html과 동기화하여 앱이 동적으로 상호작용할 수 있도록 함

#### 장점
+ SEO에 좋음(모든 데이터가 html 파일에 포함)
+ 초기 로딩 속도가 빠름
+ 정적 웹에 좋음
+ 기본적으로 web pack과 babel을 사용하고 있어 SSR 및 개발에 필요한 설정이 이미 되어있어 개발을 빠르게 시작 가능
#### 단점
+ 컴퍼넌트 로딩이 오래 걸리면 유저가 빈 화면을 볼 수 있음
+ 서버에 매번 요청해 서버 부하가 크다
+ 페이지를 요청할 때마다 새로고침되어 UX가 다소 떨어짐

모바일 최적화를 위해 CSR과 SPA(Sinle Page Application)가 등장

### Project Structure
#### Top-level folders
+ `app`: App Router
+ `pages`: Pages Router
+ `public`: Static assets to be served
+ `src`: Optional applcation source folder
#### Top-level files
##### Next.js
+ `next.config.js`: Configuration file for Next.js
+ `package.json`: Poject dependencies and scripts
+ `instrumentation.ts`: OpenTelemetry and Instrumentation file
+ `middleware.ts`: Next.js request middleware
+ `.env`: Environment variables
+ `.env.local`: Local enviroment variables
+ `.env.production`: Producation environment variables
+ `.env.development`: Development environment variables
+ `.eslintrc.json`: Development enviroment variables
+ `.gitignore`: Git files and folders to ignore
+ `next-env.d.ts`: TypeScript declaration file for Next.js
+ `tsconfig.json`: Configuration file for TypeScript
+ `jsconfig.json`: Configuration file for JavaScript
#### `app` Routing Conventions
##### Routing Fies
+ `layout`: `.js` `.jsx` `.tsx` Layout
+ `page`: `.js` `.jsx` `.tsx` Page
+ `loading`: `.js` `.jsx` `.tsx` Loading UI
+ `not-found`: `.js` `.jsx` `.tsx` Not found UI
+ `error`: `.js` `.jsx` `.tsx` Error UI
+ `global-error`: `.js` `.jsx` `.tsx` Global error UI
+ `route`: `.js` `.ts` API endpoint
+ `template`: `.js` `.jsx` `.tsx` Re-rendered layout
+ `default`: `.js` `.jsx` `.tsx` Parallel route fallback page
##### Nested Routes
+ `folder`: Route segment
+ `folder/folder`: Nested route segment
##### Dynamic Routes
+ `[folder]`: Dynamic route segment
+ `[...folder]`: Catch-all route segment
+ `[[...folder]]`: Optional catch-all route segment
##### Route Groups and Private Folders
+ `(folder)`: Group routes without affecting routing
+ `_folder`: Opt folder and all child segments out of routing
##### Parallel and Intercepted Routes
+ `@folder`: Named slot
+ `(.)folder`: Intercept same level
+ `(..)folder`: Intercept one level above
+ `(..)(..)folder`: Intercept two levels above
+ `(...)folder`: Intercept from root
##### Metadata File Convertions
###### App Icons
+ `favicon`: `.ico` Favicon file
+ `icon`: `.ico` `.jpg` `.jpeg` `.png` `.svg` App Icon file
+ `icon`: `.js` `.ts` `.tsx` Generated App Icon
+ `apple-icon`: `.jpg` `.jpeg` `.png` Apple App Icon file
+ `apple-icon`: `.js` `.ts` `.tsx` Generated Apple App Icon
###### Open Graph and Twitter Images
+ `opengraph-image`: `.jpg` `.jpeg` `.png` `.gif` Open Graph image file
+ `opengraph-image`: `.js` `.ts` `.tsx`  Generated Graph image
+ `twitter-image`: `.jpg` `.jpeg` `.png` `.gif` Twitter image file
+ `twitter-image`: `.js` `.ts` `.tsx`  Generated Twitter image
###### SEO
+ `sitemap`: `.xml` Sitemap file
+ `sitemap`: `.js` `.ts` Generated Sitemap
+ `robots`: `.txt` Robots file
+ `robots`: `.js` `.ts` Generated Rebots file
#### `pages` Routing Conventions
##### Special Files
+ `_app`: `.js` `.jsx` `.tsx` Custom App
+ `_document`: `.js` `.jsx` `.tsx` Custom Document
+ `_error`: `.js` `.jsx` `.tsx` Custom Error Page
+ `404`: `.js` `.jsx` `.tsx` 404 Error Page
+ `500`: `.js` `.sjx` `.tsx` 500 Error Page
##### Routes
###### Folder convention
+ `index`: `.js` `.jsx` `.tsx` Home page
+ `folder/index`: `.js` `.jsx` `.tsx` Nested page
###### File convention
+ `index`: `.js` `.jsx` `.tsx` Home page
+ `file`: `.js` `.jsx` `.tsx` Nested page
##### Dynamic Routes
###### Folder convention
+ `[folder]/index`: `.js` `.jsx` `.tsx` Dynamic route segment
+ `[...folder]/index`: `.js` `.jsx` `.tsx` Catch-all route segment
+ `[[...folder]]/index`: `.js` `.jsx` `.tsx` Optional catch-all route segment
###### File convention
+ `[file]`: `.js` `.jsx` `.tsx` Dynamic route segment
+ `[...file]`: `.js` `.jsx` `.tsx` Catch-all route segment
+ `[[...file]]`: `.js` `.jsx` `.tsx` Optional catch-all route segment