<script>
    import { Icon } from '@steeze-ui/svelte-icon';
    import { User } from '@steeze-ui/heroicons';

    export let item;
    export let idx = 0;
</script>

<li class="flex items-center h-10">
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
    <div class="min-w-[10px] h-full border-r border-y border-gray-300 dark:border-slate-600 rounded-r-md flex divide-x divide-gray-300">
        <slot name="actions"></slot>
    </div>
</li>
