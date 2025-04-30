<script>
	import { createClient } from '@supabase/supabase-js';
	import { onMount } from 'svelte';
	// import JSConfetti from 'js-confetti'
	const supabase = createClient(
		'https://lgztvgybalhvppkfpwdc.supabase.co',
		'eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJzdXBhYmFzZSIsInJlZiI6ImxnenR2Z3liYWxodnBwa2Zwd2RjIiwicm9sZSI6ImFub24iLCJpYXQiOjE3NDQzNDkwMTQsImV4cCI6MjA1OTkyNTAxNH0.JfB6J38LmdlvUwIgkdRmQcBDnv6OzFaA-D27S0ylVnA'
	);
	let data = [];
	let error = null;
	let loading = true;
	let id = 0;
	let mysrc =
		'https://rncoursefeng37.expo.app/assets/assets/IMG_0032.52028bb69a561fde0f9857c864594271.jpg';

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
			// const jsConfetti = new JSConfetti();
			// jsConfetti.addConfetti();
			id = data[0].id;
			MostLeastBank();
			CurrentMonthMaxMin();
			YearlyMonthMaxMin();
			AllMonthMaxMin();
		}
	});
	let bank1 = $state(0);
	let bank2 = $state(0);
	let bank3 = $state(0);
	let bank4 = $state(0);
	let bank5 = $state(0);
	let bank6 = $state(0);
	let bank7 = $state(0);
	let bank8 = $state(0);
	let bank9 = $state(0);
	let bank10 = $state(0);
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
			// 每 1 秒執行一次
			const intervalId = setInterval(() => {
				console.log('每秒執行一次');
				isbanksave = true;
				banksavesec -= 1;
			}, 1000);

			// 在 5 秒後停止這個 interval
			setTimeout(() => {
				(async () => {
					clearInterval(intervalId);
					console.log('5 秒到了，停止執行');
					isbanksave = false;
					banksavesec = 5;
					MostLeastBank();
					CurrentMonthMaxMin();
					YearlyMonthMaxMin();
					AllMonthMaxMin();

					const { data, error } = await supabase.from('bankhistory').insert({
						user: supabase.auth.getUser(),
						bank1: bank1,
						bank2: bank2,
						bank3: bank3,
						bank4: bank4,
						bank5: bank5,
						bank6: bank6,
						bank7: bank7,
						bank8: bank8,
						bank9: bank9,
						bank10: bank10,
						banksum: sum,
						bankdate: new Date()
					});

					if (error) {
						console.error('儲存失敗:', error.message);
					} else {
						console.log('儲存成功:', data);
					}
				})();
			}, 5000);
		}
	}
	import { Button, Label } from 'flowbite-svelte';
	import { Alert } from 'flowbite-svelte';
	let ismoreinfo = $state(false);
	let isbankhistory = $state(false);
	let isbanksave = $state(false);
	let banksavesec = $state(5);
	let bankname = [
		'合作金庫',
		'台北富邦',
		'國泰世華',
		'兆豐銀行',
		'王道銀行',
		'新光銀行',
		'中華郵政',
		'玉山銀行',
		'台新銀行',
		'中國信託'
	];
	let banknamemore = [
		'(006)合作金庫(5880)',
		'(012)台北富邦(2881)',
		'(013)國泰世華(2882)',
		'(017)兆豐銀行(2886)',
		'(048)王道銀行(2897)',
		'(103)新光銀行(2888)',
		'(700)中華郵政(0000)',
		'(808)玉山銀行(2884)',
		'(812)台新銀行(2887)',
		'(822)中國信託(2891)'
	];
	let maxbankindex = $state(0);
	let minbankindex = $state(0);
	function MostLeastBank() {
		let banks = [bank1, bank2, bank3, bank4, bank5, bank6, bank7, bank8, bank9, bank10];
		let max = Math.max(...banks);
		let maxIndex = banks.indexOf(max);
		console.log('最大值:', max, '索引:', maxIndex);
		maxbankindex = maxIndex;
		let min = Math.min(...banks);
		let minIndex = banks.indexOf(min);
		console.log('最小值:', min, '索引:', minIndex);
		minbankindex = minIndex;
	}
	let currentmonthmax = $state(0);
	let currentmonthmin = $state(0);
	let yearmonthmaxmonth = $state(0);
	let yearmonthminmonth = $state(0);
	let yearmonthmax = $state(0);
	let yearmonthmin = $state(0);
	let allmonthmaxmonth = $state('');
	let allmonthminmonth = $state('');
	let allmonthmax = $state(0);
	let allmonthmin = $state(0);
	function toPostgresTimestamp(date) {
		// 移除毫秒，並補上正確時區
		return date.toISOString().split('.')[0] + '+00:00'; // "2025-04-01T00:00:00+00:00"
	}

	async function CurrentMonthMaxMin() {
		const now = new Date();
		const firstDayOfMonth = toPostgresTimestamp(new Date(now.getFullYear(), now.getMonth(), 1));
		const firstDayOfNextMonth = toPostgresTimestamp(
			new Date(now.getFullYear(), now.getMonth() + 1, 1)
		);

		const { data: maxData, error: maxError } = await supabase
			.from('bankhistory')
			.select('*')
			.gte('bankdate', firstDayOfMonth)
			.lt('bankdate', firstDayOfNextMonth)
			.order('bankdate', { ascending: false })
			.limit(1);

		const { data: minData, error: minError } = await supabase
			.from('bankhistory')
			.select('*')
			.gte('bankdate', firstDayOfMonth)
			.lt('bankdate', firstDayOfNextMonth)
			.order('bankdate', { ascending: true })
			.limit(1);

		console.log('起始:', firstDayOfMonth, '結束:', firstDayOfNextMonth);
		console.log('本月最大值:', maxData, '錯誤:', maxError);
		console.log('本月最小值:', minData, '錯誤:', minError);
		if (maxData.length>0)
			currentmonthmax = maxData[0].banksum;
		else
			currentmonthmax = '查無本月資料';
		if(minData.length>0)
			currentmonthmin = minData[0].banksum;
		else
			currentmonthmin = '查無本月資料';
	}

async function YearlyMonthMaxMin() {
    const now = new Date();
    const yearStart = new Date(now.getFullYear(), 0, 1); // 今年年初
    const yearEnd = new Date(now.getFullYear() + 1, 0, 1); // 明年年初，這樣可以排除跨年資料

    // 查詢今年所有的存款資料
    const { data, error } = await supabase
        .from('bankhistory')
        .select('bankdate, banksum')
        .gte('bankdate', yearStart.toISOString())
        .lt('bankdate', yearEnd.toISOString());

    if (error) {
        console.error('查詢錯誤:', error);
        return;
    }

    // 用 Map 分組，選取每月的最大和最小存款
    const monthMax = new Map();
    const monthMin = new Map();

    data.forEach(({ bankdate, banksum }) => {
        const date = new Date(bankdate);
        const yearMonth = `${date.getFullYear()}-${date.getMonth() + 1}`; // YYYY-MM 格式

        // 檢查是否已有該月的最大值和最小值，若無則初始化
        if (!monthMax.has(yearMonth)) {
            monthMax.set(yearMonth, banksum);
            monthMin.set(yearMonth, banksum);
        } else {
            // 比較最大值
            const currentMax = monthMax.get(yearMonth);
            if (banksum > currentMax) {
                monthMax.set(yearMonth, banksum);
            }

            // 比較最小值
            const currentMin = monthMin.get(yearMonth);
            if (banksum < currentMin) {
                monthMin.set(yearMonth, banksum);
            }
        }
    });

    // 找出最大和最小存款
    let maxMonth = null, minMonth = null;
    let maxSum = -Infinity, minSum = Infinity;

    monthMax.forEach((sum, month) => {
        if (sum > maxSum) {
            maxSum = sum;
            maxMonth = month;
        }
    });

    monthMin.forEach((sum, month) => {
        if (sum < minSum) {
            minSum = sum;
            minMonth = month;
        }
    });

    // 只取出月份數字（不需要年份部分）
    const maxMonthNumber = parseInt(maxMonth.split('-')[1], 10);
    const minMonthNumber = parseInt(minMonth.split('-')[1], 10);

    // 顯示結果
    console.log(`今年單月最多存款是：${maxMonthNumber} 月，金額: ${maxSum}`);
    console.log(`今年單月最少存款是：${minMonthNumber} 月，金額: ${minSum}`);
	yearmonthmaxmonth = maxMonthNumber;
	yearmonthminmonth = minMonthNumber;
	yearmonthmax = maxSum;
	yearmonthmin = minSum;
}

async function AllMonthMaxMin() {
    const { data, error } = await supabase
        .from('bankhistory')
        .select('bankdate, banksum');

    if (error) {
        console.error('查詢錯誤:', error);
        return;
    }

    // 用 Map 分組，選取每月的最大和最小存款
    const yearMonthMax = new Map();
    const yearMonthMin = new Map();

    data.forEach(({ bankdate, banksum }) => {
        const date = new Date(bankdate);
        const year = date.getFullYear();
        const month = date.getMonth() + 1; // 月份從 0 開始，所以加 1
        const yearMonth = `${year}-${month.toString().padStart(2, '0')}`; // 格式化為 yyyy-mm

        // 檢查是否已有該月的最大值和最小值，若無則初始化
        if (!yearMonthMax.has(yearMonth)) {
            yearMonthMax.set(yearMonth, banksum);
            yearMonthMin.set(yearMonth, banksum);
        } else {
            // 比較最大值
            const currentMax = yearMonthMax.get(yearMonth);
            if (banksum > currentMax) {
                yearMonthMax.set(yearMonth, banksum);
            }

            // 比較最小值
            const currentMin = yearMonthMin.get(yearMonth);
            if (banksum < currentMin) {
                yearMonthMin.set(yearMonth, banksum);
            }
        }
    });

    // 初始化最大最小存款
    let maxMonth = null, minMonth = null;
    let maxSum = -Infinity, minSum = Infinity;

    // 找出最大存款和最小存款的年月
    yearMonthMax.forEach((max, yearMonth) => {
        if (max > maxSum) {
            maxSum = max;
            maxMonth = yearMonth;
        }
    });

    yearMonthMin.forEach((min, yearMonth) => {
        if (min < minSum) {
            minSum = min;
            minMonth = yearMonth;
        }
    });

    // 顯示最大和最小存款年月
    const [maxYear, maxMonthNumber] = maxMonth.split('-');
    const [minYear, minMonthNumber] = minMonth.split('-');

    console.log(`最大年月: ${maxYear}年${maxMonthNumber}月, 最多存款: ${maxSum}`);
    console.log(`最小年月: ${minYear}年${minMonthNumber}月, 最少存款: ${minSum}`);
	allmonthmaxmonth = `${maxYear}-${maxMonthNumber}`;
	allmonthminmonth = `${minYear}-${minMonthNumber}`;
	allmonthmax = maxSum;
	allmonthmin = minSum;
}






</script>

{#if !ismoreinfo}
	<Button color="dark" onclick={() => (ismoreinfo = !ismoreinfo)}>更多資訊</Button>
{/if}
{#if ismoreinfo}
	<Button color="dark" onclick={() => (ismoreinfo = !ismoreinfo)}>更少資訊</Button>
{/if}
{#if !isbankhistory}
	<Button color="blue" onclick={() => (isbankhistory = !isbankhistory)}>展開銀行統計</Button>
{/if}
{#if isbankhistory}
	<Button color="blue" onclick={() => (isbankhistory = !isbankhistory)}>關閉銀行統計</Button>
	<Alert>
		<span class="font-medium">最多存款的銀行是：{bankname[maxbankindex]}</span>
	</Alert>
	<Alert color="blue">
		<span class="font-medium">最少存款的銀行是：{bankname[minbankindex]}</span>
	</Alert>
	<Alert color="red">
		<span class="font-medium">本月最多存款是：{currentmonthmax}</span>
	</Alert>
	<Alert color="green">
		<span class="font-medium">本月最少存款是：{currentmonthmin}</span>
	</Alert>
	<Alert color="yellow">
		<span class="font-medium">今年單月最多存款是：{yearmonthmaxmonth}月，{yearmonthmax}</span>
	</Alert>
	<Alert color="dark">
		<span class="font-medium">今年單月最少存款是：{yearmonthminmonth}月，{yearmonthmin}</span>
	</Alert>
	<Alert color="yellow">
		<span class="font-medium">歷史單月最多存款是：{allmonthmaxmonth}，{allmonthmax}</span>
	</Alert>
	<Alert color="dark">
		<span class="font-medium">歷史單月最少存款是：{allmonthminmonth}，{allmonthmin}</span>
	</Alert>
{/if}
{#if isbanksave}
	<Alert>
		<span class="font-medium">銀行資料已存檔，{banksavesec}秒之後此訊息隱藏。</span>
	</Alert>
{/if}
<h1>歡迎來到銀行資訊管理</h1>
{#if !ismoreinfo}
	<p>
		{bankname[0]}：<input type="number" bind:value={bank1} style="width: 100px;" />
		{bankname[1]}：<input type="number" bind:value={bank2} style="width: 100px;" />
	</p>
	<p>
		{bankname[2]}：<input type="number" bind:value={bank3} style="width: 100px;" />
		{bankname[3]}：<input type="number" bind:value={bank4} style="width: 100px;" />
	</p>
	<p>
		{bankname[4]}：<input type="number" bind:value={bank5} style="width: 100px;" />
		{bankname[5]}：<input type="number" bind:value={bank6} style="width: 100px;" />
	</p>
	<p>
		{bankname[6]}：<input type="number" bind:value={bank7} style="width: 100px;" />
		{bankname[7]}：<input type="number" bind:value={bank8} style="width: 100px;" />
	</p>
	<p>
		{bankname[8]}：<input type="number" bind:value={bank9} style="width: 100px;" />
		{bankname[9]}：<input type="number" bind:value={bank10} style="width: 100px;" />
	</p>
	<p>
		累積存款：<input type="number" value={sum} readonly style="width: 100px;" />
		<Button onclick={onSaveSaving}>儲存</Button>
	</p>
{/if}
{#if ismoreinfo}
	<img src={mysrc} alt="cat" width="100" />
	<p>
		{banknamemore[0]}：<input type="number" bind:value={bank1} style="width: 100px;" />
		{banknamemore[1]}：<input type="number" bind:value={bank2} style="width: 100px;" />
	</p>
	<p>
		{banknamemore[2]}：<input type="number" bind:value={bank3} style="width: 100px;" />
		{banknamemore[3]}：<input type="number" bind:value={bank4} style="width: 100px;" />
	</p>
	<p>
		{banknamemore[4]}：<input type="number" bind:value={bank5} style="width: 100px;" />
		{banknamemore[5]}：<input type="number" bind:value={bank6} style="width: 100px;" />
	</p>
	<p>
		{banknamemore[6]}：<input type="number" bind:value={bank7} style="width: 100px;" />
		{banknamemore[7]}：<input type="number" bind:value={bank8} style="width: 100px;" />
	</p>
	<p>
		{banknamemore[8]}：<input type="number" bind:value={bank9} style="width: 100px;" />
		{banknamemore[9]}：<input type="number" bind:value={bank10} style="width: 100px;" />
	</p>
	<p>
		(000)累積存款(0000)：<input type="number" value={sum} readonly />
		<Button onclick={onSaveSaving}>儲存</Button>
	</p>
{/if}
