<script>
	import { onMount } from 'svelte';

	export let pageName = '';
	export let pagePath = '';

	let data = null;
	let loading = false;
	let error = null;
	let inputValue = '';

	// ฟังก์ชันสำหรับเรียก API (GET)
	async function fetchData() {
		loading = true;
		error = null;

		try {
			// TODO: เปลี่ยน URL เป็น API endpoint จริง
			// const response = await fetch(`${pagePath}/api/feature2`);
			// const result = await response.json();
			// data = result;

			// ตัวอย่างข้อมูล (ลบออกเมื่อเชื่อม API จริง)
			await new Promise((resolve) => setTimeout(resolve, 500));
			data = {
				message: 'Feature 2 data loaded successfully',
				items: ['Item 1', 'Item 2', 'Item 3'],
				timestamp: new Date().toISOString()
			};
		} catch (err) {
			error = err.message;
		} finally {
			loading = false;
		}
	}

	// ฟังก์ชันสำหรับส่งข้อมูล (POST)
	async function submitData() {
		if (!inputValue.trim()) {
			alert('Please enter a value');
			return;
		}

		loading = true;
		error = null;

		try {
			// TODO: เปลี่ยน URL เป็น API endpoint จริง
			// const response = await fetch(`${pagePath}/api/feature2`, {
			//   method: 'POST',
			//   headers: { 'Content-Type': 'application/json' },
			//   body: JSON.stringify({ value: inputValue })
			// });
			// const result = await response.json();

			// ตัวอย่าง (ลบออกเมื่อเชื่อม API จริง)
			await new Promise((resolve) => setTimeout(resolve, 500));
			alert(`Submitted: ${inputValue}`);
			inputValue = '';
			await fetchData();
		} catch (err) {
			error = err.message;
		} finally {
			loading = false;
		}
	}

	onMount(() => {
		fetchData();
	});

	$: if (pagePath) {
		fetchData();
	}
</script>

<div>
	<h3>Feature 2</h3>
	<p style="color: #666; font-size: 12px;">Environment: {pageName}</p>

	<!-- Input Form -->
	<div style="margin-top: 20px; padding: 15px; background: #f0f0f0; border: 2px solid #000;">
		<h4>Submit Data</h4>
		<input
			type="text"
			bind:value={inputValue}
			placeholder="Enter value..."
			style="
				width: 100%;
				padding: 10px;
				border: 2px solid #000;
				font-size: 14px;
				margin-bottom: 10px;
			"
		/>
		<button
			on:click={submitData}
			disabled={loading}
			style="
				padding: 10px 20px;
				background: #2196f3;
				border: 2px solid #000;
				color: #fff;
				font-weight: bold;
				cursor: {loading ? 'not-allowed' : 'pointer'};
				opacity: {loading ? 0.6 : 1};
			"
		>
			{loading ? 'Submitting...' : 'Submit'}
		</button>
	</div>

	<!-- Data Display -->
	{#if loading}
		<div style="padding: 20px; text-align: center; margin-top: 20px;">
			<p>Loading...</p>
		</div>
	{:else if error}
		<div
			style="padding: 20px; background: #ffcccc; border: 2px solid #cc0000; margin-top: 20px;"
		>
			<p style="color: #cc0000;">Error: {error}</p>
		</div>
	{:else if data}
		<div style="padding: 20px; background: #e3f2fd; border: 2px solid #2196f3; margin-top: 20px;">
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
			disabled={loading}
			style="
				padding: 10px 20px;
				background: #4caf50;
				border: 2px solid #000;
				color: #fff;
				font-weight: bold;
				cursor: {loading ? 'not-allowed' : 'pointer'};
				opacity: {loading ? 0.6 : 1};
			"
		>
			Reload Data
		</button>
	</div>

	<div style="margin-top: 20px; padding: 15px; background: #fff3cd; border: 2px solid #ffc107;">
		<h4>API Configuration</h4>
		<p style="font-size: 12px;">Path: {pagePath}</p>
		<p style="font-size: 12px; color: #666;">
			TODO: Update API endpoints in fetchData() and submitData() functions
		</p>
	</div>
</div>
