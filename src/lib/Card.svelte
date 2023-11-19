<script>
    import { Icon } from '@steeze-ui/svelte-icon';
    import { User } from '@steeze-ui/heroicons';

    // rank svgs
    import rankO1 from '../assets/rank-O1.svelte';
    import rankO2 from '../assets/rank-O2.svelte';
    import rankO3 from '../assets/rank-O3.svelte';
    import rankO4 from '../assets/rank-O4.svelte';
    import rankO5 from '../assets/rank-O5.svelte';
    import rankO6 from '../assets/rank-O6.svelte';

    export let item;
    export let idx = 0;

    // callback functions
    export let onDoubleClick = () => {};
    export let onRightClick = () => {};

    function grade(g) {
        switch (g) {
            case '01-2LT':
                return rankO1;
            case '02-1LT':
                return rankO2;
            case '03-CPT':
                return rankO3;
            case '04-MAJ':
                return rankO4;
            case '05-LTC':
                return rankO5;
            case '06-COL':
                return rankO6;
            default:
                return g;
        }
    }
</script>

<li
    class="flex items-center h-10 hover:bg-slate-200"
    on:dblclick={onDoubleClick}
    on:contextmenu={onRightClick}
    >
    <div class="p-2 w-12 rounded-l-md bg-gray-100 border border-gray-300 dark:bg-slate-600 dark:border-slate-600 h-full flex items-center justify-center">
        {#if idx > 0}
            <span class="font-mono text-sm">{idx}</span>
        {:else}
            <Icon src={User} size="16px" theme="outline" class="text-black dark:text-white" />
        {/if}
    </div>
    <div class="flex grow justify-between pl-2 border-y border-gray-300 dark:border-slate-600 h-full items-center">
        <div class="flex flex-col">
            <p class="text-sm font-semibold leading-5 text-gray-900 dark:text-white">{item.name}</p>
            <p class="text-xs pr-2 truncate leading-3 text-gray-500 dark:text-slate-400">{item.title}</p>
        </div>
    </div>
    <div class="min-w-[10px] h-full border-r border-y border-gray-300 dark:border-slate-600 rounded-r-md flex items-center divide-x divide-gray-300">
        <svelte:component this={grade(item.grade)}  class="h-9 w-9 p-2" />
        <slot name="actions"></slot>
    </div>
</li>
