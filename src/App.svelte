<script>
	import PrefectureCheckbox from './PrefectureCheckbox.svelte';
	import PopulationChart from './PopulationChart.svelte';
	import Header from './Header.svelte';

	let selectedPrefs = [];
</script>


<div>
	<Header></Header>
	<main>
		<PrefectureCheckbox bind:selectedPrefs />
		<PopulationChart selectedPrefs={selectedPrefs}/>	
	</main>
	</div>

<style>
	main {
		text-align: center;
		padding: 1em;
		max-width: 240px;
		margin: 0 auto;
	}

	@media (min-width: 640px) {
		main {
			max-width: none;
		}
	}
</style>