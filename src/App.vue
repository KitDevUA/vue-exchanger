<template>
	<div class="bg-light">
		<Loader v-if="!loaded" />
		<Converter v-else :entry_currencies="currencies" />
	</div>
</template>

<script>
import Loader from './components/Loader.vue';
import Converter from './components/Converter.vue';

export default {
	data() {
		return {
			loaded: false,
			currencies: [
				{ currency: 'USD', rate: 37.5 },
				{ currency: 'EUR', rate: 40 },
				{ currency: 'ZLT', rate: 8 },
			],
		};
	},
	components: {
		Loader,
		Converter,
	},
	methods: {
		getCurrencies() {
			setTimeout(() => {
				// Симуляція AJAX запиту..
				this.loaded = true;
			}, 1000);
		},
	},
	mounted() {
		this.getCurrencies();
	},
};
</script>

<style scoped>

</style>
