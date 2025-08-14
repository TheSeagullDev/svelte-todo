<script>
	import '../app.css';
	import Trash from '$lib/Trash.svelte';
	import Check from '$lib/Check.svelte';
	import Edit from '$lib/Edit.svelte';
	import { flip } from 'svelte/animate';
	import { fade } from 'svelte/transition';

	const todos = $state([]);

	let uid = todos.length + 1;

	function add(desc) {
		todos.push({
			done: false,
			text: desc,
			editing: false,
			id: uid++,
			contentElement: null
		});
	}

	function remove(id) {
		const toDeleteIndex = todos.findIndex((todo) => todo.id === id);
		todos.splice(toDeleteIndex, 1);
	}
</script>

<div class="flex min-h-screen flex-col bg-slate-900">
	<div class="m-auto flex h-64 w-1/2 flex-none items-center">
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
	<div class="m-auto w-1/2 flex-1">
		{#each todos.toSorted((a, b) => Number(a.done) - Number(b.done)) as todo (todo.id)}
			<div class="my-4 flex rounded-2xl bg-slate-800 p-4 shadow-2xl" transition:fade animate:flip>
				<input type="checkbox" bind:checked={todo.done} />
				{#if !todo.editing}
				<div
					bind:this={todo.contentElement}
					class={[
						'mx-4 flex-1 rounded-md p-1',
						{ 'text-slate-400 line-through': todo.done },
						{ 'text-white': !todo.done },
						{ 'bg-slate-600': todo.editing}
					]}
				>
					{todo.text}
				</div>
				{:else}
				<input
					bind:this={todo.contentElement}
					bind:value={todo.text}
					onkeydown={(e) => {
						if(e.key === "Enter") {
							todo.editing = false;
						}
					}}
					class={[
						'mx-4 flex-1 rounded-md p-1',
						{ 'text-slate-400 line-through': todo.done },
						{ 'text-white': !todo.done },
						{ 'bg-slate-600': todo.editing}
					]}
				>
				{/if}
				<div class="flex flex-auto justify-end">
					{#if !todo.editing}
						<button
							onclick={() => {
								todo.editing = true;
								setTimeout(() => todo.contentElement.focus(), 0);
							}}
						>
							<Edit stroke="white" />
						</button>
					{:else}
						<button onclick={() => (todo.editing = false)}>
							<Check stroke="white" />
						</button>
					{/if}
					<button onclick={() => remove(todo.id)}>
						<Trash stroke="white" />
					</button>
				</div>
			</div>
		{/each}
	</div>
</div>
