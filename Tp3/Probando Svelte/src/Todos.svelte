<script>
  import { slide } from "svelte/transition";
  import { elasticInOut } from "svelte/easing";
  import Card from '@smui/card/bare.js';
  import '@smui/card/bare.css';
  import Switch from '@smui/switch/bare.js';
  import '@smui/switch/bare.css';
  import Fab from '@smui/fab/bare.js';
  import '@smui/fab/bare.css';
  
  function agregarTarea() {
    if (input)
      tareas = [
        ...tareas,
        {
          text: input,
          id: Math.random()
            .toString(36)
            .substr(2, 9)
        }
      ];
    input = "";
  }

  function removerTarea(id) {
    const index = tareas.findIndex(todo => todo.id === id);
    tareas.splice(index, 1);
    tareas = tareas;
  }

  function download() {
  var element = document.createElement('a');
  var filtro = tareas.map(({text})=>({text}));

  var texto = JSON.stringify(filtro,null,2);
  element.setAttribute('href', 'data:text/plain;charset=utf-8,' + encodeURIComponent(texto));
  element.setAttribute('download', "Tareas.txt");
  element.style.display = 'none';
  document.body.appendChild(element);
  element.click();
  document.body.removeChild(element);
}
  let darkMode=false;
  $:pendientes = tareas.length;
  let input="";
  let tareas = [
	{text: 'Aprender Svelte',id: Math.random().toString(36).substr(2, 9)},
	{text: 'Practicar JS',id: Math.random().toString(36).substr(2, 9)},
	{text: 'Subir tareas nuevas',id: Math.random().toString(36).substr(2, 9)},
	{text: 'Eliminar tareas viejas',id: Math.random().toString(36).substr(2, 9)},
	{text: 'Descifrar como hacer que funcione modo oscuro',id: Math.random().toString(36).substr(2, 9)}
  ];
</script>

<style>
    .margins {
    margin: 0 .4em .4em 0;
  }
</style>

<svelte:head>
  {#if darkMode}
    <link rel="stylesheet" href="https://jenil.github.io/bulmaswatch/slate/bulmaswatch.min.css">
	{:else}
    <link rel="stylesheet" href="https://jenil.github.io/bulmaswatch/default/bulmaswatch.min.css">
  {/if}
  <script src="https://use.fontawesome.com/releases/v5.3.1/js/all.js"></script>
</svelte:head>


<main class="container is-fluid">
  <body>
  <div class="columns is-centered is-vcentered is-mobile">
    <div class="column is-narrow" style="width: 80%">
      <h1 class="has-text-centered title">Lista de tareas Svelte</h1>
      <div class="card-container short columns is-centered is-vcentered is-mobile">
        <Card style="width: 60%;" padded>Tienes {pendientes} {pendientes === 1? 'tarea pendiente': 'tareas pendientes'}</Card>
      </div>
      <form class="field has-addons" style="justify-content: center" on:submit|preventDefault={agregarTarea}>
        <div class="control" style="width: 60%;">
          <input bind:value={input} class="input" type="text" placeholder="Nueva tarea">
        </div>
        <div class="control">
          <button class="button is-primary">
            <span class="icon is-small">
              <i class="fas fa-plus"></i>
            </span>
          </button>
        </div>
      </form>
      <ul class:list={tareas.length > 0}>
        {#each tareas as todo (todo.id)}
          <li class="list-item" transition:slide="{{duration: 300, easing: elasticInOut}}">
            <div class="is-flex" style="align-items: center">
              <span class="is-pulled-left">{todo.text}</span>
              <div style="flex: 1"></div>
              <button class="button is-text is-pulled-right is-small" on:click={()=> removerTarea(todo.id)}>
                <span class="icon">
                  <i class="fas fa-check"></i>
                </span>
              </button>
            </div>
          </li>
        {:else}
          <li class="has-text-centered" transition:slide="{{delay: 600, duration: 300, easing: elasticInOut}}">Nothing here!</li>
        {/each}
      </ul>
      
      <div class="margins">
        <Fab on:click={download} extended>Descargar</Fab>
      </div> 
      <div>
        <Switch bind:checked={darkMode}/>
        <span>Modo oscuro</span>
    </div> 
    </div>
  </div>
</main>