<script>
    export let title;
    export let show = false;
    export let size = 'xl';

    function getSize() {
        switch (size) {
            case 'md':
                return 'max-w-md';
            case 'lg':
                return 'max-w-lg';
            case 'xl':
                return 'max-w-xl';
            case '2xl':
                return 'max-w-2xl';
            case '3xl':
                return 'max-w-3xl';
            case '4xl':
                return 'max-w-4xl';
            default:
                return 'max-w-sm';
        }
    }
</script>
<div class="relative z-10" class:hidden={!show} aria-labelledby="add-second-level-strat" role="dialog" aria-modal="true">
    <div class="fixed inset-0 bg-slate-500 bg-opacity-75 transition-opacity"></div>
    <div class="fixed inset-0 z-10 w-screen overflow-y-auto">
        <div class="flex min-h-full items-end justify-center p-4 text-center sm:items-center sm:p-0">
            <div class={`relative transform overflow-hidden rounded-lg bg-white text-left shadow-xl transition-all sm:my-8 sm:w-full ${getSize()}`}>
                <div class="bg-white px-4 pb-4 pt-5 sm:p-6 sm:pb-4">
                    <div class="sm:flex sm:items-start">
                        <div class="mt-3 text-center sm:ml-4 sm:mt-0 sm:text-left w-full">
                            <h3 class="text-2xl text-center font-semibold leading-6 text-slate-900 border-b pb-4" id="modal-title">{title}</h3>
                            <div class="mt-2 w-full">
                                <slot></slot>
                            </div>
                        </div>
                    </div>
                </div>
            </div>
        </div>
    </div>
</div>
