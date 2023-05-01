<script lang="ts" setup>
import axios from 'axios';
import { ref, watch } from 'vue';
import Card from './Card.vue';

type TCharacters = {
	created: string;
	episode: string[];
	gender: string;
	id: number;
	image: string;
	location: {
		name: string;
		url: string;
	};
	name: string;
	origin: {
		name: string;
		url: string;
	};
	species: string;
	status: string;
	url: string;
}[];
const characters = ref<TCharacters | null>(null);
const page = ref(1);
const totalPages = ref<number | null>(null);
const response = await axios.get(
	'https://rickandmortyapi.com/api/character?page=1'
);
characters.value = response.data.results;
totalPages.value = response.data.info.pages;

watch(page, async () => {
	const res = await axios.get(
		`https://rickandmortyapi.com/api/character?page=${page.value}`
	);
	characters.value = res.data.results;
	console.log(characters.value);
});
console.log(characters.value?.[0].created);

function handlePagination(direction: 'next' | 'prev') {
	if (direction === 'next') {
		if (totalPages.value && page.value >= totalPages.value) return;
		++page.value;
	}
	if (direction === 'prev') {
		if (page.value <= 1) return;
		--page.value;
	}
}

console.log(handlePagination);
</script>

<template>
	<div class="container">
		<div class="cards">
			<Card
				v-for="character in characters"
				:key="character.id"
				:name="character.name"
				:image="character.image"
				:episodes="character.episode.length"
			/>
			<!-- <NButton type="primary">Primary</NButton> -->
		</div>
		<div class="button-container">
			<button @click="handlePagination.call(null, 'prev')">&lt;</button>
			<button @click="handlePagination.call(null, 'next')">&gt;</button>
		</div>
	</div>
</template>

<style scoped>
.container {
	background-color: rgb(27, 26, 26);
	padding: 30px;
}
.cards {
	max-width: 1000px;
	margin: 0 auto;
	display: flex;
	flex-wrap: wrap;
	/* height: 700px; */
	height: min-content;
}
.cards h3 {
	font-weight: bold;
}
.cards p {
	font-size: 10px;
}
.jobs {
	display: flex;
	flex-wrap: wrap;
}
.button-container {
	display: flex;
	justify-content: center;
	padding-top: 30px;
}
.button-container button {
	border: none;
	width: 50px;
	height: 50px;
	border-radius: 100%;
	margin: 0 5px;
	cursor: pointer;
}
.spinner {
	display: flex;
	align-items: center;
	justify-content: center;
}
</style>
