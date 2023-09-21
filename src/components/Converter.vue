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
export default {
	props: ['entry_currencies'],
	data() {
		return {
			currencies: [],
		};
	},
	methods: {
		calcUahToCurrency(i) {
			this.currencies[i].currencyAmount = this.formatValue(this.currencies[i].uahAmount / this.currencies[i].rate);
		},
		calcCurrencyToUah(i) {
			this.currencies[i].uahAmount = this.formatValue(this.currencies[i].currencyAmount * this.currencies[i].rate);
		},
		
		formatValue(value) {
			const rounded = Math.round(value * 100) / 100;
			return Number(rounded.toFixed(2).replace(/\.?0+$/, ''));
		},
		
		createCurrencies() {
			this.entry_currencies.forEach((currency) => {
				this.currencies.push({
					name: currency.currency,
					rate: currency.rate,
					uahAmount: 100,
					currencyAmount: 0,
				});
			});
			
			this.currencies.forEach((currency, i) => {
			this.calcUahToCurrency(i);
		});
		},
	},
	mounted() {
		this.createCurrencies();
	},
};
</script>