<script>
    import { flip } from 'svelte/animate';
    import { dndzone } from 'svelte-dnd-action';
    import Card from './Card.svelte';

    const flipDurationMs = 150;

    export let name;

    /*
    let search = '';
    $: filtered = items.filter(item => item.name.toLowerCase().includes(search.toLowerCase()));

    function handleDndConsiderCards(e) {
        items = e.detail.items;
    }

    function handleDndFinalizeCards(e) {
        onDrop(e.detail.items);
    }
    */
</script>

<div class="h-full w-1/3 p-2 border border-gray-200 rounded-md">
    <div class="w-full h-full overflow-y-hidden flex flex-col">
        <div class="font-bold flex justify-center items-center border-b py-1 mb-1">
            {name}
        </div>
        <slot name="action"></slot>
        <slot></slot>
        <!--
            <div class="p-1 mb-1 border-b">
                <input
                    type="text"
                    class="w-full border border-gray-200 rounded-md p-1"
                    placeholder="Search..."
                    bind:value={search}
                />
            </div>
        <ul
            class="flex flex-col divide-y divide-gray-100 gap-y-2 overflow-y-scroll h-full grow p-1"
            use:dndzone={{items, flipDurationMs, zoneTabIndex: -1}}
            on:consider={handleDndConsiderCards}
            on:finalize={handleDndFinalizeCards}
        >
            {#each filtered as item, idx (item.id)}
                <Card item={item} idx={idx + 1} {showRank} />
            {/each}
        </ul>
        -->
    </div>
</div>
