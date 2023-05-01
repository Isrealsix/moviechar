<script setup lang="ts">
interface IProps {
	image: string;
	name: string;
	episodes: number;
}


const { image, name, episodes } = defineProps<IProps>();
</script>
<template>
	<NCard>
		<template #cover>
			<img :src="image">
		</template>
		<h3>{{ name }}</h3>
		<p>Episodes: {{ episodes }}</p>
	</NCard>
</template>

<style scoped>
	.n-card {
	width: 200px;
	margin: 10px 20px;

}
.n-card img {
	height: 250px
}

p {
	font-size: 10px;
}

.jobs {
	display: flex;
	flex-wrap: wrap;
}
</style>