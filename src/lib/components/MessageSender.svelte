<script>
	export let pageName = '';
	export let pagePath = '';

	let inputMessage = '';
	let loading = false;
	let response = null;
	let error = null;

	// ฟังก์ชันสำหรับส่ง Request
	async function sendRequest() {
		if (!inputMessage.trim()) {
			alert('กรุณากรอกข้อมูล');
			return;
		}

		loading = true;
		error = null;
		response = null;

		try {
			const apiUrl = 'https://cha14.xyz/api/message';

			// สร้าง message ตามรูปแบบที่กำหนด
			const messageText = `aws-gcp-uat-clear-dynamic-reject ${inputMessage}`;

			const res = await fetch(apiUrl, {
				method: 'POST',
				headers: {
					'Content-Type': 'application/json'
				},
				body: JSON.stringify({
					agent: 'dgl-gcp-uat',
					message: messageText
				})
			});

			if (!res.ok) {
				throw new Error(`HTTP error! status: ${res.status}`);
			}

			// ลอง parse เป็น text ก่อน
			const responseText = await res.text();
			console.log('Raw response:', responseText);

			// พยายาม parse เป็น JSON
			let data;
			try {
				data = JSON.parse(responseText);
			} catch (e) {
				// ถ้า parse JSON ไม่ได้ ให้ใช้ text ที่ได้มา
				data = { message: responseText };
			}

			response = {
				success: true,
				statusCode: res.status,
				data: data,
				rawResponse: responseText,
				timestamp: new Date().toISOString()
			};
		} catch (err) {
			error = err.message || 'เกิดข้อผิดพลาดในการส่ง Request';
			console.error('Error sending request:', err);
		} finally {
			loading = false;
		}
	}

	// ฟังก์ชันสำหรับ clear form
	function clearForm() {
		inputMessage = '';
		response = null;
		error = null;
	}
</script>

<div>
	<h3>Clear Dynamic Reject</h3>
	<p style="color: #666; font-size: 12px; margin-bottom: 20px;">
		Environment: {pageName}
	</p>

	<!-- คำเตือน -->
	<div style="margin-bottom: 15px; padding: 10px; background: #ffebee; border: 2px solid #d32f2f;">
		<p style="color: #c62828; margin: 0; font-size: 11px; line-height: 1.5;">
			⚠️ Feature นี้ถ้าทดลองยิงแล้วยังไม่สามารถเข้าได้ อาจเกิดจากติด Dynamic Reject ที่
			CMLOS
		</p>
	</div>

	<!-- Input Form -->
	<div style="margin-bottom: 20px; padding: 15px; background: #f5f5f5; border: 2px solid #000;">
		<h4 style="margin-top: 0; margin-bottom: 10px; font-size: 14px;">กรอกข้อมูล</h4>
		<label
			for="message-input"
			style="display: block; margin-bottom: 5px; font-size: 12px; font-weight: bold;"
		>
			Message:
		</label>
		<input
			id="message-input"
			type="text"
			bind:value={inputMessage}
			placeholder="เช่น LNAPP หรือ LNAPP,LNAPP2"
			disabled={loading}
			style="
				width: 100%;
				padding: 10px;
				border: 2px solid #000;
				font-size: 14px;
				margin-bottom: 12px;
				background: {loading ? '#eee' : '#fff'};
			"
		/>

		<div style="display: flex; gap: 10px;">
			<button
				on:click={sendRequest}
				disabled={loading}
				style="
					padding: 12px 24px;
					background: {loading ? '#ccc' : '#4caf50'};
					border: 3px solid #000;
					color: #fff;
					font-weight: bold;
					font-size: 14px;
					cursor: {loading ? 'not-allowed' : 'pointer'};
					box-shadow: {loading ? 'none' : '3px 3px 0px #000'};
				"
			>
				{loading ? 'Sending...' : 'Send Request'}
			</button>

			<button
				on:click={clearForm}
				disabled={loading}
				style="
					padding: 12px 24px;
					background: #ff9800;
					border: 3px solid #000;
					color: #fff;
					font-weight: bold;
					font-size: 14px;
					cursor: {loading ? 'not-allowed' : 'pointer'};
					box-shadow: 3px 3px 0px #000;
					opacity: {loading ? 0.5 : 1};
				"
			>
				Clear
			</button>
		</div>
	</div>

	<!-- Response Display -->
	{#if error}
		<div style="margin-top: 15px; padding: 12px; background: #f5f5f5; border: 2px solid #000;">
			<h4 style="margin-top: 0; margin-bottom: 8px; font-size: 12px;">Error:</h4>
			<p style="margin: 0; font-size: 12px; color: #333;">{error}</p>
		</div>
	{/if}

	{#if response}
		<div style="margin-top: 15px; padding: 12px; background: #f5f5f5; border: 2px solid #000;">
			<h4 style="margin-top: 0; margin-bottom: 8px; font-size: 12px;">Response:</h4>
			<pre
				style="background: #fff; padding: 10px; border: 1px solid #ccc; overflow-x: auto; font-size: 11px; margin: 0; white-space: pre-wrap; word-wrap: break-word;">{response.rawResponse || JSON.stringify(response, null, 2)}</pre>
		</div>
	{/if}
</div>
