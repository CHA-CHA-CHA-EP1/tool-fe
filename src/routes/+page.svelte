<script>
	import MessageSender from '$lib/components/MessageSender.svelte';
	import DeleteApplication from '$lib/components/DeleteApplication.svelte';

	const pages = [
		{ name: 'DGL JAIPUMP UAT', path: '/dgl-gcp-sit' },
		{ name: 'DGL JAIPUMP SIT', path: '/dgl-gcp-uat' },
		{ name: 'DGL LOC UAT', path: '/dgl-aws-sit' },
		{ name: 'DGL LOC SIT', path: '/dgl-aws-uat' }
	];

	const features = [
		{ name: 'Clear Dynamic Reject', id: 'clear-dynamic-reject' },
		{ name: 'Delete LoanAppId', id: 'delete-application' }
	];

	let selectedPage = 0;
	let selectedFeature = 'clear-dynamic-reject';
</script>

<h1><i>WELCOME! <span style="color: #ebebeb">develop by น้องดุ๊กแห่ง DGL</span></i></h1>

<div class="flex gap-4" style="margin-bottom: 20px;">
	{#each pages as page, index}
		{#if index === 0}
			<button
				on:click={() => (selectedPage = index)}
				style="
					display: inline-block;
					padding: 15px 25px;
					background: {selectedPage === index ? '#00CED1' : '#00B0B3'};
					border: 3px solid #000;
					color: #000;
					font-weight: bold;
					font-size: 12px;
					text-decoration: none;
					box-shadow: 5px 5px 0px #000;
					cursor: pointer;
				"
			>
				{page.name}
			</button>
		{:else}
			<div
				style="
					display: inline-block;
					padding: 15px 25px;
					background: #C0C0C0;
					border: 3px solid #808080;
					color: #505050;
					font-weight: bold;
					font-size: 12px;
					cursor: not-allowed;
				"
			>
				{page.name}
			</div>
		{/if}
	{/each}
</div>

<div style="display: flex; gap: 20px;">
	<!-- Left Sidebar - Menu Bar -->
	<div
		style="
			width: 250px;
			min-width: 250px;
			flex-shrink: 0;
			padding: 20px;
			background: #F5F5F5;
			border: 3px solid #000;
			box-shadow: 5px 5px 0px #000;
		"
	>
		<h2
			style="margin-top: 0; margin-bottom: 20px; font-size: 16px; border-bottom: 2px solid #000; padding-bottom: 10px;"
		>
			Feature
		</h2>

		<div style="display: flex; flex-direction: column; gap: 10px;">
			{#each features as feature}
				<button
					on:click={() => (selectedFeature = feature.id)}
					style="
						padding: 12px 20px;
						background: {selectedFeature === feature.id ? '#FFD700' : '#FFF'};
						border: 2px solid #000;
						color: #000;
						font-weight: bold;
						font-size: 12px;
						cursor: pointer;
						text-align: left;
						box-shadow: {selectedFeature === feature.id ? '3px 3px 0px #000' : 'none'};
					"
				>
					{feature.name}
				</button>
			{/each}
		</div>
	</div>

	<!-- Right Content Area -->
	<div
		style="
			flex: 1;
			padding: 20px;
			border: 3px solid #000;
			background: #FFF;
			box-shadow: 5px 5px 0px #000;
      min-width: 300px;
		"
	>
		{#if selectedFeature === 'clear-dynamic-reject'}
			<MessageSender pageName={pages[selectedPage].name} pagePath={pages[selectedPage].path} />
		{:else if selectedFeature === 'delete-application'}
			<DeleteApplication pageName={pages[selectedPage].name} pagePath={pages[selectedPage].path} />
		{/if}
	</div>
</div>
