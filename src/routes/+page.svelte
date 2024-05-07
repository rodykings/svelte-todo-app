<script>
	import { onMount } from 'svelte';

	/**
	 * @type {HTMLInputElement}
	 */
	let inputEl;

	let newTodo = '';
	/**
	 * @type {string[]}
	 */
	let todos = [];

	onMount(() => {
		const localStorageTodos = localStorage?.getItem('todos');
		todos = JSON.parse(localStorageTodos || '{}') || [];
	});

	function updateLocalStorage() {
		localStorage.setItem('todos', JSON.stringify(todos));
	}

	function addTodo() {
		todos = [...todos, newTodo];
		updateLocalStorage();
		newTodo = '';
		inputEl.focus();
	}

	/**
	 * @param {number} i
	 */
	function deleteTodo(i) {
		todos = todos?.filter((_, index) => index != i);
		updateLocalStorage();
	}
</script>

<svelte:head>
	<title>My Todo App</title>
	<meta name="description" content="My todo app" />
</svelte:head>

<h2>My Todos</h2>

<form on:submit={addTodo}>
	<input id="task" placeholder="My task" bind:this={inputEl} bind:value={newTodo} />
	<button id="add-button">Add</button>
</form>

<div class="todos-list">
	{#each todos as todo, i (i)}
		<article class="todo">
			<h4>{todo}</h4>
			<button on:click={() => deleteTodo(i)}>Delete</button>
		</article>
	{/each}
</div>

<style>
	form {
		display: flex;
		justify-content: start;
		align-items: center;
		gap: 4px;
	}

	input {
		width: 100%;
		border-radius: 0.5rem;
		height: 40px;
		border: 2px solid #cccccc;
		font-size: 16px;
		padding: 0 8px;
	}

	button {
		cursor: pointer;
	}

	#add-button {
		min-width: 100px;
		border-radius: 0.5rem;
		height: 40px;
		border: 2px solid #cccccc;
		font-size: 16px;
		padding: 0 8px;
	}

	.todo {
		display: flex;
		justify-content: space-between;
		align-items: center;
		height: 40px;
		padding: 0 16px;
		border-radius: 8px;
		font-weight: 200;
		border: 2px solid #cccccc;
	}

	.todos-list {
		margin-top: 16px;
		display: flex;
		flex-direction: column;
		gap: 4px;
		padding: 0;
	}
</style>
