<script>
    import { flip } from 'svelte/animate';
    import { dndzone } from 'svelte-dnd-action';

    export let items;
    export let onFinalize;

    export let filter = '';

    // flipDurationMs
    export let duration = 150;

    $: filtered = items.filter(item => item.name.toLowerCase().includes(filter.toLowerCase()));

    function onConsider(event) {
        items = event.detail.items;
    }
</script>

<ul
    class="flex flex-col gap-y-2 overflow-y-scroll h-full grow p-1"
    use:dndzone={{items, flipDurationMs: duration, zoneTabIndex: -1}}
    on:consider={onConsider}
    on:finalize={event => onFinalize(event.detail.items)}
>
    {#each filtered as item, idx (item.id)}
        <slot {item} {idx}></slot>
    {/each}
</ul>
