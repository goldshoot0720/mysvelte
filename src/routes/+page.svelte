<script>
	import Sub from './sub.svelte';
	import Sub2 from './sub2.svelte';
	import { Button, ButtonGroup, GradientButton } from 'flowbite-svelte';
	let showSub1 = false;
	let showSub2 = false;
	let showSub4 = false;
	let showSub5 = false;
	let showSub6 = false;
	let showSub7 = false;
	let showSub9 = false;
	function LoadSub1() {
		if (showSub1) {
			showSub1 = false;
		} else {
			showSub1 = true;
			showSub2 = false;
			showSub4 = false;
			showSub5 = false;
			showSub6 = false;
			showSub7 = false;
			showSub9 = false;
		}
	}
	function LoadSub2() {
		if (showSub2) {
			showSub2 = false;
		} else {
			showSub2 = true;
			showSub1 = false;
			showSub4 = false;
			showSub5 = false;
			showSub6 = false;
			showSub7 = false;
			showSub9 = false;
		}
	}
	function LoadSub4() {
		if (showSub4) {
			showSub4 = false;
		} else {
			showSub4 = true;
			showSub1 = false;
			showSub2 = false;
			showSub5 = false;
			showSub6 = false;
			showSub7 = false;
			showSub9 = false;
		}
	}
	function LoadSub5() {
		if (showSub5) {
			showSub5 = false;
		} else {
			showSub5 = true;
			showSub1 = false;
			showSub2 = false;
			showSub4 = false;
			showSub6 = false;
			showSub7 = false;
			showSub9 = false;
		}
	}
	function LoadSub6() {
		if (showSub6) {
			showSub6 = false;
		} else {
			showSub6 = true;
			showSub1 = false;
			showSub2 = false;
			showSub4 = false;
			showSub5 = false;
			showSub7 = false;
			showSub9 = false;
		}
	}
	function LoadSub7() {
		if (showSub7) {
			showSub7 = false;
		} else {
			showSub7 = true;
			showSub1 = false;
			showSub2 = false;
			showSub4 = false;
			showSub5 = false;
			showSub6 = false;
			showSub9 = false;
		}
	}
	function LoadSub9() {
		if (showSub9) {
			showSub9 = false;
		} else {
			showSub9 = true;
			showSub1 = false;
			showSub2 = false;
			showSub4 = false;
			showSub5 = false;
			showSub6 = false;
			showSub7 = false;
		}
	}
</script>

<svelte:head>
	<title>草包鋒兄 Svelte</title>
</svelte:head>
<div class="text-gray-900 dark:text-gray-100">
	<div class="py-4">
		<ButtonGroup>
			<Button color="red" onclick={LoadSub1}>訂閱資訊管理</Button>
			<Button color="green" onclick={LoadSub2}>銀行資訊管理</Button>
			<Button color="yellow">錄取資訊管理</Button>
		</ButtonGroup>
	</div>
	<div class="py-4">
		<ButtonGroup>
			<Button color="red" onclick={LoadSub4}>學習資訊管理</Button>
			<Button color="green" onclick={LoadSub5}>門診處方箋資訊管理</Button>
			<Button color="yellow" onclick={LoadSub6}>消費資訊管理</Button>
		</ButtonGroup>
	</div>
	<div class="py-4">
		<ButtonGroup>
			<Button color="dark" onclick={LoadSub7}>日期時間資訊管理</Button>
			<Button color="blue">金融市場資訊管理</Button>
			<Button color="none" onclick={LoadSub9}>動畫追劇資訊管理</Button>
		</ButtonGroup>
	</div>
	<div class="py-4">
		<ButtonGroup>
			<Button color="dark">部落格資訊管理</Button>
			<Button color="blue">大位天定資訊管理</Button>
			<Button color="none">保留天定資訊管理</Button>
		</ButtonGroup>
	</div>
</div>

{#if showSub1}
	<Sub
		subtitle="訂閱"
		textareavaluetitle1="訂閱項目"
		textareavaluetitle2="訂閱費用"
		textareavaluetitle3="訂閱到期日"
		textareavaluetitle4="備註"
		myangularsubtitle="myangularsub1"
	/>
{/if}
{#if showSub2}
	<Sub2 />
{/if}
{#if showSub4}
	<Sub
		subtitle="學習"
		textareavaluetitle1="學習項目"
		textareavaluetitle2="學習進度"
		textareavaluetitle3="最近學習日"
		textareavaluetitle4="備註"
		myangularsubtitle="myangularsub4"
	/>
{/if}
{#if showSub5}
	<Sub
		subtitle="門診處方箋"
		textareavaluetitle1="醫院/醫師"
		textareavaluetitle2="處方箋一"
		textareavaluetitle3="處方箋二"
		textareavaluetitle4="回診日期"
		myangularsubtitle="myangularsub5"
	/>
{/if}
{#if showSub6}
	<Sub
		subtitle="消費"
		textareavaluetitle1="點數項目"
		textareavaluetitle2="剩餘點數"
		textareavaluetitle3="點數到期日"
		textareavaluetitle4="備註"
		myangularsubtitle="myangularsub6"
	/>
{/if}
{#if showSub7}
	<Sub
		subtitle="日期時間"
		textareavaluetitle1="日期"
		textareavaluetitle2="日期資訊"
		textareavaluetitle3="手動倒數日期"
		textareavaluetitle4="手動換算倒數日期"
		myangularsubtitle="myangularsub7"
	/>
{/if}
{#if showSub9}
	<Sub
		subtitle="動畫追劇"
		textareavaluetitle1="動畫追劇項目"
		textareavaluetitle2="當前集數"
		textareavaluetitle3="最近觀看日"
		textareavaluetitle4="備註"
		myangularsubtitle="myangularsub9"
	/>
{/if}
