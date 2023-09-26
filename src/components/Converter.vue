<template>
	<div class="container">
		<div class="px-5 py-2 rounded">
			<h1 class="text-center mb-3">Конвертер валют</h1>
			<div 
				v-for="(currency, i) in currencies"
				:key="i"
				class="mb-4"
			>
				<div class="card">
					<div class="card-header text-white bg-primary">
						{{ currency.name }}
					</div>
					<div class="card-body">
						<div class="row">
							<div class="col">
								<label for="uah-amount">Сума в гривні</label>
								<input 
									v-model="currency.uahAmount" 
									@input="calcUahToCurrency(i)"
									class="form-control" type="number"
								>
							</div>
							<div class="col">
								<label for="currency-rate">Курс</label>
								<input :value="formatValue(currency.rate)" id="currency-rate" class="form-control" type="number" readonly>
							</div>
							<div class="col">
								<label for="foreign-amount">Сума в іноземній валюті</label>
								<input 
									v-model="currency.currencyAmount" 
									@input="calcCurrencyToUah(i)"
									class="form-control" type="number"
								>
							</div>
						</div>
					</div>
				</div>
			</div>
		</div>
	</div>
</template>

<script>
import { ref, reactive, onMounted } from 'vue';

export default {
	props: ['entry_currencies'],
	setup(props) {
		const entry_currencies = ref(props.entry_currencies);
		let currencies = reactive([]);
		
		const calcUahToCurrency = (i) => {
			currencies[i].currencyAmount = formatValue(currencies[i].uahAmount / currencies[i].rate);
		};
		const calcCurrencyToUah = (i) => {
			currencies[i].uahAmount = formatValue(currencies[i].currencyAmount * currencies[i].rate);
		};
		
		const formatValue = (value) => {
			const rounded = Math.round(value * 100) / 100;
			return Number(rounded.toFixed(2).replace(/\.?0+$/, ''));
		};
		
		const createCurrencies = () => {
			entry_currencies.value.forEach((currency) => {
				currencies.push({
					name: currency.currency,
					rate: currency.rate,
					uahAmount: 100,
					currencyAmount: 0,
				});
			});
			
			currencies.forEach((currency, i) => {
				calcUahToCurrency(i);
			});
		};
		
		onMounted(createCurrencies);
		// Або:
		/* onMounted(() => {
			createCurrencies();
		}); */
		
		return {
			currencies,
			formatValue,
			calcUahToCurrency,
			calcCurrencyToUah
		};
	},
};
</script>