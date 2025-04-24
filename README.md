# 📘 스벨트 (Svelte) 공부

## 1. 스벨트 기본 개념
### 🔹스벨트란?
- 컴파일 단계에서 최적화되는 프론트엔드 프레임워크
- 빌드 타임에 Svelte 코드를 순수 JS로 컴파일
- React나 Vue와 달리 런타임 프레임워크가 존재하지 않음
- 결과적으로 빠른 렌더링 성능과 작은 번들 크기

### 🔹개발 흐름
- Svelte 문법 → 컴파일 → 순수 JS → 실행

### 🔹주요 특징
- 가상 DOM 없음 → 직접 DOM 업데이트
- .svelte 파일에서 HTML, CSS, JS를 함께 사용
- useState, setState 없이 변수만 재할당하면 반응성 발생

### 🔹REPL (Read-Eval-Print Loop)
- 스벨트 코드를 실시간 테스트할 수 있는 웹 기반 환경
- https://svelte.dev/repl


## 2. 핵심 문법
### 🔹선언적 렌더링
```html
<h1>{name}님 안녕하세요!</h1>
```
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
### 🔹스토어 (Store)
```svelte
import { writable } from 'svelte/store';
export const count = writable(0);
```
## 3. 라이프사이클 & 반응성
### 🔹라이프사이클 함수
| 메서드        | 설명                                      |
|---------------|-------------------------------------------|
| `onMount`     | 컴포넌트가 DOM에 처음 렌더링 후 실행      |
| `onDestroy`   | 컴포넌트가 제거되기 직전 실행             |
| `beforeUpdate`| 상태 변경으로 DOM 업데이트 직전 실행      |
| `afterUpdate` | DOM 업데이트 직후 실행                    |
| `tick()`      | DOM 업데이트가 완료된 후 다음 코드 실행   |


```svelte
예시 (스크롤 핸들러)
// utils/useScrollHandler.js
import { onMount, onDestroy } from 'svelte';

export function useScrollHandler(callback) {
  function handleScroll(event) {
    callback(event);
  }

  onMount(() => {
    window.addEventListener('scroll', handleScroll);
  });

  onDestroy(() => {
    window.removeEventListener('scroll', handleScroll);
  });
}
```
### 🔹반응성

```svelte
let count = 0;
function add() {
  count += 1; // 반응성 발생
}
```

- 이 부분 아직 이해 안 됨 .. https://svelte.dev/tutorial/svelte/keyed-each-blocks
## 4. 스타일링과 바인딩

### 🔹클래스 바인딩
```svelte
<div class:on={isActive}>활성 상태</div>
<div class:highlight={isHighlighted} class:error={hasError}>상태 표시</div>
```
### 🔹요소 바인딩
```svelte
<input type="checkbox" bind:checked={checked} />
<select bind:value={formData.fruit}>
  <option value="apple">Apple</option>
  <option value="banana">Banana</option>
</select>
```
### 🔹콘텐츠 바인딩
```svelte
<div contenteditable="true" bind:innerHTML={content}></div>
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

## 추가로 궁금한 것들
### 🔹 npx란?
- npx: 설치 없이 npm 패키지를 실행할 수 있게 해주는 CLI 도구
```bash
npx create-svelte myapp
```
### 🔹 스벨트 프로젝트 구조
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

