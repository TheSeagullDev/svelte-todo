<script>
	import '../app.css';
	import Trash from '$lib/Trash.svelte';
	import { flip } from 'svelte/animate';
	import { fade } from 'svelte/transition';

	const todos = $state([]);

	let uid = todos.length + 1;

	function add(desc) {
		todos.push({
			done: false,
			text: desc,
			id: uid++
		});
	}

	function remove(id) {
		const toDeleteIndex = todos.findIndex((todo) => todo.id === id);
		todos.splice(toDeleteIndex, 1);
	}
</script>

<div class="min-h-screen bg-slate-900 flex flex-col">
	<div class="m-auto flex h-64 flex-none w-1/2 items-center">
		<input
			type="text"
			class="w-full rounded-2xl bg-slate-800 p-4 text-white shadow-2xl transition hover:scale-103"
			placeholder="what to do?"
			onkeydown={(e) => {
				if (e.key !== 'Enter') return;
				add(e.currentTarget.value);
				e.currentTarget.value = '';
			}}
		/>
	</div>
	<div class="m-auto flex-1 w-1/2">
		{#each todos.toSorted((a, b) => Number(a.done) - Number(b.done)) as todo (todo.id)}
			<div class="my-4 flex rounded-2xl bg-slate-800 p-4 shadow-2xl" transition:fade animate:flip>
				<input type="checkbox" bind:checked={todo.done} />
				<div
					class={[
						'mx-4',
						{ 'text-slate-400 line-through': todo.done },
						{ 'text-white': !todo.done }
					]}
				>
					{todo.text}
				</div>
				<button onclick={() => remove(todo.id)} class="flex-auto flex justify-end">
					<Trash stroke="white"/>
				</button>
			</div>
		{/each}
	</div>
</div>
