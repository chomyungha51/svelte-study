<script>
  import MyComponent from '../lib/MyComponent.svelte'

  // Runes 종류

  // 1. $state
  // 단일 값을 state에 넣어서 props로 줄 땐, child에서 임시로 덮어씀
  let count = $state(0);

  // 2. $derived
  let doubled = $derived( count * 2);

  let numbers = $state([1,2,3]);
  let total = $derived.by(() =>{
    let total = 0;
    for (const n of numbers) {
        total += n;
    }
    return total;
  })

  // 3. $props
  let object = $state({count: 0})
  
  // 4 $bindable 로 선언한 props는 넘겨줄 때도 bind:붙여서 줄 수 있음

</script>

<h1>Welcome to SvelteKit</h1>

<button onclick={() => count++}>clicks (parent) +</button>
<p>Parent Count: {count}</p>

<p>Double: {doubled}</p>
<button onclick={() => numbers.push(numbers.length + 1)}>
	add number {numbers.join(' + ')} = {total}
</button>

<MyComponent adjective="nice" bind:count={count} object={{count:0}}/>