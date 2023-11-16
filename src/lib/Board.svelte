<script>
    import { flip } from 'svelte/animate';
    import { dndzone } from 'svelte-dnd-action';
    import Column from './Column.svelte';

    const flipDurationMs = 300;

    // column items/data
    export let columns;

    // will be called anyime a card/column gets dropped to update parent data
    export let onFinalUpdate;

    function handleDndConsiderColumns(e) {
        columns = e.detail.items;
    }

    function handleDndFinalizeColumns(e) {
        onFinalUpdate(e.detail.items);
    }

    function handleItemFinalize(columnIdx, newItems) {
        columns[columnIdx].items = newItems;
        onFinalUpdate([...columns]);
    }
</script>

<style>
    .board {
        height: 60vh;
        width: 100%;
        padding: 0.5em;
    }

    .column {
        height: 100%;
        width: 35%;
        padding: 0.5em;
        margin: 0.5em;
        float: left;
        border: 1px solid #333333;
    }
</style>

<section
    class="board"
    use:dndzone={{items: columns, flipDurationMs, type: 'column'}}
    on:consider={handleDndConsiderColumns}
    on:finalize={handleDndFinalizeColumns}
>
    {#each columns as {id, name, items}, idx (id)}
        <div class="column" animate:flip="{{duration: flipDurationMs}}">
            <Column name={name} items={items} onDrop={(newItems) => handleItemFinalize(idx, newItems)} /> 
        </div>
    {/each}
</section>
