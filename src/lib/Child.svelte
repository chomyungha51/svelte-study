<!--
@component
- 컴포넌트 설명을 작성할 수 있습니다.
-->
<script>
  /** @type {{ adjective: string, count: number, object:{count:number} }} */
  let { adjective = "awesome", count = $bindable(), object } = $props();
  // object가 일반 객체면 자식에서 바꿔도 다시 렌더링되지 않는 이상 안 바뀌는데, state 객체면 바로 바뀐다. 하지만 이렇게 쓰지 마라
  // 그냥 state값을 넘기면 일시적으로 자식 범위에서 업데이트할 수 있지만 다시 렌더링되면 부모에서 내려준 값으로 보임
</script>

<p>this component is {adjective}</p>

<button onclick={() => count++}>cilcks (child): {count} +</button>

<button
  onclick={() => {
    // has no effect
    object.count += 1;
  }}
>
  click object count : {object.count}
</button>

<input bind:value={count} />

<style>
  input {
    font-family: "Comic Sans MS";
    color: deeppink;
  }
</style>
