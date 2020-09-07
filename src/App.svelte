<script>

import AddTodo from "./components/AddTodo.svelte";

	// export let name;
	// let bisa diganti, const tidak bisa/static
	let name = "Hi, Sean";

	let todoList = ["Bangun tidur", "Mandi", "Gosok gigi"];

	// setTimeout(() => {
	// 	todoList = [...todoList, "Tidur again"];
	// }, 1000);

	let updateTodoIndex = null;

	let updateTodoText = "";

	function addNewTodo(e){
		e.preventDefault();
		todoList = [...todoList, e.detail.newTodo];
	}

	function deleteTodo(deleteTodo){
		// alert("Hapus " + todo);
		const indexDeleteTodo = todoList.findIndex(todo => todo === deleteTodo);

		todoList = [
			...todoList.slice(0, indexDeleteTodo),
			...todoList.slice(indexDeleteTodo + 1 , todoList.length)
		]
	}

	function updateTodo(updateTodo){
		updateTodoIndex = todoList.findIndex(todo => todo === updateTodo);

		updateTodoText = updateTodo;
	}

	function submitUpdateTodo(e){
		if(e.keyCode === 13){
			const indexUpdateTodo = todoList.findIndex((todo, index) => index === updateTodoIndex);

			todoList = [
				...todoList.slice(0, indexUpdateTodo),
				updateTodoText,
				...todoList.slice(indexUpdateTodo + 1 , todoList.length)
			]
			
			updateTodoIndex = null;
		}

	}


	let searchValue = "";
	$: filteredTodo = todoList.filter(todo => todo.match(new RegExp(searchValue, 'i')));


</script>

<main>
	<div class="min-h-screen bg-gray-100 flex flex-row justify-center items-start p-10">
		<div class="bg-white pt-3 rounded shadow-xl w-1/2">
		  <div class="px-3 mb-2">
			<span class="tracking-wide leading-normal text-lg text-gray-800">{name}</span>
		  </div>
  
		  <AddTodo on:onAddTodo={addNewTodo}/>
  
		  <div class="bg-gray-100 py-3 rounded-b">
			<div class="px-3 mb-2 flex flex-row justify-end">
			  <div class="relative">
				<input
				  class="bg-gray-100 focus:bg-white focus:border-gray-200 border p-1 rounded appearance-none focus:outline-none pl-5 "
				  placeholder="Cari todo list"
				  bind:value={searchValue}
				/>
  
				<span class="absolute inset-y-0 left-0 flex items-center pl-1">
				  <svg
					xmlns="http://www.w3.org/2000/svg"
					viewBox="0 0 24 24"
					class="icon-search w-4 h-4 fill-current text-gray-100 focus:text-black"
				  >
					<circle cx="10" cy="10" r="7" class="primary" />
					<path
					  class="secondary fill-current text-gray-400"
					  d="M16.32 14.9l1.1 1.1c.4-.02.83.13 1.14.44l3 3a1.5 1.5 0 0 1-2.12 2.12l-3-3a1.5 1.5 0 0 1-.44-1.14l-1.1-1.1a8 8 0 1 1 1.41-1.41zM10 16a6 6 0 1 0 0-12 6 6 0 0 0 0 12z"
					/>
				  </svg>
				</span>
			  </div>
			</div>
  
			<div class="px-3 flex flex-col">
				{#each filteredTodo as todo, index}
				<div class="flex flex-row w-100 mb-2 py-1 justify-between items-center">
					{#if index === updateTodoIndex}
					<input
						class="bg-gray-100 focus:bg-white focus:border-gray-200 border p-2 flex-grow rounded appearance-none focus:outline-none mr-2"
						placeholder="Tekan Enter untuk mengupdate todo"
						on:keyup={submitUpdateTodo}
						bind:value={updateTodoText}
					/>
					{:else}
					<span class="leading-normal tracking-wide text-lg text-gray-800">{todo}</span>
					{/if}
					<div class="flex flex-row">
					  <!-- Edit -->
					  <svg
						xmlns="http://www.w3.org/2000/svg"
						viewBox="0 0 24 24"
						class="icon-edit w-4 h-4 mr-2 fill-current text-gray-400 hover:text-blue-500 cursor-pointer"
						on:click={e => updateTodo(todo)}
					  >
						<path
						  class="primary"
						  d="M4 14a1 1 0 0 1 .3-.7l11-11a1 1 0 0 1 1.4 0l3 3a1 1 0 0 1 0 1.4l-11 11a1 1 0 0 1-.7.3H5a1 1 0 0 1-1-1v-3z"
						/>
						<rect width="20" height="2" x="2" y="20" class="secondary" rx="1" />
					  </svg>
	  
					  <!-- Delete -->
					  <svg
						xmlns="http://www.w3.org/2000/svg"
						viewBox="0 0 24 24"
						class="icon-trash w-4 h-4 fill-current text-gray-400 hover:text-red-500 cursor-pointer"
						on:click={e => deleteTodo(todo)}
					  >
						<path
						  class="primary"
						  d="M5 5h14l-.89 15.12a2 2 0 0 1-2 1.88H7.9a2 2 0 0 1-2-1.88L5 5zm5 5a1 1 0 0 0-1 1v6a1 1 0 0 0 2 0v-6a1 1 0 0 0-1-1zm4 0a1 1 0 0 0-1 1v6a1 1 0 0 0 2 0v-6a1 1 0 0 0-1-1z"
						/>
						<path
						  class="secondary"
						  d="M8.59 4l1.7-1.7A1 1 0 0 1 11 2h2a1 1 0 0 1 .7.3L15.42 4H19a1 1 0 0 1 0 2H5a1 1 0 1 1 0-2h3.59z"
						/>
					  </svg>
					</div>
				  </div>
				{/each}
			  
			  <!-- <div class="flex flex-row w-100 mb-2 py-1 justify-between items-center">
				<input
				  class="bg-gray-100 focus:bg-white focus:border-gray-200 border p-2 flex-grow rounded appearance-none focus:outline-none mr-2"
				  placeholder="Tekan Enter untuk mengupdate todo"
				/>
				<div class="flex flex-row">
				  <svg
					xmlns="http://www.w3.org/2000/svg"
					viewBox="0 0 24 24"
					class="icon-edit w-4 h-4 mr-2 fill-current text-gray-400 hover:text-blue-500 cursor-pointer"
				  >
					<path
					  class="primary"
					  d="M4 14a1 1 0 0 1 .3-.7l11-11a1 1 0 0 1 1.4 0l3 3a1 1 0 0 1 0 1.4l-11 11a1 1 0 0 1-.7.3H5a1 1 0 0 1-1-1v-3z"
					/>
					<rect width="20" height="2" x="2" y="20" class="secondary" rx="1" />
				  </svg>
  
				  <svg
					xmlns="http://www.w3.org/2000/svg"
					viewBox="0 0 24 24"
					class="icon-trash w-4 h-4 fill-current text-gray-400 hover:text-red-500 cursor-pointer"
				  >
					<path
					  class="primary"
					  d="M5 5h14l-.89 15.12a2 2 0 0 1-2 1.88H7.9a2 2 0 0 1-2-1.88L5 5zm5 5a1 1 0 0 0-1 1v6a1 1 0 0 0 2 0v-6a1 1 0 0 0-1-1zm4 0a1 1 0 0 0-1 1v6a1 1 0 0 0 2 0v-6a1 1 0 0 0-1-1z"
					/>
					<path
					  class="secondary"
					  d="M8.59 4l1.7-1.7A1 1 0 0 1 11 2h2a1 1 0 0 1 .7.3L15.42 4H19a1 1 0 0 1 0 2H5a1 1 0 1 1 0-2h3.59z"
					/>
				  </svg>
				</div>
			  </div> --> 
			</div>
		  </div>
		</div>
	  </div>
</main>

<style>
	main {
		text-align: center;
		padding: 0;
		max-width: 240px;
		margin: 0;
	}

	@media (min-width: 640px) {
		main {
			max-width: none;
		}
	}
</style>