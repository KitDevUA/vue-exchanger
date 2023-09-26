<template>
	<div class="bg-light">
		<Loader v-if="!loaded" />
		<Converter v-else :entry_currencies="currencies" />
	</div>
</template>

<script>
import { ref, onMounted } from 'vue';
import Loader from './components/Loader.vue';
import Converter from './components/Converter.vue';

export default {
	components: {
		Loader,
		Converter,
	},
	setup() {
		const loaded = ref(false);
		const currencies = ref([
			{ currency: 'USD', rate: 37.5 },
			{ currency: 'EUR', rate: 40 },
			{ currency: 'PLN', rate: 8 },
		]);
		
		const getCurrencies = () => {
			setTimeout(() => {
				// Симуляція AJAX запиту.
				loaded.value = true;
			}, 2000);
		};
		
		onMounted(getCurrencies);
		
		return {
			loaded,
			currencies,
			getCurrencies
		};
	},
};
</script>

<style scoped>

</style>
