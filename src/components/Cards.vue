<script lang="ts" setup>
import axios from 'axios';
import { ref, watch } from 'vue';

type TCharacters = {
	created: string;
	episode: string[];
	gender: string;
	id: number;
	image: string;
	location: {
		name: string;
		url: string;
	}
	name: string;
	origin: {
		name: string;
		url: string;
	};
	species: string;
	status: string;
	url: string;
}[]
const characters = ref<TCharacters | null>(null);
const page = ref(1);
const totalPages = ref<number | null>(null);
const response = await axios.get('https://rickandmortyapi.com/api/character?page=1');
characters.value = response.data.results;
totalPages.value = response.data.info.pages;

watch(page, async () => {
	const res = await axios.get(`https://rickandmortyapi.com/api/character?page=${page.value}`);
	characters.value = res.data.results;
	console.log(characters.value)
})
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
</script>

<template>
	<div>
		<h1>Breaking Bad Cards</h1>
	</div>
	{{ characters }}
	<button @click="handlePagination.call(null, 'prev')">Prev</button>
	<button @click="handlePagination.call(null, 'next')">Next</button>
</template>