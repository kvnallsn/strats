<script>
    export let onClick;
    export let color = 'indigo'

    function getColor() {
        switch (color) {
        case 'red':
            return 'bg-red-600 hover:bg-red-500 focus-visible:outline-red-600 focus:bg-red-400';
        default:
            return 'bg-indigo-600 hover:bg-indigo-500 focus-visible:outline-indigo-600 focus:bg-indigo-400';
        }
    }
</script>

<button
    type="button"
    class={`${getColor()} rounded-md px-3 py-2 text-sm font-semibold text-white shadow-sm focus-visible:outline focus-visible:outline-2 focus-visible:outline-offset-2`}
    on:click={onClick}
>
    <slot></slot>
</button>
