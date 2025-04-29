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
	// import JSConfetti from 'js-confetti'
	const supabase = createClient(
		'https://lgztvgybalhvppkfpwdc.supabase.co',
		'eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJzdXBhYmFzZSIsInJlZiI6ImxnenR2Z3liYWxodnBwa2Zwd2RjIiwicm9sZSI6ImFub24iLCJpYXQiOjE3NDQzNDkwMTQsImV4cCI6MjA1OTkyNTAxNH0.JfB6J38LmdlvUwIgkdRmQcBDnv6OzFaA-D27S0ylVnA'
	);
	let data = $state([]);
	let error = $state(null);
	let loading = $state(true);

	onMount(async () => {
		const { data: fetchedData, error: fetchError } = await supabase.from('myangularsub7').select();
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

<h1>歡迎來到日期時間資訊管理</h1>
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
	<Table>
		<TableHead>
			<TableHeadCell>日期</TableHeadCell>
			<TableHeadCell>日期資訊</TableHeadCell>
			<TableHeadCell>倒數日期</TableHeadCell>
			<TableHeadCell>換算倒數日期</TableHeadCell>
		</TableHead>
		<TableBody tableBodyClass="divide-y">
			{#each data as item}
				{#key item.id}
					<TableBodyRow>
						<TableBodyCell>{item.date1}</TableBodyCell>
						<TableBodyCell>{item.date1info}</TableBodyCell>
						<TableBodyCell
							>倒數{Math.floor(
								(new Date(item.date1) - new Date()) / (1000 * 60 * 60 * 24)
							)}天</TableBodyCell
						>
						<TableBodyCell
							>或{(
								Math.floor((new Date(item.date1) - new Date()) / (1000 * 60 * 60 * 24)) / 7
							).toFixed(0)}週又{(
								Math.floor((new Date(item.date1) - new Date()) / (1000 * 60 * 60 * 24)) % 7
							).toFixed(0)}天</TableBodyCell
						>
					</TableBodyRow>
				{/key}
			{/each}
		</TableBody>
	</Table>
{/if}
