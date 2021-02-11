<script>
 import { onMount } from "svelte";
	export let text = "";
	export let name = "";
	let page = 0;
	let pageSize = 1;
	let json= {};
	let totalpage = 0;
	
	onMount(function(){
	thispage(page)}
	);

  function nextpage() {
    page += 1;
	thispage(page);
  }

  function prevpage() {
    page -= 1;
	thispage(page);
  }
  function thispage(x) {
    page = x;
	let query = [];
	let url = 'http://localhost:3000/list/';
	if (page > 0){
		query.push('page=' + page);
	}
	if (pageSize > 1){
		query.push('pageSize=' + pageSize);
	}
	if (query.length > 0){
		url = url + '?' + query.join('&');
	}
	getFetch(url).then(res => {
	json = res;
	text = json.text;
	totalpage = json.settings.totalpage;
	if (totalpage - 1 < page){
		page = totalpage - 1;
		thispage(page);
	}
	});
  }
  async function getFetch(url) {
    let response = await fetch(url);
	let resp = await response.json();
    return resp;
	}
</script>

<main>
	<h1>Hello {name}!</h1>
	<p>{text}</p>
	<div>Текущея страница: {page + 1}</div>
	<div>
		{#each  Array(totalpage) as _, i}
			<button on:click={() => thispage(i)}>page{ i + 1 }</button>
		{/each}
	</div>
	<div>
		{#if page > 0}
			<button on:click={prevpage}>prevpage</button>
		{/if}
		{#each  Array(page) as _, i}
			<button on:click={() => thispage(i)}>page{ i + 1 }</button>
		{/each}
		<button on:click={nextpage}>nextpage</button>
	</div>


</main>

<style>
	main {
		text-align: center;
		padding: 1em;
		max-width: 240px;
		margin: 0 auto;
	}

	h1 {
		color: #ff3e00;
		text-transform: uppercase;
		font-size: 4em;
		font-weight: 100;
	}

	@media (min-width: 640px) {
		main {
			max-width: none;
		}
	}
</style>