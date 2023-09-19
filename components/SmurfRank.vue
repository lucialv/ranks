<template>
	<div class="mr-4 flex h-auto items-center justify-center">
		<div>
			<div class="flex">
				<h1 class="mb-4 whitespace-nowrap text-center text-xl text-white md:w-[350px] md:text-2xl lg:text-2xl">
					<div class="mb-4 flex items-center">
						<!-- <img
							src="https://cdn.discordapp.com/attachments/1126125323101151252/1146928211352109287/c184e9750723afcc6e1ecc57284fe1af.png"
							class="mr-2 h-8 w-8 rounded-full"
							alt="logolucia"
						/> -->
						<a
							href="https://tracker.gg/valorant/profile/riot/LuciaZapatito%23love/overview"
							target="_blank"
							class="mx-auto font-bold hover:underline"
							:style="{ color: getTierColor(currentTierPatched) }"
							>{{ playerName }}#{{ tag }}</a
						>
					</div>
					<img :src="images.large" alt="Large Icon" class="mx-auto h-32 w-32 md:h-44 md:w-44" />
					<p class="my-4 font-bold" :style="{ color: getTierColor(currentTierPatched) }">
						{{ getTierTranslation(currentTierPatched).toUpperCase() }}
					</p>
					<div class="relative mb-2 h-2 w-full overflow-hidden rounded-full bg-white/60">
						<div
							class="animate-stripes absolute left-0 top-0 h-full rounded-lg bg-[#23fed7]"
							:style="{ width: `${rankingInTier}%` }"
						></div>
					</div>
					<div class="mb-10 flex">
						<p class="text-start text-sm">RANK RATING</p>
						<p class="ml-auto text-sm font-bold">
							<span class="text-[#23fed7]">{{ rankingInTier }}</span> / 100
						</p>
					</div>

					<p v-if="mmrChangeToLastGame > 0" class="text-basee font-semibold md:text-2xl lg:text-2xl">
						Última partida: <span class="font-bold text-[#23fed7]">+{{ mmrChangeToLastGame }}</span>
					</p>
					<p v-else class="lg:text-2xle text-base font-semibold md:text-2xl">
						Última partida: <span class="font-bold text-[#f0615f]">{{ mmrChangeToLastGame }}</span>
					</p>
					<p v-if="100 - rankingInTier >= 76" class="lg:text-2xle text-base font-semibold md:text-2xl">
						Puntos para subir: <span class="font-bold text-[#f0615f]">{{ 100 - rankingInTier }}</span>
					</p>
					<p v-else-if="100 - rankingInTier >= 51" class="lg:text-2xle text-base font-semibold md:text-2xl">
						Puntos para subir: <span class="font-bold text-orange-300">{{ 100 - rankingInTier }}</span>
					</p>
					<p v-else-if="100 - rankingInTier >= 26" class="lg:text-2xle text-base font-semibold md:text-2xl">
						Puntos para subir: <span class="font-bold text-yellow-300">{{ 100 - rankingInTier }}</span>
					</p>
					<p v-else-if="100 - rankingInTier >= 0" class="lg:text-2xle text-base font-semibold md:text-2xl">
						Puntos para subir: <span class="font-bold text-emerald-300">{{ 100 - rankingInTier }}</span>
					</p>
					<p class="text-base font-semibold md:text-2xl lg:text-2xl">
						Total de puntos: <span class="font-bold text-[#23fed7]">{{ elo }}</span>
					</p>
				</h1>
			</div>
		</div>
	</div>
</template>
<script setup>
import { ref, onMounted } from 'vue';

const apiUrl = 'https://api.henrikdev.xyz/valorant/v1/mmr/eu/LuciaZapatito/love';
const playerName = ref('');
const currentTier = ref('');
const mmr = ref(0);
const currentTierPatched = ref('');
const rankingInTier = ref(0);
const mmrChangeToLastGame = ref(0);
const elo = ref(0);
const tag = ref('');
const isFinished = ref(false);
const images = ref({
	small: '',
	large: '',
	triangle_down: '',
	triangle_up: ''
});

const tierColors = {
	'Iron 1': '#838283',
	'Iron 2': '#838283',
	'Iron 3': '#838283',
	'Bronze 1': '#a4855c',
	'Bronze 2': '#a4855c',
	'Bronze 3': '#a4855c',
	'Silver 1': '#bac3c2',
	'Silver 2': '#bac3c2',
	'Silver 3': '#bac3c2',
	'Gold 1': '#ecce57',
	'Gold 2': '#ecce57',
	'Gold 3': '#ecce57',
	'Platinum 1': '#57a7b2',
	'Platinum 2': '#57a7b2',
	'Platinum 3': '#57a7b2',
	'Diamond 1': '#b588c4',
	'Diamond 2': '#b588c4',
	'Diamond 3': '#b588c4',
	'Ascendant 1': '#6be0af',
	'Ascendant 2': '#6be0af',
	'Ascendant 3': '#6be0af',
	'Immortal 1': '#bb3c64',
	'Immortal 2': '#bb3c64',
	'Immortal 3': '#bb3c64',
	Radiant: '#ffffaa'
};

const tierTranslations = {
	'Iron 1': 'Hierro 1',
	'Iron 2': 'Hierro 2',
	'Iron 3': 'Hierro 3',
	'Bronze 1': 'Bronce 1',
	'Bronze 2': 'Bronce 2',
	'Bronze 3': 'Bronce 3',
	'Silver 1': 'Plata 1',
	'Silver 2': 'Plata 2',
	'Silver 3': 'Plata 3',
	'Gold 1': 'Oro 1',
	'Gold 2': 'Oro 2',
	'Gold 3': 'Oro 3',
	'Platinum 1': 'Platino 1',
	'Platinum 2': 'Platino 2',
	'Platinum 3': 'Platino 3',
	'Diamond 1': 'Diamante 1',
	'Diamond 2': 'Diamante 2',
	'Diamond 3': 'Diamante 3',
	'Ascendant 1': 'Ascendente 1',
	'Ascendant 2': 'Ascendente 2',
	'Ascendant 3': 'Ascendente 3',
	'Immortal 1': 'Inmortal 1',
	'Immortal 2': 'Inmortal 2',
	'Immortal 3': 'Inmortal 3',
	Radiant: 'Radiante'
};

const getTierColor = (tier) => {
	return tierColors[tier] || ''; // Si el rango no está en la lista, no se aplicará ningún color
};

const getTierTranslation = (tier) => {
	return tierTranslations[tier] || tier; // Si el rango no está en la lista, se mostrará el nombre original
};

const fetchData = async () => {
	try {
		const response = await fetch(apiUrl);
		if (!response.ok) {
			throw new Error('No se ha podido obtener la información');
		}
		const data = await response.json();
		playerName.value = data.data.name;
		currentTier.value = data.data.currenttier;
		mmr.value = data.data.elo;
		currentTierPatched.value = data.data.currenttierpatched;
		rankingInTier.value = data.data.ranking_in_tier;
		mmrChangeToLastGame.value = data.data.mmr_change_to_last_game;
		elo.value = data.data.elo;
		tag.value = data.data.tag;
		isFinished.value = data.isFinished;
		images.value = data.data.images;
	} catch (error) {
		console.error(error);
	}
};

onMounted(() => {
	fetchData();
});
</script>
