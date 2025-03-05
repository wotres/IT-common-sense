# part1
## SPA(Single Page Application)
* 웹 애플리케이션이 처음 로드될 때 필요한 모든 리소스를 한 번에 불러오고, 이후에는 새로운 페이지를 요청하는 대신 JavaScript를 이용해 동적으로 콘텐츠를 갱신하는 방식의 애플리케이션
* 특징
  * 새로운 페이지를 로드하지 않고 JavaScript를 사용하여 필요한 데이터만 변경
  * 초기 요청에서 HTML을 거의 빈 상태로 보내고, JavaScript가 데이터를 불러와 화면을 구성
  * 필요한 데이터만 가져오기 때문에 속도가 빠름
* 최적화 해야함
  * SPA는 동적이고 사용자 경험이 좋은 웹앱을 만들 수 있지만, 최적화가 부족하면 성능 저하 발생
  * 최적화 대상
    * 초기 로딩 속도
      * 처음 로드할 때 모든 리소스를 다운로드하므로, 번들 크기가 커지면 로딩 시간이 길어짐
      * 해결법
        * 코드 스플리팅(Code Splitting)
          * 필요한 코드만 로드하도록 분할
          * Entry Point 분할 (ex. Webpack에서 entry 설정)
          * 여러 개의 진입점을 만들어 특정 페이지마다 필요한 코드만 로드
          * Router에서 특정 페이지가 로드될 때만 필요한 컴포넌트를 불러오도록 설정
        * 레이지 로딩(Lazy Loading)
          * 필요한 순간까지 리소스를 로드하지 않고, 사용자가 요청할 때만 불러오는 기법
          * 사용자가 스크롤을 내릴 때, 보이는 이미지만 로드
        * 트리 쉐이킹(Tree Shaking)
          * 사용하지 않는(dead code) JavaScript 코드를 제거하여 번들 크기를 줄이는 최적화 기법
    * SEO(Search Engine Optimization)
      * SPA는 클라이언트 사이드에서 렌더링되므로, 크롤러가 내용을 제대로 인식하지 못할 수 있음
      * 해결법
        * 서버 사이드 렌더링(SSR)
          * 요청시 서버에서 HTML을 미리 렌더링한 후 클라이언트에 전달
        * 프리렌더링(Pre-rendering)
          * HTML을 빌드 타임에 미리 생성하여 제공하는 방식
          * JavaScript를 실행하기 전에 정적인 HTML 파일을 만들어서 빠른 로딩 속도와 SEO 최적화
        * 동적 렌더링(Dynamic Rendering)
          * 검색 엔진 크롤러와 일반 사용자에게 각기 다른 방식으로 페이지를 제공하는 기법
          * 검색 엔진 크롤러(봇)에는 SSR(서버 사이드 렌더링)된 HTML을 제공
          * 일반 사용자에게는 CSR(클라이언트 사이드 렌더링) 페이지 제공

## CSR (Client-Side Rendering) vs SSR (Server-Side Rendering)
* 비교
  | 구분  | **CSR (Client-Side Rendering)** | **SSR (Server-Side Rendering)** |
  |--------|--------------------------------|--------------------------------|
  | **렌더링 위치** | 브라우저 (클라이언트) | 서버 |
  | **초기 로딩 속도** | 느림 (JS 다운로드 & 실행 필요) | 빠름 (완성된 HTML 제공) |
  | **페이지 전환 속도** | 빠름 (클라이언트에서 처리) | 느림 (서버 요청 필요) |
  | **SEO(검색 엔진 최적화)** | 불리함 (JavaScript 실행 필요) | 유리함 (완전한 HTML 제공) |
  | **서버 부하** | 낮음 (초기 요청 후 서버 부담 적음) | 높음 (매 요청마다 HTML 생성) |
  | **UX (사용자 경험)** | 앱 같은 부드러운 전환 가능 | 일부 페이지 전환 시 깜빡임 발생 가능 |
  | **예제 프레임워크** | React (CRA), Vue, Angular | Next.js, Nuxt.js |
* CSR이 적합한 경우
  * 사용자 인터랙션이 많고, 페이지 전환이 자주 발생하는 경우 (예: 대시보드, 메신저)
  * SEO가 중요하지 않은 웹앱
* SSR이 적합한 경우
  * SEO가 중요한 사이트 (예: 블로그, 뉴스, 전자상거래)
  * 초기 로딩 속도가 중요한 경우 (초기 HTML이 즉시 표시되므로 첫 화면이 빠르게 렌더링됨)
  * SNS 공유 시 미리보기(OG 태그)가 필요한 경우
    * CSR(클라이언트 사이드 렌더링)은 초기 HTML이 거의 비어 있고, JavaScript가 실행된 후 콘텐츠가 로드됨
      하지만 SNS 크롤러는 JavaScript 실행 없이 HTML만 가져가서 OG 태그를 인식하지 못하는 경우가 있음
      * 공유 시 이미지 없음, 제목 없음, 설명 없음
    * ex. <meta property="og:title" content="OO 블로그">
  
## Yarn Berry (with PnP)
* Yarn 2.x 이상의 버전 (기존 Yarn 1과 비교해 성능과 보안이 크게 개선된 패키지 매니저)
* PnP(Plug’n’Play)
  * node_modules 없이 패키지를 관리하는 새로운 방식
* 특징
  * node_modules/ 없음! (.pnp.cjs 파일로 패키지를 관리)
  * 패키지 설치 속도가 기존 node_modules 방식보다 훨씬 빠름
  * 모듈 탐색이 매우 빠름! (fs 시스템 탐색을 하지 않음) => 탐색 비용 최적화
    * 기존 npm 방식: fs 시스템을 재귀적으로 탐색 => 느림
    * Yarn PnP 방식: .pnp.cjs에서 패키지 경로를 즉시 찾음 => 빠름
  * Zero-Installs (제로 인스톨) 지원
    * 패키지가 .yarn/cache/에 압축된 상태로 저장
    * .yarn/cache/에 패키지를 저장하므로, yarn install을 다시 할 필요 없음
* IDE에서 자동 완성 안되는 문제가 발생할 수도 있음
  * VSCode에서는 Yarn PnP SDK 플러그인 사용 (yarn dlx @yarnpkg/sdks vscode 실행)
* 설치
  * yarn 설치 및 yarn berry 적용
    * $ npm install -g yarn
    * $ yarn set version berry    
  * PnP 모드 활성화 (node module 대신 사용 명시)
    * $ yarn config set nodeLinker pnp
  * 패키지 설치 (.pnp.cjs 파일 생성됨)
    * $ yarn install
  * .zip 내부의 파일을 직접 접근을 위해 ZipFS Plugin 설치
  * Yarn PnP(Plug’n’Play) 환경에서 VSCode가 패키지를 정상적으로 인식할 수 있도록 설정
    * $ yarn dlx @yarnpkg/sdks vscode
  * .yarn/cache 폴더 안생기는경우 enableGlobalCache false 
    * 
    ```bash
    enableGlobalCache: false
    cacheFolder: .yarn/cache
    ```

## vim undo
* i 입력 모드
* u 되돌리기

## nvm 사용
### nvm 설치
* https://github.com/nvm-sh/nvm
* curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.40.1/install.sh | bash
* 설정
```bash
export NVM_DIR="$HOME/.nvm"
[ -s "$NVM_DIR/nvm.sh" ] && \. "$NVM_DIR/nvm.sh"  # This loads nvm
[ -s "$NVM_DIR/bash_completion" ] && \. "$NVM_DIR/bash_completion"  # This loads nvm bash_completion
```
* shell restart
  * $ source ~/.zshrc
### node 설치 및 설정
* 설치 할수 있는 node version 확인
  * $ nvm ls-remote
* 원하는 버전 설치
  * $ nvm install v22.14.0
* 확인
  * $ nvm list
  * $ node -v
* 원하는 버전 선택
  * $ nvm list
  * $ nvm use v22.14.0
* 글로벌 설정
  * $ nvm use -g v22.14.0

## REST 
* REST는 웹에서 HTTP 기반으로 데이터를 깔끔하게 주고받는 규칙
* 원칙
  * 클라이언트(브라우저, 모바일 앱 등)-서버(API 제공자) 구조 => 분리
  * 무상태(Stateless) 
    * => 요청(Request) 간에 서버가 클라이언트의 상태를 기억하지 않아 매 요청마다 필요한 정보를 포함해야 함
  * 캐시 가능(Cacheable)
    * => 클라이언트는 응답을 캐싱하여 서버 부하를 줄일 수 있음
  * 일관된 인터페이스(Uniform Interface)
    * => API는 표준적인 HTTP 메서드와 URI를 사용
  * 계층화(Layered System)
    * => 클라이언트는 API 서버 외에 보안, 로드 밸런서 등의 중간 계층을 알 필요 없음
* RESTful 은 REST 원칙을 따르는 API
* 실시간성이 중요한 경우에는 효율적이지 않아 WebSocket 같은 양방향 통신 프로토콜 고려 (ex. 채팅 어플)

