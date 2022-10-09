<script>
		import axios from 'axios';
		import { onMount } from 'svelte';
		import { info } from '../store.js';
		import Card from '../lib/Card.svelte';

		let cryptos = [];
		let filteredOnes = [];
		let blue = 0;
		let page = 1;

		const loadCryptos = async() => {
			let { data } = await axios.get(`https://api.coingecko.com/api/v3/coins/markets?vs_currency=usd&order=market_cap_desc&per_page=10&page=${page}&sparkline=false`);
			cryptos = data;
			filteredOnes = data;
		}

		onMount(async () => {
			await loadCryptos();
			let { data: blueValue } = await axios.get("https://api.bluelytics.com.ar/v2/latest");
			blue = blueValue.blue.value_buy
		});

		let show = true;

		const toggleTable = () => {
			let table = document.querySelector("table");

			if (show) {
				table.style.transition = "all .5s ease-in";
				table.style.transform = "translateX(-1000px)";

				setTimeout(() => {
					show = false
					table.style.position = "absolute";
				}, 400);
			} else {
				show = true;
				table.style.position = "";
				table.style.transform = "";
			}
		};

		const handleCoinClick = (e) => {
			let selected = e.target.getAttribute("key");
			$info = cryptos.find((x) => x.id === selected);
		}

		const handleBack = async() => {
			page--
			loadCryptos();
		};

		const handleNext = async () => {
			page++
			loadCryptos();
		}

		const handleReset = () => {
			page = 0;
			loadCryptos();
		}


</script>

<main class="w-full min-h-screen bg-zinc-800 flex flex-wrap">
	<section name="cryptos" class="w-1/2 p-5 flex flex-col items-start">
		<div class="flex">
			<div class="flex flex-col">
				<table class="bg-zinc-900 w-full" class:hidden={!show}>
			<tr class="text-left text-cyan-300 p-3">
				<th class="p-2">Top</th>
				<th class="p-2">Nombre</th>
				<th class="p-2">Símbolo</th>
				<th class="p-2">Precio (USD)</th>
				<th class="p-2">Últimas 24h</th>
			</tr>
			{#each cryptos as coin, index}
				<tr class="hover:bg-neutral-900 duration-100 bg-zinc-800 rounded-lg cursor-pointer" on:click={handleCoinClick}>
					<td key={coin.id} class="text-neutral-500 p-2">{page == 1 ? index+1 : (index + 1)+page*10}</td> /* POR FAVOR IGNORAR ESTA LÍNEA ME DIO PAJA ARREGLARLO */
					<td key={coin.id} class="text-cyan-300 font-bold p-2 flex items-center"><img src={coin.image} style="width: 2rem" class="mr-2 rounded-full"> {coin.name}</td>
					<td key={coin.id} class="text-neutral-500 p-2">{coin.symbol.toUpperCase()}</td>
					<td key={coin.id} class="text-zinc-400 p-2">${coin.current_price}</td>
					<td key={coin.id} class={`p-2 ${coin.price_change_percentage_24h < 0 ? "text-red-600" : "text-emerald-500"}`}>{coin.price_change_percentage_24h}%</td>
				</tr>
				{:else}
			<p class="font-bold text-3xl uppercase p-2 text-cyan-300 text-center">Loading...</p>
			{/each}
		</table>
		<div class="flex justify-between" class:hidden={!show}>
			<button class="px-3 py-2 bg-blue-500 hover:bg-blue-600 text-white mt-2 rounded-lg disabled:bg-blue-900 disabled-text-gray-200" on:click={handleBack} disabled={page <= 1}>Anterior</button>
			<button class="px-3 py-2 bg-blue-500 hover:bg-blue-600 text-white mt-2 rounded-lg disabled:bg-blue-900 disabled-text-gray-200" on:click={handleReset} disabled={page <= 1}>Resetear</button>
			<button class="px-3 py-2 bg-blue-500 hover:bg-blue-600 text-white mt-2 rounded-lg" on:click={handleNext}>Siguiente</button>
		</div>
			</div>
		<div class="h-10 w-10 bg-zinc-900 items-center justify-center">
			<p class="text-center mt-2 font-bold text-cyan-300 cursor-pointer" on:click={toggleTable}>{!show ? ">" : "X"}</p>
		</div>
		</div>
	</section>
	{#if $info.ath}
		<Card data={$info} blue={blue}/>
	{:else}
		<p class="text-2xl font-bold text-white mx-auto mt-5">Selecciona alguna criptomoneda</p>
	{/if}
</main>
