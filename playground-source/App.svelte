<script>
	import Tags from '../src/Tags.svelte';

	let basic = $state(['Apple', 'Banana', 'Cherry', 'Date', 'Elderberry']);
	let widths = $state(['A', 'A much longer tag than the others', 'B', 'Medium tag', 'C', 'Another quite long tag', 'D']);
	let duplicates = $state(['x', 'y', 'x', 'z', 'y']);
	let objects = $state([
		{ id: 1, name: 'Argentina' },
		{ id: 2, name: 'Brazil' },
		{ id: 3, name: 'Chile' },
	]);
	let withDrop = $state(['one', 'two', 'three']);
	let left = $state(['L1', 'L2', 'L3']);
	let right = $state(['R1', 'R2', 'R3']);
	let toggled = $state(['alpha', 'beta', 'gamma']);
	let disable = $state(false);
	let readonly = $state(false);
	let allowReorder = $state(true);

	const countries = [
		{ id: 1, name: 'Argentina' },
		{ id: 2, name: 'Brazil' },
		{ id: 3, name: 'Chile' },
		{ id: 4, name: 'Colombia' },
		{ id: 5, name: 'Mexico' },
		{ id: 6, name: 'Peru' },
	];

	let log = $state([]);
	const logReorder = (label) => (tag, tags, from, to) => {
		const name = typeof tag === 'string' ? tag : tag.name;
		log = [`[${label}] "${name}" ${from} → ${to}`, ...log].slice(0, 12);
	};
	const show = (tags) => JSON.stringify(tags.map((t) => (typeof t === 'string' ? t : t.name)));
</script>

<main>
	<h1><code>allowReorder</code> playground</h1>
	<p class="hint">
		Drag tags to reorder. The order should preview while dragging, but the bound value below each
		input should only change on drop. Drop outside the box or press Escape to cancel.
	</p>

	<section>
		<h2>1. Basic</h2>
		<Tags bind:tags={basic} allowReorder onTagReordered={logReorder('basic')} placeholder="Add a fruit..." />
		<pre>{show(basic)}</pre>
	</section>

	<section>
		<h2>2. Mixed widths and wrapping (check for jitter)</h2>
		<div class="narrow">
			<Tags bind:tags={widths} allowReorder onTagReordered={logReorder('widths')} />
		</div>
		<pre>{show(widths)}</pre>
	</section>

	<section>
		<h2>3. Duplicates (no <code>onlyUnique</code>)</h2>
		<p class="hint">The exact tag you drag should move, not the first one with the same text.</p>
		<Tags bind:tags={duplicates} allowReorder onTagReordered={logReorder('duplicates')} />
		<pre>{show(duplicates)}</pre>
	</section>

	<section>
		<h2>4. Object tags with autocomplete</h2>
		<Tags
			bind:tags={objects}
			allowReorder
			autoComplete={countries}
			autoCompleteKey="name"
			onlyUnique
			onTagReordered={logReorder('objects')}
			placeholder="Type a country..."
		/>
		<pre>{JSON.stringify(objects)}</pre>
	</section>

	<section>
		<h2>5. Together with <code>allowDrop</code></h2>
		<p class="hint">
			Dropping a tag on the input should reorder it, not add a copy. Dragging text from elsewhere on
			the page onto the input should still add tags: <span class="drag-text">try, dragging, this</span>
		</p>
		<Tags bind:tags={withDrop} allowReorder allowDrop onTagReordered={logReorder('drop')} />
		<pre>{show(withDrop)}</pre>
	</section>

	<section>
		<h2>6. Two instances</h2>
		<p class="hint">Dragging a tag from one box into the other should do nothing.</p>
		<div class="pair">
			<div>
				<Tags bind:tags={left} allowReorder allowDrop onTagReordered={logReorder('left')} />
				<pre>{show(left)}</pre>
			</div>
			<div>
				<Tags bind:tags={right} allowReorder allowDrop onTagReordered={logReorder('right')} />
				<pre>{show(right)}</pre>
			</div>
		</div>
	</section>

	<section>
		<h2>7. Toggles</h2>
		<div class="toggles">
			<label><input type="checkbox" bind:checked={allowReorder} /> allowReorder</label>
			<label><input type="checkbox" bind:checked={disable} /> disable</label>
			<label><input type="checkbox" bind:checked={readonly} /> readonly</label>
		</div>
		<Tags bind:tags={toggled} {allowReorder} {disable} {readonly} onTagReordered={logReorder('toggles')} />
		<pre>{show(toggled)}</pre>
	</section>

	<section>
		<h2><code>onTagReordered</code> log</h2>
		{#if log.length}
			<ol>
				{#each log as entry}<li>{entry}</li>{/each}
			</ol>
		{:else}
			<p class="hint">Nothing yet.</p>
		{/if}
	</section>
</main>

<style>
	main {
		max-width: 760px;
		margin: 2rem auto;
		padding: 0 16px;
		font-family: system-ui, sans-serif;
	}
	section {
		margin-bottom: 2rem;
	}
	h2 {
		font-size: 1.05rem;
		margin-bottom: 0.5rem;
	}
	.hint {
		color: #555;
		font-size: 0.9rem;
	}
	pre {
		background: #f4f4f4;
		padding: 6px 8px;
		font-size: 0.8rem;
		white-space: pre-wrap;
	}
	.narrow {
		max-width: 360px;
	}
	.pair {
		display: grid;
		grid-template-columns: 1fr 1fr;
		gap: 1rem;
	}
	.toggles {
		display: flex;
		gap: 1rem;
		margin-bottom: 0.5rem;
	}
	.drag-text {
		background: #fffbcc;
		padding: 0 4px;
	}
</style>
