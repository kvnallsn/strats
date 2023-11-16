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

<section
    class="w-full flex-1 h-full flex p-2 gap-x-4"
    use:dndzone={{items: columns, flipDurationMs, type: 'column'}}
    on:consider={handleDndConsiderColumns}
    on:finalize={handleDndFinalizeColumns}
>
    {#each columns as {id, name, items}, idx (id)}
        <div class="h-full w-1/3 p-2 border border-gray-200 rounded-md" animate:flip="{{duration: flipDurationMs}}">
            <Column name={name} items={items} onDrop={(newItems) => handleItemFinalize(idx, newItems)} /> 
        </div>
    {/each}
</section>
