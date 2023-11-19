<script lang="ts">
    import { onMount } from 'svelte';
    import Board from './lib/Board.svelte';

    import { saveAs } from 'file-saver';
    import { parse } from 'csv-parse/browser/esm/sync';

    let data = initData();

    onMount(() => {
        const store = localStorage.getItem('data');
        if (store) {
            try {
                data = JSON.parse(store);
            } catch {
                data = initData(); 
            }
        }
    });

    function initData() {
        return {
            pool: [],
            strats: [],
            views: [],
        };
    }

    function handleBoardUpdated(newData) {
        if (newData) {
            data = newData;
            localStorage.setItem('data', JSON.stringify(data));
        }
    }

    /**
     * Imports a CSV file into the pool
     */
    async function importCSV(e) {
        const file = e.target.files[0];
        if (file === null) {
            // user cancelled the dialog
            return;
        }

        const text = await file.text();
        const pool = parse(text, {
            columns: true,
            skip_empty_lines: true
        });

        data.pool = pool;
        handleBoardUpdated(data);
    }

    function save() {
        const file = new Blob([JSON.stringify(data)], {type: 'text/plain'});
        saveAs(file, "strats.json");
    }

    async function load(e) {
        const file = e.target.files[0];
        if (file === null) {
            // user cancelled the dialog
            return;
        }

        const text = await file.text();
        try {
            handleBoardUpdated(JSON.parse(text));
        } catch(e) {
            console.error('failed to parse JSON', e);
        }
    }

    function reset() {
        data = initData();
    }
</script>

<header class="fixed top-0 w-full h-12 text-white bg-gray-800 border-b border-gray-500 flex justify-between items-center px-2">
    <h2 class="text-xl">Stratification Tool</h2>
    <div class="flex gap-x-2">
        <label for="import-file" class=" cursor-pointer px-3 py-1 border rounded-md hover:bg-orange-500">
            <span>Import Pool</span>
            <input id="import-file" type="file" class="sr-only" on:change={importCSV}>
        </label>
        <button class="px-3 py-1 border rounded-md hover:bg-orange-500" on:click={save}>Save</button>
        <label for="load-file" class=" cursor-pointer px-3 py-1 border rounded-md hover:bg-orange-500">
            <span>Load</span>
            <input id="load-file" type="file" class="sr-only" on:change={load}>
        </label>
        <button class="px-3 py-1 border rounded-md bg-red-600 border-red-300 hover:bg-orange-500" on:click={reset}>Reset</button>
    </div>
</header>
<main class="pt-12 max-h-full flex flex-col bg-white dark:bg-black">
   <Board data={data} onFinalUpdate={handleBoardUpdated} />
</main>
