<script>
	export let item;
	let { position, project, organization, startDate, endDate, highlights, irrelevant } = item;
</script>

<div
	class={'portfolio-item' + (irrelevant ? ' irrelevant' : '') + (item.expanded ? ' expanded' : '')}
>
	<button class="header" on:click={() => (item.expanded = !item.expanded)}>
		<div class="title">
			<span class="heavy">{organization}.</span><span class="light">{project}.</span>
		</div>
		<div class="dates">
			<span class="heavy">
				{#if startDate}
					{startDate} -
				{/if}
				{endDate}.
			</span>
			<span class="light">
				{#if position}
					{position}.
				{/if}
			</span>
		</div>
	</button>
	<div class={'collapsible' + (item.expanded ? ' expanded' : '')}>
		<div class="content">
			<ul class="highlights">
				{#each highlights as highlight (highlight)}
					<li>{highlight}</li>
				{/each}
				{#if item.link}
					<li><a target="_blank" href={item.link.href}>{item.link.text}</a></li>
				{/if}
			</ul>
		</div>
	</div>
</div>

<style>
	a {
		color: var(--accent);
	}

	.title,
	.dates {
		display: flex;
		align-items: center;
		justify-content: space-between;
		gap: 8px;
	}

	.title {
		font-size: 16px;
	}

	.dates {
		font-size: 12px;
	}

	.heavy {
		font-weight: 700;
		text-align: left;
	}

	.light {
		font-weight: 300;
		text-align: right;
	}

	.irrelevant {
		opacity: 0.16;
	}

	.collapsible {
		display: grid;
		grid-template-rows: 0fr;
		color: var(--light);
		transition: grid-template-rows 0.1s ease-in-out, color 0.5s ease-in-out;
		margin-bottom: 8px;
		border: 1px solid var(--dark);
	}

	.collapsible.expanded {
		grid-template-rows: 1fr;
		color: var(--dark);
		border: 1px solid var(--accent-mid);
		color: var(--accent-mid);
	}

	.content {
		overflow: hidden;
		border-top: none;
	}

	.highlights {
		margin: 8px 16px;
		padding: 4px 8px;
		font-size: 12px;
	}

	li {
		margin: 4px 0px;
	}

	button {
		width: 100%;
		border: none;
		cursor: pointer;
		user-select: text;
		border: 1px solid var(--dark);
		background-color: var(--light);
		margin-bottom: -1px;
		margin-top: -1px;
		padding: 12px;
		color: var(--dark);
		box-shadow: 0px 2px 0px 0px var(--dark);
	}

	.expanded button {
		background-color: var(--accent);
		border: 1px solid var(--accent-mid);
		color: var(--light);
		box-shadow: 0px 2px 0px 0px var(--accent-mid);
	}
	@media (min-width: 666px) {
		.expanded button:hover {
			background-color: var(--accent-mid);
		}

		button:hover {
			background-color: var(--light-mid);
			box-shadow: none;
		}
	}
</style>
