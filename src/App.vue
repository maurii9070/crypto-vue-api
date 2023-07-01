<script setup>
	import { onMounted, ref, reactive } from 'vue';
	import Alerta from './components/Alerta.vue';

	const monedas = ref([
		{ codigo: 'USD', texto: 'Dolar de Estados Unidos' },
		{ codigo: 'MXN', texto: 'Peso Mexicano' },
		{ codigo: 'EUR', texto: 'Euro' },
		{ codigo: 'GBP', texto: 'Libra Esterlina' },
	]);

	const criptomonedas = ref([]);
	const error = ref('');

	const cotizar = reactive({
		moneda: '',
		criptomoneda: '',
	});

	onMounted(() => {
		const url = 'https://min-api.cryptocompare.com/data/top/mktcapfull?limit=20&tsym=USD';
		fetch(url)
			.then(respuesta => respuesta.json())
			.then(data => {
				criptomonedas.value = data.Data;
			});
	});

	const cortizarCripto = () => {
		// Validar que cotizar este lleno
		if (Object.values(cotizar).includes('')) {
			error.value = 'Todos los campos son obligatorio';
			return;
		}

		error.value = '';
		console.log('Paso las validaciones');
	};
</script>

<template>
	<div class="contenedor">
		<h1 class="titulo">Cotizador de <span>Criptomonedas</span></h1>

		<div class="contenido">
			<Alerta v-if="error">{{ error }}</Alerta>

			<form
				class="formulario"
				@submit.prevent="cortizarCripto"
			>
				<div class="campo">
					<label for="moneda">Moneda:</label>
					<select
						id="moneda"
						v-model="cotizar.moneda"
					>
						<option value="">-- Selecciona --</option>
						<option
							v-for="moneda in monedas"
							:value="moneda.codigo"
						>
							{{ moneda.texto }}
						</option>
					</select>
				</div>

				<div class="campo">
					<label for="cripto">Criptomoneda:</label>
					<select
						id="cripto"
						v-model="cotizar.criptomoneda"
					>
						<option value="">-- Selecciona --</option>
						<option
							v-for="criptomoneda in criptomonedas"
							:value="criptomoneda.CoinInfo.Name"
						>
							{{ criptomoneda.CoinInfo.FullName }}
						</option>
					</select>
				</div>

				<input
					type="submit"
					value="Cotizar"
				/>
			</form>
		</div>
	</div>
</template>

<style scoped></style>
