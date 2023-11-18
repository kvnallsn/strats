<script>
    import { flip } from 'svelte/animate';
    import { dndzone } from 'svelte-dnd-action';
    import Card from './Card.svelte';

    const flipDurationMs = 150;

    export let name;
</script>

<div class="w-1/3 border border-gray-200 dark:border-slate-600 rounded-md">
    <div class="w-full h-full overflow-y-hidden flex flex-col">
        <div class="font-bold text-center border-b bg-gray-200 dark:bg-slate-700 dark:text-white p-2 mb-1">
            {name}
        </div>
        <slot name="action"></slot>
        <slot></slot>
    </div>
</div>
