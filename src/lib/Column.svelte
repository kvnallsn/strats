<script>
    import { flip } from 'svelte/animate';
    import { dndzone } from 'svelte-dnd-action';

    const flipDurationMs = 150;

    export let name;
    export let items;
    export let onDrop;

    function handleDndConsiderCards(e) {
        console.warn("got consider", name);
        items = e.detail.items;
    }

    function handleDndFinalizeCards(e) {
        onDrop(e.detail.items);
    }
</script>

<div class="w-full h-full overflow-y-hidden flex flex-col">
    <div class="font-bold flex justify-center items-center border-b py-1 mb-1">
        {name}
    </div>
    <ul
        class="flex flex-col divide-y divide-gray-100 gap-y-2 overflow-y-scroll h-full grow p-1"
        use:dndzone={{items, flipDurationMs, zoneTabIndex: -1}}
        on:consider={handleDndConsiderCards}
        on:finalize={handleDndFinalizeCards}
    >
        {#each items as item, idx (item.id)}
            <li class="flex justify-between gap-x-6 py-5 px-2 border border-gray-300 rounded-md" animate:flip="{{duration: flipDurationMs}}">
                <div class="flex items-center min-w-0 gap-x-4">
                    <p>{idx + 1}</p> 
                    <div class="min-w-0 flex-auto">
                        <p class="text-sm font-semibold leading-6 text-gray-900">{item.name}</p>
                        <p class="text-xs mt-1 truncate leading-5 text-gray-500">{item.title}</p>
                    </div>
                </div>
            </li>
        {/each}
    </ul>
</div>
