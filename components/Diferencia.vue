<template>
	<div class="mt-24 flex h-auto items-center justify-center">
		<div class="flex">
			<h1 class="mb-4 text-center text-xl text-white md:w-[350px] md:text-2xl lg:text-2xl">
				<p class="bg-gradient-to-r from-sky-300 via-blue-400 to-emerald-400 bg-clip-text font-bold text-transparent">
					Puntos en Total: <span class="text-2xl underline md:text-3xl lg:text-4xl">{{ diferenciaElo }}</span>
				</p>
			</h1>
		</div>
	</div>
</template>

<script setup>
import { ref, onMounted } from 'vue';

const apiUrl1 = 'https://api.henrikdev.xyz/valorant/v1/mmr/eu/shy%20lucia/love';
const apiUrl2 = 'https://api.henrikdev.xyz/valorant/v1/mmr/eu/LuciaZapatito/love';
const apiUrl3 = 'https://api.henrikdev.xyz/valorant/v1/mmr/eu/mochi%20ツ/1307';
const apiUrl4 = 'https://api.henrikdev.xyz/valorant/v1/mmr/eu/lucia%20plays%20sage/inmo';
const apiUrl5 = 'https://api.henrikdev.xyz/valorant/v1/mmr/eu/lucia%20te%20abuchea/buu';
const elo1 = ref(0);
const elo2 = ref(0);
const elo3 = ref(0);
const elo4 = ref(0);
const elo5 = ref(0);
const diferenciaElo = ref(0);

const fetchData = async (url, targetRef) => {
	try {
		const response = await fetch(url);
		if (!response.ok) {
			throw new Error('No se ha podido obtener la información');
		}
		const data = await response.json();
		targetRef.value = data.data.elo;
	} catch (error) {
		console.error(error);
	}
};

onMounted(async () => {
	await fetchData(apiUrl1, elo1);
	await fetchData(apiUrl2, elo2);
	await fetchData(apiUrl3, elo3);
	await fetchData(apiUrl4, elo4);
	await fetchData(apiUrl5, elo5);
	diferenciaElo.value = elo1.value + elo2.value + elo3.value + elo4.value + elo5.value;
	console.log(elo5.value);
	console.log(elo4.value);
	console.log(elo3.value);
	console.log(elo2.value);
	console.log(elo1.value);
});
</script>
