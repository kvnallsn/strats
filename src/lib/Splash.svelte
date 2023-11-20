<script>
    import { onMount } from 'svelte';
    import { saveAs } from 'file-saver';
    import { parse } from 'csv-parse/browser/esm/sync';

    import Button from './Button.svelte';
    import Modal from './Modal.svelte';
    import { Icon } from '@steeze-ui/svelte-icon';
    import { ArrowTopRightOnSquare, ArrowDownTray, ArrowUpTray, Trash } from '@steeze-ui/heroicons';

    export let show = false;
    export let onCreate;
    export let onOpen;

    let sessions = [];
    let sessionName = '';
    let sessionPool = '';

    let showDeleteDialog = false;
    let deleteSessionName = '';

    onMount(() => {
        try {
            const savedSessions = localStorage.getItem('sessions');
            if (savedSessions) {
                sessions = JSON.parse(savedSessions);
            }
        } catch (error) {
            // failed to load local storage, leave as an empty array
            console.error('failed to load from local storage', error);
        }
    });

    /**
     * Saves the list of sessions to local storage
     */
    function saveSessionList(name) {
        // save this session into the sessions list
        sessions = [...sessions, name];
        localStorage.setItem('sessions', JSON.stringify(sessions));
    }

    /**
     * Creates a new session, then calls the onCreate callback
     */
    async function createSession() {
        const file = sessionPool[0];
        if (!file) {
            console.log('no pool selected!');
            return;
        }

        if (!sessionName) {
            console.log('no session name set!');
            return;
        }
        
        const text = await file.text();
        const pool = parse(text, {
            columns: true,
            skip_empty_lines: true
        });

        saveSessionList(sessionName);
        onCreate(sessionName, pool);

        // clear the fields
        sessionName = '';
        sessionPool = '';
    }

    /**
     * Downloads the JSON data for a stratification session
     */
     function downloadSession(session) {
        const data = localStorage.getItem(session);
        const filename = session.toLowerCase().replaceAll(' ', '_');
        if (data) {
            saveAs(new Blob([data]), `${filename}.json`);
        } else {
            console.warn('unable to load session', session);
        }
     }

     /**
      * Uploads an session to this application
      */
     async function uploadSession(e) {
        const file = e.target.files[0];
        if (file === null) {
            // user cancelled the dialog
            return;
        }

        let name = file
            .name
            .replace(/_([a-z])/g, c => ` ${c[1].toUpperCase()}`)
            .replace(/\.json/g, '');

        if (sessions.includes(name)) {
            name += ` (${new Date().toLocaleString()})`;
        }

        localStorage.setItem(name, await file.text());
        saveSessionList(name);
     }

     /**
      * Deletes a session from the local storage
      */
     function deleteSession(name) {
        localStorage.removeItem(name);
        const idx = sessions.indexOf(name);
        sessions.splice(idx, 1);
        sessions = [...sessions];
        localStorage.setItem('sessions', JSON.stringify(sessions));

        closeDeleteModal();
     }

     /**
      * Deletes a session from the local storage.  This is irreversable!
      */
     function showDeleteModal(session) {
        deleteSessionName = session;
        showDeleteDialog = true;
     }


     /**
      * Closes the delete dialog and clears any settings
      */
     function closeDeleteModal() {
        deleteSessionName = '';
        showDeleteDialog  = false;
     }
</script>

<Modal title="Stratification Tool" {show}>
    <div class="grid grid-cols-2 divide-x divide-slate-300">
        <div class="flex flex-col pr-2">
            <div class="flex items-center border-b border-slate-300 h-10">
                <h2 class="text-md w-full text-center">Open Session</h2>
                <label for="upload-session" class=" cursor-pointer p-1 hover:text-primary-600">
                    <Icon src={ArrowUpTray} size="20px" theme="outline" class="hover:text-primary-600" />
                    <input id="upload-session" type="file" class="sr-only" on:change={uploadSession}>
                </label>
            </div>
            <div class="py-4">
                <ul class="divide-y divide-slate-300">
                {#each sessions as session}
                    <li
                        class="flex p-1 hover:bg-slate-200 hover:rounded-md flex cursor-pointer justify-between items-center"
                        on:dblclick={() => onOpen(session)}
                    >
                        <p class="text-sm leading-6">{session}</p>
                        <div class="flex items-center">
                            <button class="hover:text-indigo-600 p-1" on:click={() => onOpen(session)}>
                                <Icon src={ArrowTopRightOnSquare} size="20px" theme="outline" />
                            </button>
                            <button class="hover:text-indigo-600 p-1" on:click={() => downloadSession(session)}>
                                <Icon src={ArrowDownTray} size="20px" theme="outline" />
                            </button>
                            <button class="hover:text-red-600 p-1" on:click={() => showDeleteModal(session)}>
                                <Icon src={Trash} size="20px" theme="outline" />
                            </button>
                        </div>
                    </li>
                {:else}
                    <div class="text-center italic text-slate-500">No Sessions Found</div>
                {/each}
                </ul>
            </div>
        </div>
        <div class="flex flex-col pl-2">
            <div class="flex items-center border-b border-slate-300 h-10">
                <h2 class="text-md w-full text-center">Create Session</h2>
            </div>
            <div class="py-4">
                <label for="templates" class="block text-sm font-medium leading-6 text-slate-900">Session Name</label>
                <div class="mt-2">
                    <div class="flex rounded-md shadow-sm ring-1 ring-inset ring-slate-300 sm:max-w-md">
                        <input
                            type="text"
                            bind:value={sessionName}
                            class="block flex-1 border-0 bg-transparent py-1.5 pl-1 text-slate-900 placeholder:text-slate-400 focus:ring-0 sm:text-sm sm:leading-6"
                            placeholder="e.g., 2023 Lt Strats"
                        >
                    </div>
                </div>

                <label for="templates" class="block text-sm font-medium leading-6 text-slate-900 mt-4">Stratification Pool</label>
                <div class="mt-2">
                    <div class="flex rounded-md shadow-sm ring-1 ring-inset ring-slate-300 sm:max-w-md">
                        <input accept="text/csv" id="upload-pool" type="file" bind:files={sessionPool} />
                    </div>
                </div>
            </div>
            <Button onClick={() => createSession()}>Create</Button>
        </div>
    </div>
</Modal>

<Modal title="Confirm Delete" size="xl" show={showDeleteDialog}>
    <div>
        <div class="rounded-md border-2 border-red-500 bg-red-300 mb-4">
            <p class="p-1">This action cannot be undone!</p>
        </div>
        <p>Are you sure you want to delete session {deleteSessionName}?</p>
    </div>
    <div class="mt-6 flex items-center justify-end gap-x-3">
        <button
            type="button" class="text-sm font-semibold leading-6 text-slate-900 mr-2"
            on:click={() => closeDeleteModal()}
        >
            Cancel
        </button>
        <Button onClick={() => downloadSession(deleteSessionName)}>Download</Button>
        <Button color="red" onClick={() => deleteSession(deleteSessionName)}>Delete</Button>
    </div>
</Modal>
