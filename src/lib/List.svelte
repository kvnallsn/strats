<script>
    export let items;
    export let filter;

    $: filtered = items.filter(filter)
</script>

<ul class="flex flex-col gap-y-2 overflow-y-scroll p-1">
    {#each filtered as item, idx (item.id)}
        <slot {item} {idx}></slot>
    {/each}
</ul>
