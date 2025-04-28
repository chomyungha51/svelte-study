# 📘 스벨트 (Svelte) 공부

## 1. 스벨트 기본 개념
### 🔹스벨트란?
- 컴파일 단계에서 최적화되는 프론트엔드 프레임워크
- 빌드 타임에 Svelte 코드를 순수 JS로 컴파일
- React나 Vue와 달리 런타임 프레임워크가 존재하지 않음
- 결과적으로 빠른 렌더링 성능과 작은 번들 크기

### 🔹주요 특징
- 가상 DOM 없음 → 직접 DOM 업데이트
- .svelte 파일에서 HTML, CSS, JS를 함께 사용
- useState, setState 없이 변수만 재할당하면 반응성 발생

### 🔹REPL (Read-Eval-Print Loop)
- 스벨트 코드를 실시간 테스트할 수 있는 웹 기반 환경
- https://svelte.dev/repl


## 2. 핵심 문법

### 🔹조건문 & 반복문
```svelte
{#if visible}
  <p>보입니다</p>
{/if}

{#each items as item}
  <li>{item}</li>
{/each}
```
### 🔹이벤트 핸들링
```html
<button on:click={handleClick}>클릭</button>
```

## 3. 라이프사이클 & 반응성
### 🔹라이프사이클 함수
| 메서드        | 설명                                      |
|---------------|-------------------------------------------|
| `beforeUpdate`| 상태 변경으로 DOM 업데이트 직전 실행      |
| `afterUpdate` | DOM 업데이트 직후 실행                    |
| `tick()`      | DOM 업데이트가 완료된 후 다음 코드 실행   |
- `$effect`를 추천 : 컴포넌트가 마운팅된 이후 + 포함된 변수의 값이 바뀔 때마다

### 🔹반응성
- `재할당`에 반응

- `$state()` - 이렇게 선언한 변수의 변화 추적, 내부의 변화까지 추적, 객체나 클래스의 속성 변화는 감지하지 못함, 배열에 push등으로 변경하면 업데이트하지만 내부 요소를 직접 수정하는 식으로는 반응하지 않음
- `$state.raw()` - 내부 변화는 추적 안 하고 재할당에만 반응

👉 이 둘의 차이점 아직 이해 안 됨... .push()말고는 차이점이 없는건가?

- `$derived()` - 한 줄, `$derived.by()` - 여러 줄
- `new Date()` - `setInterval`로 업데이트해줘야 함
- `SvelteDate()` - 자동으로 초 업데이트함
- `Tween` - 시간 기반 가속 및 감속
- `Spring` - 물리적으로 튕기듯이 이동

## 4. 스타일링과 바인딩

### 🔹클래스 바인딩
```svelte
<div class:on={isActive}>활성 상태</div>
<div class:highlight={isHighlighted} class:error={hasError}>상태 표시</div>
```
### 🔹콘텐츠 바인딩
```svelte
<div contenteditable bind:innerHTML={content} bind:textContnet={content}></div>
```
## 5. 비동기와 데이터 처리
### 🔹비동기 처리 예시
```svelte
<script>
  let keyword = '';
  let moviePromise;

  function search() {
    moviePromise = fetch(`https://api.example.com/movies?query=${keyword}`)
      .then(res => res.json());
  }
</script>

<input bind:value={keyword} />
<button on:click={search}>검색</button>

{#if moviePromise}
  {#await moviePromise}
    <p>로딩 중...</p>
  {:then data}
    {#each data.results as movie}
      <li>{movie.title}</li>
    {/each}
  {:catch error}
    <p>에러: {error.message}</p>
  {/await}
{/if}
```
### 🔹이벤트 수식어
| 수식어             | 설명                     |
|--------------------|--------------------------|
| `preventDefault`   | 기본 동작 막기           |
| `stopPropagation`  | 버블링 중단              |
| `once`             | 한 번만 실행됨           |

```svelte
<a href="#" on:click|preventDefault={() => alert("차단됨")}>링크</a>
<button on:click|once={() => alert("딱 한 번!")}>한 번</button>
```
- svelte:body 에만 onmouseenter, onmouseleave 존재
## 6. 슬롯과 액션
### 🔹슬롯(Slot)
```svelte
<!-- 자식 Layout.svelte -->
<header><slot name="header">기본 헤더</slot></header>
<main><slot /></main>
<footer><slot name="footer">기본 푸터</slot></footer>

<!-- 부모 -->
<Layout>
  <h1 slot="header">커스텀 헤더</h1>
  <p>본문 내용</p>
  <p slot="footer">커스텀 푸터</p>
</Layout>
```
### 🔹슬롯 + each 활용
```svelte
<!-- List.svelte -->
<ul>
  {#each items as item}
    <li><slot item={item} /></li>
  {/each}
</ul>

<!-- App.svelte -->
<List items={['🍎', '🍌', '🍇']}>
  <p slot={item}>{item.toUpperCase()}</p>
</List>
```
### 🔹액션(Action)
- DOM 요소에 기능 추가 가능 (예: 포커스, 드래그, 외부 라이브러리 적용 등)



# SvelteKit 공부
- +layout.svelte는 부모 +layout.server.js를 상속한다.
- + server.js에서는 load(){}를 선언하여 사용한다

# 추가로 궁금한 것들
## 🔹 `<pre>` `</pre> `
- 입력한 그대로를 보여줌
## 🔹 css mask
- 요소가 표시되는 부분을 제어
## 🔹 getBoundingClientRect()
- 요소의 x, y, 위치, 너비, 높이를 알려줌
## 🔹 requiestAnimationFrame()
- 브라우저에서 애니메이션을 그릴 때, 최적화된 방식으로 요청.
- 다음 새로고침이 일어나기 전에 callback 실행
```javascript
frame = requestAnimationFrame(function loop(t){
  frame = requestAnimationFrame(loop); # 다음 애니메이션 예약
  paint(context, t); # 현재 프레임에 그림
  # 여기까지 실행이 완료된 후 위에서 예약한 loop로 돌아감
})
```
- frame은 애니메이션을 제어하는 식별자
- 매 프레임마다 frame을 갱신
## 🔹 vmin
- 너비와 높이 중 더 작은 값
ex) 너비가 800px이고 높이가 600px이면, 1vmin = 600px

## 🔹 ESM ECMA script module
- import, export를 지원하는 최신 자바스크립트 표준

## 🔹 npx란?
- npx: 설치 없이 npm 패키지를 실행할 수 있게 해주는 CLI 도구
```bash
npx create-svelte myapp
```
## 🔹 스벨트 프로젝트 구조
```
myapp/
├── src/
│   ├── lib/         # 공통 컴포넌트, 유틸 등
│   ├── routes/      # 페이지 라우팅
│   ├── app.html     # HTML 템플릿
│   └── app.d.ts
├── static/          # 정적 파일
├── tests/
├── .svelte-kit/     # 개발 서버/빌드 산출물
├── package.json
├── svelte.config.js
├── tsconfig.json
└── vite.config.js
```

# 참고 링크
1. https://svelte.dev/tutorial/svelte/welcome-to-svelte
2. https://www.inflearn.com/course/%EC%8A%A4%EB%B2%A8%ED%8A%B8-%EC%99%84%EB%B2%BD-%EA%B0%80%EC%9D%B4%EB%93%9C#curriculum 
3. https://svelte.dev/tutorial/svelte/welcome-to-svelte

