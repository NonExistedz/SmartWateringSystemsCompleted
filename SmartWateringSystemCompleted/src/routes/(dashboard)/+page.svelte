<script>
  import { ref, onValue } from 'firebase/database'
  import { onMount } from 'svelte';

  import database from '$lib/firebase'
  
  const wateringSystemRef = ref(database, "/");

  let data = [];
  let humidity = 90;
  let date = new Date().toLocaleString('en-us');

  setInterval(() => {
    date = new Date().toLocaleString('en-us');
  }, 1000);

  onValue(wateringSystemRef, (snapshot) => {
    let temp = snapshot.val();
    let keys = Object.keys(temp)
    data = []
    keys.forEach((key) => {
      data.unshift(temp[key])
    })
    console.log(data)
  })
</script>
<!-- -------------------------------------------Ead Head------------------------ -->

<div class="flex flex-col p-2 h-full">
  <div class="flex text-2xl gap-2">
    <span class="bg-[#181C25] p-5">
      {#if data.length != 0}
        <span>Status: {data[0].Status}</span>
      {/if}
    </span>
    <span class="bg-[#181C25] p-5">
      {date}
    </span>
  </div>
  <div class="mt-2 text-xl bg-[#181C25] h-full p-2 flex flex-col"> 
    <span>
      History
    </span>
    <div class="flex flex-col max-h-full divide-y">
      {#each data as d }
        <span class="p-2">{new Date(d.Time).toLocaleString('en-us')} {d.Status}</span>
      {/each}
    </div>
  </div>
</div>