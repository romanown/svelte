<script>
  import { onMount } from "svelte";
  export let name = "";
  export let recivedData = {};
  let currentPage = 0;
  let pageSize = 1;
  let totalPage = 1;
  let url = "http://localhost:3000/list/";

  onMount(function() {
    getPage(currentPage);
  });

  function nextpage() {
    currentPage += 1;
    getPage(currentPage);
  }

  function prevpage() {
    currentPage -= 1;
    getPage(currentPage);
  }

  function getPage(x) {
    currentPage = x;
    let urlToSend = url + dataToSend();
    getFetch(urlToSend).then((res) => {
      recivedData = res;
      totalPage = recivedData.settings.totalPage;
      if (totalPage - 1 < currentPage) {
        getPage(totalPage - 1);
      }
    });
  }

  function dataToSend() {
    let query = [];
    if (currentPage > 0) {
      query.push("page=" + currentPage);
    }
    if (pageSize > 1) {
      query.push("pageSize=" + pageSize);
    }
    if (query.length > 0) {
      return "?" + query.join("&");
    }
    return "";
  }

  async function getFetch(url) {
    return await fetch(url).then((res) => {
      return res.json();
    });
  }
</script>

<main>
  <h1>Hello {name}!</h1>
  <p>{recivedData.text}</p>
  <div>Текущея страница: {currentPage + 1}</div>
  <div>
    {#each Array(totalPage) as _, i}
      <button on:click={() => getPage(i)}>page{i + 1}</button>
    {/each}
  </div>
  <div>
    {#if currentPage > 0}
      <button on:click={prevpage}>prevpage</button>
    {/if}
    {#each Array(currentPage) as _, i}
      <button on:click={() => getPage(i)}>page{i + 1}</button>
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
