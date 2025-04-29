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
	import { Label, Input,Button } from 'flowbite-svelte';
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
		<Table>
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
						</TableBodyRow>
					{/key}
				{/each}
			</TableBody>
		</Table>
	{/if}
	{#if data.length > 0}
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
							</TableBodyRow>
						{/key}
					{/each}
				</TableBody>
			</Table>
		{/if}
		<p>Data loaded successfully!</p>
	{/if}
{/if}

<div class="mb-6">
	<Label for="large-input" class="block mb-2">{textareavaluetitle1}：</Label>
	<Input id="large-input" size="lg" placeholder="" />
  </div>
  <div class="mb-6">
	<Label for="default-input" class="block mb-2">{textareavaluetitle2}：</Label>
	<Input id="default-input" placeholder="" />
  </div>
  <div class="mb-6">
	<Label for="small-input" class="block mb-2">{textareavaluetitle3}：</Label>
	<Input id="small-input" size="sm" placeholder="" />
  </div>
  {#if myangularsubtitle !== 'myangularsub5'}
  <div class="mb-6">
	<Label for="small-input" class="block mb-2">{textareavaluetitle4}：</Label>
	<textarea id="small-input" size="sm" placeholder="" />
  </div>
  {/if}
  {#if myangularsubtitle === 'myangularsub5'}
  <div class="mb-6">
	<Label for="small-input" class="block mb-2">{textareavaluetitle4}：</Label>
	<Input id="small-input" size="sm" placeholder="" />
  </div>
  <div class="mb-6">
	<Label for="small-input" class="block mb-2">{textareavaluetitle5}：</Label>
	<Input id="small-input" size="sm" placeholder="" />
  </div>
  {/if}
  <Button color="dark">新增資料</Button>
