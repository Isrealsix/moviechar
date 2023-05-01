<script setup lang="ts">
import Cards from './components/Cards.vue'
</script>

<template>
	<main>
		<h1>HERO</h1>
		<Suspense>
			<template #default>
				<Cards />
			</template>
			<template #fallback>
				<div>
					<p>Loading...</p>
				</div>
			</template>
		</Suspense>
		<Cards />
	</main>
</template>