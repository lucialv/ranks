<template>
	<div class="mb-8 h-full">
		<div class="mt-16 flex items-center justify-center">
			<div class="w-1/2">
				<div class="flex justify-center">
					<h1 class="mb-4 whitespace-nowrap text-center text-2xl font-bold text-white lg:text-3xl">
						<span class="align-bottom"><span class="text-red-400">Mis rangos</span> en <span class="text-red-400">Valorant</span></span>
					</h1>
				</div>
			</div>
		</div>
		<div class="mx-auto mt-16 flex justify-center">
			<LuciaRank />
		</div>
		<div class="mx-auto mt-8 flex justify-center">
			<SmurfRank />
			<SmurfRank4 />
		</div>
		<div class="mx-auto mt-8 flex justify-center">
			<SmurfRank3 />
			<SmurfRank2 />
		</div>
		<Diferencia />
		<div class="mt-28">
			<FooterPage />
		</div>
	</div>
</template>
