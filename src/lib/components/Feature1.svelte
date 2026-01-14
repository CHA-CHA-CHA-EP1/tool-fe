<script>
	import { onMount } from 'svelte';

	export let pageName = '';
	export let pagePath = '';

	let data = null;
	let loading = false;
	let error = null;

	// ฟังก์ชันสำหรับเรียก API
	async function fetchData() {
		loading = true;
		error = null;

		try {
			// TODO: เปลี่ยน URL เป็น API endpoint จริง
			// const response = await fetch(`${pagePath}/api/feature1`);
			// const result = await response.json();
			// data = result;

			// ตัวอย่างข้อมูล (ลบออกเมื่อเชื่อม API จริง)
			await new Promise((resolve) => setTimeout(resolve, 500));
			data = {
				message: 'Feature 1 data loaded successfully',
				timestamp: new Date().toISOString()
			};
		} catch (err) {
			error = err.message;
		} finally {
			loading = false;
		}
	}

	// เรียก API เมื่อ component ถูก mount
	onMount(() => {
		fetchData();
	});

	// เรียก API ใหม่เมื่อ page เปลี่ยน
	$: if (pagePath) {
		fetchData();
	}
</script>

<div>
	<h3>Feature 1</h3>
	<p style="color: #666; font-size: 12px;">Environment: {pageName}</p>

	{#if loading}
		<div style="padding: 20px; text-align: center;">
			<p>Loading...</p>
		</div>
	{:else if error}
		<div style="padding: 20px; background: #ffcccc; border: 2px solid #cc0000;">
			<p style="color: #cc0000;">Error: {error}</p>
		</div>
	{:else if data}
		<div style="padding: 20px; background: #e8f5e9; border: 2px solid #4caf50; margin-top: 10px;">
			<h4>Data:</h4>
			<pre style="background: #fff; padding: 10px; border: 1px solid #ccc; overflow-x: auto;">{JSON.stringify(
					data,
					null,
					2
				)}</pre>
		</div>
	{/if}

	<div style="margin-top: 20px;">
		<button
			on:click={fetchData}
			style="
				padding: 10px 20px;
				background: #4caf50;
				border: 2px solid #000;
				color: #fff;
				font-weight: bold;
				cursor: pointer;
			"
		>
			Reload Data
		</button>
	</div>

	<div style="margin-top: 20px; padding: 15px; background: #fff3cd; border: 2px solid #ffc107;">
		<h4>API Configuration</h4>
		<p style="font-size: 12px;">Path: {pagePath}</p>
		<p style="font-size: 12px; color: #666;">
			TODO: Update API endpoint in fetchData() function
		</p>
	</div>
</div>
