<script>
	import Folder from './Folder.svelte';
	import File from './File.svelte';

	let { expanded = $bindable(false), name, files } = $props();

	function toggle() {
		expanded = !expanded;
	}
</script>

<button class:expanded onclick={toggle}>{name}</button>

{#if expanded}
	<ul>
		{#each files as file}
			<li>
				{#if file.files}
					<Folder {...file} />
				{:else}
					<File {...file} />
				{/if}
			</li>
		{/each}
	</ul>
{/if}

<style>
	button {
		padding: 0 0 0 1.5em;
		background: url(/tutorial/icons/folder.svg) 0 0.1em no-repeat;
		background-size: 1em 1em;
		color: var(--fg-1);
		font-weight: bold;
		cursor: pointer;
		border: none;
		margin: 0;
	}

	.expanded {
		background-image: url(/tutorial/icons/folder-open.svg);
	}

	ul {
		padding: 0.2em 0 0 0.5em;
		margin: 0 0 0 0.5em;
		list-style: none;
		border-left: 1px solid rgba(128, 128, 128, 0.4);
	}

	li {
		padding: 0.2em 0;
	}
</style>
