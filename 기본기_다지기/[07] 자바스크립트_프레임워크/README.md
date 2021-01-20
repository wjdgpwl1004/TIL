# 자바스크립트 프레임워크

## React
- 컴포넌트 기반
- Virtual DOM 사용
- Virtual DOM : Virtual DOM은 뷰에 변화가 있다면, 그 변화가 실제 DOM에 적용되기 전에 Virtual DOM에 적용시키고 최종 결과만 실제 DOM에 전달
- 단방향바인딩(데이터 → 뷰의 형태로 한쪽으로 데이터가 흐름)
- JSX 기반 컴포넌트 구문
- 큰 규모에서 더 빛을 발하고, 테스팅이 수월(Javascript 으로 만들어진 Template 은 컴포넌트로 구성하기 쉽고, 재사용성이 높으며 테스트 하기가 용이)
- Web 과 Native 앱 개발에 모두 사용 가능
- 더 큰 개발자 생태계에서 오는 많은 레퍼런스와 도구들

## Vue
- 컴포넌트 기반
- Virtual DOM 사용
- HTML 기반 템플릿 구문
- 양방향바인딩(뷰 ⇄ 데이터 형태로 바인딩하여 데이터가 양 방향으로 흐르게 해주는 것. 즉, 데이터에 있는 값이 뷰에 나타나고, 이 뷰의 값이 바뀌면 데이터의 값도 바뀌는것)
- Template 과 Render Function 을 모두 사용할 수 있는 옵션
- 간편한 Syntax 와 프로젝트 설정
- 빠른 렌더링과 더 작은 용량

## React & Vue 공통점 
- Virtual DOM 으로 빠른 렌더링
- 경량 라이브러리
- Reactive Component
- Server Side Rendering
- 라우터, 번들러, state management 와 결합이 쉬움
- 훌륭한 개발자 커뮤니티와 지원

## Angular
- 완전한 프레임 워크로, 프로젝트의 생성, 테스트, 빌드, 배포를 위한 모든 기능을 제공
- TypeScript를 사용
- Angular CLI를 제공하여 개발환경을 지원. 파일 생성, 빌드, 패키징, 라이트 서버 기능 등 개발에 필요한 거의 모든 기능을 자체적으로 제공
- 모듈과 컴포넌트 기반으로 동작
- 웬만한 기능의 라이브러리는 모두 포함시켜서 자체적으로 제공(라우팅, HTTP, Form 등등)
- 기본적으로는 단일 페이지 애플리케이션 (SPA, Single Page Application) 개발을 위한 프레임워크. 다만, Server Side Rendering을 위한 기능을 구비하고 있음.


# 출처
- https://library.gabia.com/contents/infrahosting/8284/
- https://joshua1988.github.io/web_dev/vue-or-react/
- https://velog.io/@kihyeon8949/TIL.-Vue.js2-1
- https://paperblock.tistory.com/52
