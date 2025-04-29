<script>
	import { createClient } from '@supabase/supabase-js';
	import { onMount } from 'svelte';
	import JSConfetti from 'js-confetti'
	const supabase = createClient('https://lgztvgybalhvppkfpwdc.supabase.co', 'eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJzdXBhYmFzZSIsInJlZiI6ImxnenR2Z3liYWxodnBwa2Zwd2RjIiwicm9sZSI6ImFub24iLCJpYXQiOjE3NDQzNDkwMTQsImV4cCI6MjA1OTkyNTAxNH0.JfB6J38LmdlvUwIgkdRmQcBDnv6OzFaA-D27S0ylVnA')
	let data = [];
	let error = null;
	let loading = true;
	let id = 0;

	onMount(async () => {
		const { data: fetchedData, error: fetchError } = await supabase.from('myangularsub2').select();
		loading = false;

		if (fetchError) {
			error = fetchError;
			console.error('Error:', fetchError.message);
		} else {
			data = fetchedData;
			console.log('Data fetched successfully:', data);
			bank1 = data[0].bank1;
			bank2 = data[0].bank2;
			bank3 = data[0].bank3;
			bank4 = data[0].bank4;
			bank5 = data[0].bank5;
			bank6 = data[0].bank6;
			bank7 = data[0].bank7;
			bank8 = data[0].bank8;
			bank9 = data[0].bank9;
			bank10 = data[0].bank10;
			const jsConfetti = new JSConfetti();
			jsConfetti.addConfetti();
			id = data[0].id;
		}
	});
	let bank1 = $state(100);
	let bank2 = $state(200);
	let bank3 = $state(300);
	let bank4 = $state(400);
	let bank5 = $state(500);
	let bank6 = $state(600);
	let bank7 = $state(700);
	let bank8 = $state(800);
	let bank9 = $state(900);
	let bank10 = $state(1000);
	let sum = $derived(
		bank1 + bank2 + bank3 + bank4 + bank5 + bank6 + bank7 + bank8 + bank9 + bank10
	);

	async function onSaveSaving() {
		const { data, error } = await supabase
			.from('myangularsub2')
			.update({
				bank1: bank1,
				bank2: bank2,
				bank3: bank3,
				bank4: bank4,
				bank5: bank5,
				bank6: bank6,
				bank7: bank7,
				bank8: bank8,
				bank9: bank9,
				bank10: bank10
			})
			.eq('id', id);

		if (error) {
			console.error('Error updating data:', error);
		} else {
			console.log('Data updated successfully:', data);
			alert('Data updated successfully:', data);
		}
	};
	import { Button } from 'flowbite-svelte';
</script>

<p>
	(006)合作金庫(5880)：<input type="number" bind:value={bank1} />
	(012)台北富邦(2881)：<input type="number" bind:value={bank2} />
</p>
<p>
	(013)國泰世華(2882)：<input type="number" bind:value={bank3} />
	(017)兆豐銀行(2886)：<input type="number" bind:value={bank4} />
</p>
<p>
	(048)王道銀行(2897)：<input type="number" bind:value={bank5} />
	(103)新光銀行(2888)：<input type="number" bind:value={bank6} />
</p>
<p>
	(700)中華郵政(0000)：<input type="number" bind:value={bank7} />
	(808)玉山銀行(2884)：<input type="number" bind:value={bank8} />
</p>
<p>
	(812)台新銀行(2887)：<input type="number" bind:value={bank9} />
	(822)中國信託(2891)：<input type="number" bind:value={bank10} />
</p>
<p>
	(000)累積存款(0000)：<input type="number" bind:value={sum} />
	<Button onclick={onSaveSaving}>儲存</Button>
</p>
