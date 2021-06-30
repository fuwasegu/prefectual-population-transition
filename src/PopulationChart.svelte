<script>
    import axios from "axios";
    import { Chart } from "chart.js";
    import { onMount } from 'svelte';
    import 'chartjs-plugin-colorschemes';

    export let selectedPrefs = [];

    let ctx, populationChart;

    const renderChart = (populations) => {
        if(!populationChart) {
            ctx = document.getElementById("population-chart");
                populationChart = new Chart(ctx, {
                type: 'line',
                data: {
                    labels: ["1960", "1965", "1970", "1975", "1980", "1985", "1990", "1995", "2000", "2005", "2010", "2015", "2020", "2025", "2030", "2035", "2040", "2045"],
                    datasets: populations
                },
                options: {
                    tooltips: {
                        mode: 'label'
                    },
                    plugins: {
                        colorschemes: {
                            scheme: 'brewer.Paired12'
                        }
                    }
                }
            })
        }else {
            populationChart.data.datasets = populations;
            populationChart.update();
        }
        
    }
    const getPopulationData = (selectedPrefs) => Promise.all(
        selectedPrefs.map(pref =>
            axios({
                method: 'get',
                url: 'https://opendata.resas-portal.go.jp/api/v1/population/composition/perYear?prefCode='+pref.code,
                headers: {'X-API-KEY': '*****************************************'}
            })
            .then(response => ({
                label: pref.name,
                data: response.data.result.data[0].data.map(population => population.value),
            })),
        ),
    );

    const rewritePrefChart = (selectedPrefs) => {
        getPopulationData(selectedPrefs).then((populations) => {
            renderChart(populations);
        });
    }
    
    $: rewritePrefChart(selectedPrefs);
    
    onMount(renderChart);
</script>

<div class="chart-area">
    <canvas id="population-chart"></canvas>
</div>

<style>
    .chart-area {
        margin:0 auto;
        width: 980px; 
        height: 490px;
        position: relative; 
    }
</style>