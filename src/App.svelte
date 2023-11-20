<script lang="ts">
    // Components
    import Board from './lib/Board.svelte';
    import Button from './lib/Button.svelte';
    import Splash from './lib/Splash.svelte';

    let data = initData('');
    let sessions = [];
    let showSplash = true;
    let sessionName = '';

    function initData(session) {
        return {
            session: session,
            pool: [],
            strats: [],
            views: [],
        };
    }

    function createSession(session, pool) {
        sessionName = session;
        handleBoardUpdated({
            session: session,
            pool: pool,
            strats: [],
            views: []
        });
        showSplash = false;
    }

    function loadSession(session) {
        sessionName = session;
        const store = localStorage.getItem(session);
        if (store) {
            try {
                data = JSON.parse(store);
                showSplash = false;
            } catch (error) {
                console.error('failed to load session', error);
            }
        }
    }

    function handleBoardUpdated(newData) {
        if (newData) {
            data = newData;
            save();
        }
    }

    function save() {
        localStorage.setItem(sessionName, JSON.stringify(data));
    }

    function close() {
        save();
        reset();
        sessionName = '';
        showSplash = true;
    }

    function reset() {
        data = initData('');
    }
</script>

<header class="fixed top-0 w-full h-12 text-white bg-gray-800 border-b border-gray-500 flex justify-between items-center px-2">
    <div class="flex items-center">
        <h2 class="text-xl">Stratification Tool</h2>
        <h4 class="ml-4 text-md italic">{sessionName}</h4>
    </div>
    <div class="flex gap-x-2">
        <Button onClick={save}>Save</Button>
        <Button color="red" onClick={close}>Close</Button>
    </div>
</header>
<main class="pt-12 max-h-full flex flex-col bg-white dark:bg-black">
    <Splash
        show={showSplash}
        onCreate={createSession}
        onOpen={loadSession}
    />
    {#if !showSplash}
        <Board data={data} onFinalUpdate={handleBoardUpdated} />
    {/if}
</main>
