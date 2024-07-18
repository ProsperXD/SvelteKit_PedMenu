<script lang="ts">
    import { writable } from 'svelte/store';
  
    const MyId = writable<number>(3);
    const my_name = writable<string>('Prosper');
    const PedMenu = writable<boolean>(true);
    let peds = [
      { name: 'Black Panther', label: 'Black Panther', img: '/7229-2.png', owner: 3, owner_name: 'Prosper' },
      { name: 'White Tiger', label: 'White Tiger', img: '/81574_PinkPanther_POP_GLAM_1-WEB.png', owner: 'None', RedeemCode: 'CodeForWhiteTigga', owner_name: 'No One' },
      { name: 'Spider Man', label: 'Spider Man', img: '/76676_POPGames_SpiderMan2EXPOP04_GLAM-1-EE-WEB.png', owner: 'None', RedeemCode: 'CodeForSpiderMan', owner_name: 'No One' },
    ];
  
    let currentPedIndex = 0;
    let redeemToken = '';
  
    $: peds[currentPedIndex].owner_name = peds[currentPedIndex].owner === 'None' ? 'No One' : (peds[currentPedIndex].owner === $MyId ? $my_name : peds[currentPedIndex].owner_name);
  
    function previousPed() {
      currentPedIndex = (currentPedIndex - 1 + peds.length) % peds.length;
      redeemToken = '';
    }
  
    function nextPed() {
      currentPedIndex = (currentPedIndex + 1) % peds.length;
      redeemToken = '';
    }
  
    function handleRedeem() {
      if (peds[currentPedIndex].RedeemCode === redeemToken) {
        peds[currentPedIndex].owner = $MyId;
        peds[currentPedIndex].owner_name = $my_name;
        redeemToken = '';
      } else {
        console.log('Invalid redeem code');
      }
    }
    function Close (){
        PedMenu.set(!$PedMenu);
    }
    function Select(){
        alert(`Selected Ped: ${peds[currentPedIndex].label}`);
    }
</script>

{#if $PedMenu}
  <div class="ped bg-gray-900 text-white rounded-lg shadow-lg w-96 p-6 fixed transform -translate-x-1/2 -translate-y-1/2 left-1/2 top-1/2">
    <div class="flex justify-between items-center border-b border-gray-700 pb-4 mb-4">
      <h2 class="text-xl font-semibold text-orange-400">PED Menu</h2>
      <button on:click={Close} class="text-gray-500 hover:text-white">Close</button>
    </div>
    <div class="text-center">
      <img src={peds[currentPedIndex].img} alt={peds[currentPedIndex].label} class="mx-auto w-32 h-32 mb-4">
      <h3 class="text-2xl font-bold">{peds[currentPedIndex].name} <span class="text-orange-400"></span></h3>
      <p class="text-gray-400">Owned by {peds[currentPedIndex].owner_name}</p>
    </div>
    <div class="flex justify-between items-center my-4">
      <button class="text-gray-500 hover:text-white" on:click={previousPed}>
        <svg xmlns="http://www.w3.org/2000/svg" fill="currentColor" class="bi bi-arrow-left-circle w-8 h-8" viewBox="0 0 16 16">
          <path fill-rule="evenodd" d="M8 0a8 8 0 1 1-6.546 12.854l-.636.77a.5.5 0 1 1-.75-.661l.636-.771A8 8 0 1 1 8 0zM8 1a7 7 0 1 0 0 14A7 7 0 0 0 8 1zm3.354 4.354a.5.5 0 0 1 0 .707L8.707 8.707l2.647 2.647a.5.5 0 0 1-.708.708L8 9.415l-2.647 2.647a.5.5 0 0 1-.708-.708L7.293 8.707 4.646 6.06a.5.5 0 1 1 .708-.707L8 8.293l2.646-2.646a.5.5 0 0 1 .708 0z"/>
        </svg>
      </button>
      <button class="text-gray-500 hover:text-white" on:click={nextPed}>
        <svg xmlns="http://www.w3.org/2000/svg" fill="currentColor" class="bi bi-arrow-right-circle w-8 h-8" viewBox="0 0 16 16">
          <path fill-rule="evenodd" d="M8 0a8 8 0 1 1-6.546 12.854l-.636.77a.5.5 0 1 1-.75-.661l.636-.771A8 8 0 1 1 8 0zM8 1a7 7 0 1 0 0 14A7 7 0 0 0 8 1zm3.354 4.354a.5.5 0 0 1 0 .707L8.707 8.707l2.647 2.647a.5.5 0 0 1-.708.708L8 9.415l-2.647 2.647a.5.5 0 0 1-.708-.708L7.293 8.707 4.646 6.06a.5.5 0 1 1 .708-.707L8 8.293l2.646-2.646a.5.5 0 0 1 .708 0z"/>
        </svg>
      </button>
    </div>
  
    {#if peds[currentPedIndex].owner === 'None'}
      <div class="mt-4">
        <input type="text" class="w-full bg-gray-700 text-white py-2 rounded-lg mb-2 px-4" placeholder="Enter redeem token" bind:value={redeemToken}>
        <button class="w-full bg-orange-500 text-white py-2 rounded-lg hover:bg-orange-600" on:click={handleRedeem}>Redeem</button>
      </div>
    {/if}
  
    {#if peds[currentPedIndex].owner === $MyId}
      <button on:click={Select} class="w-full bg-orange-500 text-white py-2 rounded-lg mb-2 hover:bg-orange-600">Select</button>
    {/if}
  
    <button class="w-full bg-gray-700 text-white py-2 rounded-lg hover:bg-gray-800">Revert To Default</button>
    <p class="text-gray-400 text-center mt-4">Ped Available<br>You have access to this ped, you can become it at any time by clicking select</p>
  </div>
{/if}