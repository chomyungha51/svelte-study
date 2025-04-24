<script>
  import Child from "../lib/Child.svelte";
  import { fade } from "svelte/transition";

  let darkMode = false;

  let count = $state(0);
  let doubled = $derived(count * 2);

  let numbers = $state([1, 2, 3]);
  let total = $derived.by(() => {
    let total = 0;
    for (const n of numbers) {
      total += n;
    }
    return total;
  });

  let object = $state({ count: 0 });
</script>

<h1>Welcome to Svelte</h1>

<button onclick={() => count++}>clicks (parent) +</button>
<p>Parent Count: {count}</p>

<p>Double: {doubled}</p>
<button onclick={() => numbers.push(numbers.length + 1)}>
  add number {numbers.join(" + ")} = {total}
</button>

<Child
  adjective="nice"
  bind:count
  object={{ count: 0 }}
/>

{#if count < 10}
  <p>The number is under 10</p>
{:else if count == 10}
  <p>The number is 10</p>
{:else}
  <p>The number is upper 10</p>
{/if}

{#each numbers as number, i}
  <p>{i + 1}. {number}</p>
{/each}

<!--  표현식의 값이 바꾸면 다시 생성함  -->
{#key count}
  <div transition:fade>{count}</div>
{/key}

<!--  style inline으로 여러 개 적용하는 법 -->
<div
  style="color: blue;"
  style:color="red"
  style:width="12rem"
  style:background-color={darkMode ? "black" : "white"}
>
  This will be red
</div>
