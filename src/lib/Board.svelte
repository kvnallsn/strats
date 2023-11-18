<script>
    import { flip } from 'svelte/animate';
    import { dndzone } from 'svelte-dnd-action';
    import Card from './Card.svelte';
    import Column from './Column.svelte';
    import DraggableList from './DraggableList.svelte';
    import List from './List.svelte';
    import Modal from './Modal.svelte';

    import { Icon } from '@steeze-ui/svelte-icon';
    import { Plus, Trash } from '@steeze-ui/heroicons';

    const flipDurationMs = 300;

    // column items/data
    export let data;

    // will be called anytime a card/column gets dropped to update parent data
    export let onFinalUpdate;

    // search value for the pool
    let poolSearch = '';
    let selectedView = '';
    let showModal = false;

    // used to add a new view
    let name = '';
    let filter = '';

    $: population = data.pool.length + data.strats.length;
    $: topHalf = population / 2;
    $: topThird = population / 3;

    function addToStrat(item) {
        const idx = data.pool.indexOf(item);
        if (idx > -1) {
            data.pool.splice(idx, 1);
            data.strats.push(item);
        }

        onFinalUpdate(data);
    }

    function removeFromStrat(item) {
        const idx = data.strats.indexOf(item);
        if (idx > -1) {
            data.strats.splice(idx, 1);
            data.pool.push(item);
        }

        onFinalUpdate(data);
    }

    function handlePoolFinalize(pool) {
        data.pool = pool;
        onFinalUpdate(data);
    }

    function handleStratsFinalize(strats) {
        data.strats = strats;
        onFinalUpdate(data);
    }

    function addSecondLevelView() {
        data.views.push({name: name, filter: filter});
        name = '';
        filter = '';
        onFinalUpdate(data);

        showModal = false;
    }

    function filterSecondLevel(item) {
        return tem.search(new RegExp(selectedView, "i")) > -1;
    }
</script>

<section class="w-full flex-1 h-full flex p-2 gap-x-4">
    <Modal title="Add Second Level Strat Category" show={showModal}>
        <div class="flex flex-col gap-y-2">
            <input type="text" bind:value={name} class="p-1 border border-gray-300 w-full" placeholder="Filter name (e.g. Team Leads, DOs, etc.)">
            <input type="text" bind:value={filter} class="p-1 border border-gray-300 w-full" placeholder="Enter filter / category (e.g., team lead, do, etc)">
        </div>
        <svelte:fragment slot="actions">
            <button
                type="button"
                class="inline-flex w-full justify-center rounded-md bg-blue-600 px-3 py-2 text-sm font-semibold text-white shadow-sm hover:bg-blue-500 sm:ml-3 sm:w-auto"
                on:click={addSecondLevelView}
            >
                Add
            </button>
            <button
                type="button"
                class="mt-3 inline-flex w-full justify-center rounded-md bg-white px-3 py-2 text-sm font-semibold text-gray-900 shadow-sm ring-1 ring-inset ring-gray-300 hover:bg-gray-50 sm:mt-0 sm:w-auto"
                on:click={() => showModal = false}
            >
                Cancel
            </button>
        </svelte:fragment>
    </Modal>

    <Column name="Pool">
        <div slot="action" class="p-1 mb-1 border-b h-12">
            <input
                type="text"
                class="w-full border border-gray-200 rounded-md p-1"
                placeholder="Search..."
                bind:value={poolSearch}
            />
        </div>
        <List
            items={data.pool}
            filter={item => item.name.toLowerCase().includes(poolSearch.toLowerCase())}
            let:item
            let:idx
        >
            <Card item={item}>
                <button
                    slot="actions"
                    class="p-2 w-12 h-full flex items-center justify-center hover:bg-blue-200"
                    on:click={() => addToStrat(item)}
                >
                    <Icon src={Plus} size="20px" theme="outline" />
                </button>
            </Card>
        </List>
    </Column>

    <Column name='Primary Stratification'>
        <div slot="action" class="p-1 mb-1 border-b h-12 grid grid-cols-3 gap-x-2 text-center items-center divide-x divide-gray-300">
            <div>
                <span class="text-xs">Population:</span>
                <span class="text-sm">{population}</span>
            </div>
            <div>
                <span class="text-xs">Top Half:</span>
                <span class="text-sm">{topHalf}</span>
            </div>
            <div>
                <span class="text-xs">Top Third:</span>
                <span class="text-sm">{topThird}</span>
            </div>
        </div>
        <DraggableList items={data.strats} onFinalize={handleStratsFinalize} let:item let:idx>
            <Card item={item} idx={idx + 1}>
                <button
                    slot="actions"
                    class="p-2 w-12 h-full flex items-center justify-center hover:bg-red-200"
                    on:click={() => removeFromStrat(item)}
                >
                    <Icon src={Trash} size="20px" theme="outline" class="text-red-600" />
                </button>
            </Card>
        </DraggableList>
    </Column>

    <Column name='Second Level Startification'>
        <div slot="action" class="p-1 mb-1 border-b w-full flex justify-between gap-x-2 h-12">
            {#if data.views.length > 0}
                <select id="secondLevelStrat" class="border rounded-md p-1 w-full" bind:value={selectedView}>
                    <option disabled value="">Select a Category</option>
                    {#each data.views as view}
                        <option value={view.filter}>{view.name}</option>
                    {/each}
                </select>
                <button class="px-3 py-1 border rounded-md hover:bg-orange-500" on:click={() => showModal = true}>Add</button>
            {/if}
        </div>
        {#if data.views.length > 0}
            <List
                items={data.strats}
                filter={item => item.title.toLowerCase().includes(selectedView.toLowerCase())}
                let:item
                let:idx
            >
                <Card item={item} idx={idx + 1} />
            </List>
        {:else}
            <div class="h-full flex justify-center items-center">
                <button
                    class="flex flex-col border border-gray-300 px-3 py-4 rounded-md items-center hover:bg-blue-200"
                    on:click={() => showModal = true}
                >
                    <Icon src={Plus} size="24px" theme="outline" />
                    <span class="text-md">Add Category</span>
                </button>
            </div>
        {/if}
    </Column>
</section>
