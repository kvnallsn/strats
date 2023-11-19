<script>
    export let onSuccess;
    export let onCancel;
    
    const templates = [
        {name: "Flight CC", filter: /.*(Flight Commander).*/i},
        {name: "Team Leads", filter: /.*(Team Lead).*/i},
        {name: "O-4 Command (DO, Det/CC)", filter: /^(Director of Op|Commander|Det\/CC).*/i}
    ];

    let name = '';
    let filter = '';
    let selected = '';

    function onTemplateSelect() {
        name = templates[selected].name;
        filter = templates[selected].filter;
    }
</script>

<div class="border-b border-slate-900/10 pb-4">
    <div class="grid grid-cols-1 gap-x-6 gap-y-4 sm:grid-cols-6">
        <div class="sm:col-span-full border-b pb-2">
            <select id="templates" class="border rounded-md p-2 w-full bg-white text-slate-900" bind:value={selected} on:change={onTemplateSelect}>
                <option value='' disabled>Select a Template</option>
                {#each templates as category, idx}
                    <option value={idx}>{category.name}</option>
                {/each}
            </select>
        </div>

        <div class="sm:col-span-full">
            <label for="templates" class="block text-sm font-medium leading-6 text-slate-900">Category Name</label>
            <div class="mt-2">
                <div class="flex rounded-md shadow-sm ring-1 ring-inset ring-slate-300 sm:max-w-md">
                    <input
                        type="text"
                        bind:value={name}
                        class="block flex-1 border-0 bg-transparent py-1.5 pl-1 text-slate-900 placeholder:text-slate-400 focus:ring-0 sm:text-sm sm:leading-6"
                        placeholder="Filter name (e.g. Team Leads, DOs, etc.)"
                    >
                </div>
            </div>
        </div>

        <div class="sm:col-span-full">
            <label for="templates" class="block text-sm font-medium leading-6 text-slate-900">Category Filter</label>
            <div class="mt-2">
                <div class="flex rounded-md shadow-sm ring-1 ring-slate-300 sm:max-w-md">
                    <input
                        type="text"
                        bind:value={filter}
                        class="block flex-1 border-0 bg-transparent py-1.5 pl-1 text-slate-900 placeholder:text-slate-400 focus:ring-0 sm:text-sm sm:leading-6"
                        placeholder="Enter a regex filter"
                    >
                </div>
            </div>
        </div>
    </div>
</div>

<div class="mt-6 flex items-center justify-end gap-x-6">
    <button
        type="button" class="text-sm font-semibold leading-6 text-slate-900"
        on:click={onCancel}
    >
        Cancel
    </button>
    <button
        type="button"
        class="rounded-md bg-indigo-600 px-3 py-2 text-sm font-semibold text-white shadow-sm hover:bg-indigo-500 focus-visible:outline focus-visible:outline-2 focus-visible:outline-offset-2 focus-visible:outline-indigo-600"
        on:click={() => onSuccess(name, filter)}
    >
        Add
    </button>
</div>
