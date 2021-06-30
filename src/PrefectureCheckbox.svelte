<script>
    import axios from "axios";
    import { onMount } from 'svelte';

    $: data = [];
    $: prefectures = [];
    export let selectedPrefs = [];

    onMount(async () => {
        data = axios({
            method: 'get',
            url: 'https://opendata.resas-portal.go.jp/api/v1/prefectures',
            headers: {'X-API-KEY': 'hqd83fti1ReyFFjxaywjukGst6s1AvGzeMgDqwVH'}
        })
        .then(response => {
            data = response.data.result;
            prefectures = Object.values(data);
        });
    });
</script>

<div class="prefecture-checkbox-area">
    <div class="content">
        {#each prefectures as pref}
            <label class="pref-label" name="pref{pref.prefCode}" >
                <input type="checkbox" bind:group="{selectedPrefs}" value="{{"code": pref.prefCode, "name": pref.prefName}}">
                {pref.prefName}
            </label>        
        {/each}
    </div>
</div>

<style>
    .prefecture-checkbox-area {
        padding-bottom: 32px;
    }
    .content {
        display: grid;
        grid-template-columns: repeat(auto-fit, 100px);
        grid-row-gap: 20px;
        grid-column-gap: 10px;
        justify-content: center;
        width: 900px;
        margin: 0 auto;
        
    }
    .pref-label {
        text-align: left;
    }
</style>