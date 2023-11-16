<script lang="ts">
    import Board from './lib/Board.svelte';

  let data = [
    {
        id: "c1",
        name: "People",
        items: [
            {id: 1, name: "Alice"},
            {id: 2, name: "Bob"},
            {id: 3, name: "Charlie"},
            {id: 4, name: "Eve"}
        ]
    },
    {
        id: "c2",
        name: "Stratification",
        items: []
    }
  ];

  function handleBoardUpdated(newData) {
    // TODO update to server/etc. here
    data = newData;
  }
</script>

<header class="w-full h-12 text-white bg-gray-800 border-b border-gray-500 flex justify-between items-center px-2">
    <h2 class="text-xl">Stratification Tool</h2>
</header>
<main>
   <Board columns={data} onFinalUpdate={handleBoardUpdated} />
</main>
