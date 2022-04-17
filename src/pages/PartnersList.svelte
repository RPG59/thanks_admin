<script>
import { onMount } from "svelte";
import { writable } from "svelte/store";

  let partnersList = writable([]);


  onMount(() => {
    fetch(`${API_BASE_URL}/api/partners`).then(x => x.json()).then(list => {
      partnersList.set(list);
    })
  })
</script>

<main>
  <ul>
    {#each $partnersList as partner}
      <li>{partner.title}</li>
    {/each}
  </ul>
</main>