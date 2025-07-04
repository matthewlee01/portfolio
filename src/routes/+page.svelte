<script>
	import Item from '$lib/components/Item.svelte';
	import '@fontsource-variable/inter';
	import { fade } from 'svelte/transition';

	export let data;

	let expanded = true;
	let selectedTags = new Set();
	let hidden = true;
	let items = data.items.map((item) => {
		return {
			...item,
			irrelevant: false,
			expanded: false
		};
	});

	const toggleTag = (tag) => {
		if (selectedTags.has(tag)) {
			selectedTags.delete(tag);
		} else {
			selectedTags.add(tag);
		}
		selectedTags = selectedTags;
		updateItems();
	};

	const updateItems = () => {
		if (selectedTags.size === 0) {
			items = items.map((item) => {
				return {
					...item,
					irrelevant: false
				};
			});
		} else {
			items = items.map((item) => {
				let selected = item.tags.reduce((acc, tag) => {
					return acc || selectedTags.has(tag);
				}, false);
				return {
					...item,
					irrelevant: !selected
				};
			});
		}
	};
	setTimeout(() => {
		hidden = false;
	}, 200);
</script>

{#if !hidden}
	<div id="container" transition:fade={{ duration: 300 }}>
		<div class="control panel">
			<div id="header">
				<span><span class="name">Matthew J Lee.</span></span>
				<span class="title">Portfolio.</span>
			</div>
			<div id="info">
				<div>
					<span>An interactive record of my work.</span><br />
					<span><a href="https://github.com/matthewlee01" target="_blank">GitHub. </a></span>
					<span
						><a href="https://www.linkedin.com/in/matthew-lee01/" target="_blank"
							>LinkedIn.
						</a></span
					>
					<span><a href="/resume.pdf" target="_blank">Resume.</a></span>
				</div>
				<button id="expand" on:click={() => (expanded = !expanded)}>
					{expanded ? '△' : '▽'}
				</button>
			</div>
			<div class={'collapsible' + (expanded ? ' expanded' : '')}>
				<div class="tag-filters">
					{#each data.tagDirectory as group (group.category)}
						<div class="tag-group">
							<span class="tag-category">{group.category}</span>
							<div class="tags">
								{#each group.tags as tag (tag + selectedTags)}
									<button
										class={'tag ' + (selectedTags.has(tag) ? 'selected' : 'deselected')}
										id={tag}
										on:click={toggleTag(tag)}>{tag}</button
									>
								{/each}
							</div>
						</div>
					{/each}
				</div>
			</div>
		</div>
		<div class="data panel">
			{#each items as item (item.project + item.organization + item.position + item.irrelevant)}
				<Item {item} />
			{/each}
		</div>
		<div class="footer panel">
			<span
				><a href="https://matthewjl.xyz/">Home.</a>
				<a href="https://github.com/matthewlee01" target="_blank">GitHub. </a>
				<a href="https://www.linkedin.com/in/matthew-lee01/" target="_blank">LinkedIn. </a></span
			>
			<span>© 2025 Matthew J Lee.</span>
		</div>
	</div>
{/if}

<style>
	:global(:root) {
		--dark: #53917e;
		--light: #fff8f0;
		--light-mid: #fff0d7;
		--dark-mid: #73c9af;
		--accent: #bf4e22;
		--accent-mid: #783116;
	}

	a {
		color: var(--dark);
	}

	:global(*) {
		box-sizing: border-box;
		font-family: 'Inter Variable', sans-serif;
		letter-spacing: -0px;
	}

	:global(body) {
		margin: 0;
		padding: 0;
		background-color: var(--light);
	}

	span {
		color: var(--dark);
	}

	#container {
		display: flex;
		flex-direction: column;
		justify-content: center;
		align-items: center;
		padding: 16px;
		margin-top: 48px;
		margin-bottom: 128px;
	}

	#header {
		display: flex;
		justify-content: space-between;
	}

	.name {
		font-weight: 700;
		font-size: 32px;
		margin-right: 4px;
		color: var(--accent);
	}

	.title {
		font-weight: 200;
		font-size: 32px;
	}

	.panel {
		width: 100%;
		max-width: 666px;
	}

	#info {
		display: flex;
		margin: 4px 0;
		justify-content: space-between;
	}

	#info span {
		font-size: 14px;
	}

	.collapsible {
		display: grid;
		grid-template-rows: 0fr;
		width: 100%;
		transition: all 0.16s ease-in-out;
	}

	.collapsible.expanded {
		grid-template-rows: 1fr;
	}

	#expand {
		font-size: 12px;
		margin: 4px 0;
		padding: 0px 16px;
		box-shadow: 0px 1px 0px 0px var(--dark);
		color: var(--dark);
	}

	.tag-filters {
		opacity: 0;
		overflow: hidden;
		transition: all 0.66s ease-in-out;
	}

	.expanded .tag-filters {
		opacity: 1;
	}

	button {
		border: 1px solid var(--dark);
		background-color: var(--light);
		padding: 8px 12px;
		cursor: pointer;
		font-size: 12px;
	}

	.tag-group {
		display: flex;
		flex-direction: row;
		justify-content: space-between;
		align-items: flex-start;
		margin: 8px 0;
	}

	.tags {
		display: flex;
		flex-wrap: wrap;
		flex-direction: row-reverse;
	}

	.tag-category {
		font-size: 12px;
		font-weight: 600;
		margin-top: 8px;
		margin-right: 32px;
		min-width: max-content;
	}

	.tag {
		margin-left: 2px;
		margin-bottom: 2px;
		color: var(--dark);
		box-shadow: 0px 1px 0px 0px var(--dark);
	}

	.tag.selected {
		background-color: var(--accent);
		color: var(--light);
		box-shadow: 0px 1px 0px 0px var(--accent-mid);
		border: 1px solid var(--accent-mid);
	}

	.data {
		margin-top: 8px;
	}

	.footer {
		display: flex;
		justify-content: space-between;
		margin-top: 4px;
		font-size: 12px;
	}

	@media (min-width: 666px) {
		.tag:hover {
			background-color: var(--light-mid);
		}

		.tag.selected:hover {
			background-color: var(--accent-mid);
		}
		#expand:hover {
			background-color: var(--light-mid);
		}
	}
</style>
