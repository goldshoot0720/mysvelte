<script>
	import { createClient } from '@supabase/supabase-js';
	import { onMount } from 'svelte';
	import {
		Table,
		TableBody,
		TableBodyCell,
		TableBodyRow,
		TableHead,
		TableHeadCell,
		Checkbox,
		TableSearch
	} from 'flowbite-svelte';
	import { Label, Input, Button } from 'flowbite-svelte';
	// import JSConfetti from 'js-confetti'
	const {
		subtitle,
		textareavaluetitle1,
		textareavaluetitle2,
		textareavaluetitle3,
		textareavaluetitle4,
		textareavaluetitle5,
		myangularsubtitle
	} = $props();
	const supabase = createClient(
		'https://lgztvgybalhvppkfpwdc.supabase.co',
		'eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJzdXBhYmFzZSIsInJlZiI6ImxnenR2Z3liYWxodnBwa2Zwd2RjIiwicm9sZSI6ImFub24iLCJpYXQiOjE3NDQzNDkwMTQsImV4cCI6MjA1OTkyNTAxNH0.JfB6J38LmdlvUwIgkdRmQcBDnv6OzFaA-D27S0ylVnA'
	);
	let data = $state([]);
	let error = $state(null);
	let loading = $state(true);
	let id = $state(null);
	let textareavalue1 = $state('');
	let textareavalue2 = $state('');
	let textareavalue3 = $state('');
	let textareavalue4 = $state('');
	let textareavalue5 = $state('');

	onMount(async () => {
		const { data: fetchedData, error: fetchError } = await supabase
			.from(myangularsubtitle)
			.select();
		loading = false;

		if (fetchError) {
			error = fetchError;
			console.error('Error:', fetchError.message);
		} else {
			data = fetchedData;
			console.log('Data fetched successfully:', data);
			// const jsConfetti = new JSConfetti();
			// jsConfetti.addConfetti();
		}
	});

	async function onReLoad() {
		const { data: fetchedData, error: fetchError } = await supabase
			.from(myangularsubtitle)
			.select();
		loading = false;

		if (fetchError) {
			error = fetchError;
			console.error('Error:', fetchError.message);
		} else {
			data = fetchedData;
			console.log('Data fetched successfully:', data);
			// const jsConfetti = new JSConfetti();
			// jsConfetti.addConfetti();
			textareavalue1 = '';
			textareavalue2 = '';
			textareavalue3 = '';
			textareavalue4 = '';
			if (myangularsubtitle === 'myangularsub5') {
				textareavalue5 = '';
			}
		}
	}

	function onNewDataClick(textareavalue1, textareavalue2, textareavalue3, textareavalue4) {
		// Validate input fields
		if (textareavalue1 === '' || textareavalue2 === '' || textareavalue3 === '') {
			alert(
				textareavaluetitle1 +
					'不得為空值\n' +
					textareavaluetitle2 +
					'不得為空值\n' +
					textareavaluetitle3 +
					'不得為空值'
			);
			return;
		}

		// Insert data into Supabase
		if (confirm('確定要新增這筆資料嗎？')) {
			supabase
				.from(myangularsubtitle) // Ensure the table name is a string
				.insert([
					{
						textareavalue1: textareavalue1,
						textareavalue2: textareavalue2,
						textareavalue3: textareavalue3,
						textareavalue4: textareavalue4
					}
				])
				.then(({ data, error }) => {
					if (error) {
						console.error('Error inserting data:', error.message);
					} else {
						console.log('Data inserted successfully:', data);
						onReLoad(); // Reload data after deletion
					}
				});
		}
	}

	function onDeleteDataClick(id) {
		if (confirm('確定要刪除這筆資料嗎？')) {
			supabase
				.from(myangularsubtitle) // Ensure the table name is a string
				.delete()
				.eq('id', id)
				.then(({ data, error }) => {
					if (error) {
						console.error('Error deleting data:', error.message);
					} else {
						console.log('Data deleted successfully:', data);
						onReLoad(); // Reload data after deletion
					}
				});
		}
	}

	function onUpdateDataClick(data) {
		// Validate input fields
		if (textareavalue1 === '' || textareavalue2 === '' || textareavalue3 === '') {
			alert(
				textareavaluetitle1 +
					'不得為空值\n' +
					textareavaluetitle2 +
					'不得為空值\n' +
					textareavaluetitle3 +
					'不得為空值'
			);
			return;
		}
		if (confirm('確定要修改這筆資料嗎？')) {
			supabase
				.from(myangularsubtitle) // Ensure the table name is a string
				.update({
					textareavalue1: textareavalue1,
					textareavalue2: textareavalue2,
					textareavalue3: textareavalue3,
					textareavalue4: textareavalue4
				})
				.eq('id', data.id)
				.then(({ data, error }) => {
					if (error) {
						console.error('Error updating data:', error.message);
					} else {
						console.log('Data updated successfully:', data);
						onReLoad(); // Reload data after deletion
					}
				});
		}
	}

	function onSelectDataClick(item) {
		id = item.id;
		textareavalue1 = item.textareavalue1;
		textareavalue2 = item.textareavalue2;
		textareavalue3 = item.textareavalue3;
		textareavalue4 = item.textareavalue4;
		if (myangularsubtitle === 'myangularsub5') {
			textareavalue5 = item.textareavalue5;
		}
	}

	function onEmptyDataClick() {
		textareavalue1 = '';
		textareavalue2 = '';
		textareavalue3 = '';
		textareavalue4 = '';
		if (myangularsubtitle === 'myangularsub5') {
			textareavalue5 = '';
		}
	}
</script>

<h1>歡迎來到{subtitle}資訊管理</h1>
{#if loading}
	<p>Loading...</p>
{/if}
{#if error}
	<p>Error: {error.message}</p>
{/if}
{#if data.length === 0}
	<p>No data found.</p>
{/if}

{#if data.length > 0}
{#if myangularsubtitle !== 'myangularsub5'}
<Table hoverable={true}>
	<TableHead>
		<TableHeadCell>{textareavaluetitle1}</TableHeadCell>
		<TableHeadCell>{textareavaluetitle2}</TableHeadCell>
		<TableHeadCell>{textareavaluetitle3}</TableHeadCell>
		<TableHeadCell>{textareavaluetitle4}</TableHeadCell>
	</TableHead>
	<TableBody tableBodyClass="divide-y">
		{#each data as item}
			{#key item.id}
				<TableBodyRow>
					<TableBodyCell>{item.textareavalue1}</TableBodyCell>
					<TableBodyCell>{item.textareavalue2}</TableBodyCell>
					<TableBodyCell>{item.textareavalue3}</TableBodyCell>
					<TableBodyCell>{item.textareavalue4}</TableBodyCell>
					<Button color="none" onclick={() => onSelectDataClick(item)}>選取</Button>
					<Button color="dark" onclick={() => onUpdateDataClick(item)}>修改</Button>
					<Button color="blue" onclick={() => onDeleteDataClick(item.id)}>刪除</Button>
				</TableBodyRow>
			{/key}
		{/each}
	</TableBody>
</Table>
	{/if}

	{#if myangularsubtitle === 'myangularsub5'}
			<Table>
				<TableHead>
					<TableHeadCell>{textareavaluetitle1}</TableHeadCell>
					<TableHeadCell>{textareavaluetitle2}</TableHeadCell>
					<TableHeadCell>{textareavaluetitle3}</TableHeadCell>
					<TableHeadCell>{textareavaluetitle4}</TableHeadCell>
					<TableHeadCell>{textareavaluetitle5}</TableHeadCell>
				</TableHead>
				<TableBody tableBodyClass="divide-y">
					{#each data as item}
						{#key item.id}
							<TableBodyRow>
								<TableBodyCell>{item.hospital}</TableBodyCell>
								<TableBodyCell>{item.doctor}</TableBodyCell>
								<TableBodyCell>{item.prescription1}</TableBodyCell>
								<TableBodyCell>{item.prescription2}</TableBodyCell>
								<TableBodyCell>{item.prescription3}</TableBodyCell>
								<Button color="none" onclick={() => onSelectDataClick(item)}>選取</Button>
								<Button color="dark" onclick={() => onUpdateDataClick(item.id)}>修改</Button>
								<Button color="blue" onclick={() => onDeleteDataClick(item.id)}>刪除</Button>
							</TableBodyRow>
						{/key}
					{/each}
				</TableBody>
			</Table>
		<p>Data loaded successfully!</p>
	{/if}
  
{/if}

<div class="mb-6">
	<Label for="large-input" class="mb-2 block">{textareavaluetitle1}：</Label>
	<Input id="large-input" size="lg" placeholder="" bind:value={textareavalue1} />
</div>
<div class="mb-6">
	<Label for="default-input" class="mb-2 block">{textareavaluetitle2}：</Label>
	<Input id="default-input" placeholder="" bind:value={textareavalue2} />
</div>
<div class="mb-6">
	<Label for="small-input" class="mb-2 block">{textareavaluetitle3}：</Label>
	<Input id="small-input" size="sm" placeholder="" bind:value={textareavalue3} />
</div>
{#if myangularsubtitle !== 'myangularsub5'}
	<div class="mb-6">
		<Label for="small-input" class="mb-2 block">{textareavaluetitle4}：</Label>
		<textarea id="small-input" size="sm" placeholder="" bind:value={textareavalue4}></textarea>
	</div>
{/if}
{#if myangularsubtitle === 'myangularsub5'}
	<div class="mb-6">
		<Label for="small-input" class="mb-2 block">{textareavaluetitle4}：</Label>
		<Input id="small-input" size="sm" placeholder="" bind:value={textareavalue4} />
	</div>
	<div class="mb-6">
		<Label for="small-input" class="mb-2 block">{textareavaluetitle5}：</Label>
		<Input id="small-input" size="sm" placeholder="" bind:value={textareavalue5} />
	</div>
{/if}
{#if myangularsubtitle !== 'myangularsub5'}
	<Button color="none" onclick={onEmptyDataClick}>清空</Button>
	<Button
		color="dark"
		onclick={() => onNewDataClick(textareavalue1, textareavalue2, textareavalue3, textareavalue4)}
		>新增</Button
	>
{/if}
{#if myangularsubtitle === 'myangularsub5'}
	<Button color="none" onclick={onEmptyDataClick}>清空</Button>
	<Button
		color="dark"
		onclick={() =>
			onNewDataClick(
				textareavalue1,
				textareavalue2,
				textareavalue3,
				textareavalue4,
				textareavalue5
			)}>新增</Button
	>
{/if}
