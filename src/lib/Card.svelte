<script>
	import { info } from '../store.js';
	export let blue;

	const clearData = () => {
		$info = {};
	}
</script>

<section class="relative w-1/3 mx-auto h-1/3 p-5 bg-zinc-900 mt-10">
	<p class="absolute font-bold text-cyan-600 cursor-pointer text-lg top-1 right-2" on:click={clearData}>X</p>
	<img src={$info.image} class="mx-auto my-3 rounded-full" width=150>	
	<p class="text-white font-bold text-2xl text-center">{$info.name} <span class="text-lg text-zinc-500 font-light uppercase ml-2">({$info.symbol})</span></p>
	<div>
		<p class="text-center text-zinc-500 text-lg mt-2">Precios:</p>
			<ul class="p-2 bg-zinc-800 rounded-lg mt-2">
				<li class="text-white font-light text-lg text-left p-2 bg-zinc-900 mb-2 rounded-md hover:scale-105 hover:shadow-md hover:shadow-zinc-700 duration-200"><span class="font-semibold text-zinc-400">ARS: </span>${($info.current_price * blue).toLocaleString()}</li>
				<li class="text-white font-light text-lg text-left p-2 bg-zinc-900 mb-2 rounded-md hover:scale-105 hover:shadow-md hover:shadow-zinc-700 duration-200"><span class="font-semibold text-zinc-400">Más alto en 24hs: </span>${$info.high_24h}</li>
				<li class="text-white font-light text-lg text-left p-2 bg-zinc-900 rounded-md hover:scale-105 hover:shadow-md hover:shadow-zinc-700 duration-200 mb-2"><span class="font-semibold text-zinc-400">Más bajo en 24hs: </span>${$info.low_24h}</li>
				<li class="text-white font-light text-lg text-left p-2 bg-zinc-900 rounded-md hover:scale-105 hover:shadow-md hover:shadow-zinc-700 duration-200"><span class="font-semibold text-zinc-400">Pico Máximo: </span>${$info.ath.toLocaleString()}</li>
			</ul>
	</div>
</section>