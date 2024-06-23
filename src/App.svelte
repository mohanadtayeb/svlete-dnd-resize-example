<script>
	import { dndzone } from 'svelte-dnd-action';
	import { resize } from '@svelte-put/resize';
	import Component1 from './Component1.svelte';
	import Component2 from './Component2.svelte';
	import Component3 from './Component3.svelte';
  
	let components = [];
	let nextId = 1;
	let selectedComponent = 'Component1';
	let mode = 'off';
  
	const componentTypes = ['Component1', 'Component2', 'Component3'];
  
	function addComponent() {
	  components = [...components, { id: nextId++, type: selectedComponent }];
	}
  
	function removeComponent(id) {
	  components = components.filter(comp => comp.id !== id);
	}
  
	function handleDrop(event) {
	  components = event.detail.items;
	}

	$: console.log(mode)
  </script>
  
  <div class="controls">
	<select bind:value={selectedComponent}>
	  {#each componentTypes as type}
		<option value={type}>{type}</option>
	  {/each}
	</select>
	<button on:click={addComponent}>Add Component</button>
  </div>
  
  <div class="modes">
	<label>
	  <input type="radio" name="mode" value="off" bind:group={mode}>
	  Off
	</label>
	<label>
	  <input type="radio" name="mode" value="resize" bind:group={mode}>
	  Resize
	</label>
	<label>
	  <input type="radio" name="mode" value="drag" bind:group={mode}>
	  Drag
	</label>
  </div>


  
  <div class="layout"  use:dndzone={{ items: components, flipDurationMs: 300 }} on:consider={handleDrop} on:finalize={handleDrop} class:drag-mode={mode === 'drag'}>
	{#each components as component (component.id)}
	  <div class="component-wrapper" class:drag-mode={mode === 'drag'} use:resize={mode === 'resize' ? { edges: { right: true, bottom: true }, minSize: { width: 100, height: 100 } } : undefined}>
		{#if component.type === 'Component1'}
		  <Component1 id={component.id} />
		{:else if component.type === 'Component2'}
		  <Component2 id={component.id} />
		{:else if component.type === 'Component3'}
		  <Component3 id={component.id} />
		{/if}
		<button class="remove-btn" on:click={() => removeComponent(component.id)}>Remove</button>
	  </div>
	{/each}
  </div>
  
  <style>
	.controls {
	  display: flex;
	  justify-content: flex-start;
	  align-items: center;
	  margin-bottom: 20px;
	}
  
	select {
	  padding: 5px;
	  margin-right: 10px;
	  border-radius: 5px;
	  border: 1px solid #ccc;
	}
  
	button {
	  padding: 5px 10px;
	  border-radius: 5px;
	  border: none;
	  background-color: #007bff;
	  color: white;
	  cursor: pointer;
	  transition: background-color 0.3s;
	}
  
	button:hover {
	  background-color: #0056b3;
	}
  
	.modes {
	  margin-bottom: 20px;
	}
  
	.layout {
	  display: grid;
	  grid-template-columns: repeat(auto-fill, minmax(200px, 1fr));
	  grid-template-rows: repeat(auto-fill, minmax(100px, 1fr));
	  grid-auto-flow: dense;
	  grid-auto-rows: 100px;
	  grid-auto-columns: 200px;
	  
	  grid-gap: 10px;
	  width: 100%;
	  height: 100%;
	  flex-wrap: wrap;
	  gap: 10px;
	}
  
	.drag-mode .component-wrapper {
	  cursor: move;
	}
  
	.component-wrapper {
	  position: relative;
	  display: flex;
	  flex-direction: column;
	  align-items: center;
	  border: 1px dashed #ccc;
	  margin: 10px;
	  overflow: auto;
	  resize: both;
	}
  
	.remove-btn {
	  margin-top: 5px;
	  padding: 3px 7px;
	  border-radius: 5px;
	  border: none;
	  background-color: #dc3545;
	  color: white;
	  cursor: pointer;
	  transition: background-color 0.3s;
	}
  
	.remove-btn:hover {
	  background-color: #c82333;
	}
  </style>
  

