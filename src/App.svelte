<script lang="ts">
    import Board from './lib/Board.svelte';

  let data = [
    {
        id: "c1",
        name: "People",
        items: [
            {id: 1, name: "Alice", title: "DO"},
            {id: 2, name: "Bob", title: "DO"},
            {id: 3, name: "Charlie", title: "Team Lead"},
            {id: 4, name: "Eve", title: "Team Lead"}
        ]
    },
    {
        id: "c2",
        name: "Stratification",
        items: [
            {id: 5, name: "Harry", title: "DO"}
        ]
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
<main class="h-full grow flex flex-col">
   <Board columns={data} onFinalUpdate={handleBoardUpdated} />
</main>
