<script>
import { onMount } from "svelte";
import { writable } from "svelte/store";

  let partnersList = writable([]);


  onMount(async () => {
    try {
      const response = await fetch(`${API_BASE_URL}/api/partners`);
      const list = await response.json();
      console.log(list);

      partnersList.set(list);
    } catch(error) {
      console.log(error);
    }
  })
</script>

<main>
    {#if $partnersList.length}
      <table>
        <tr>
          {#each Object.keys($partnersList[0]) as keys}
          <th>{keys}</th>
          {/each}
        </tr>
        {#each $partnersList as partner}
          <tr>
            {#each Object.values(partner) as value}
              <td>{value}</td>
            {/each}
              <td>
                <button>delete</button>
              </td>
          </tr>
        {/each}
      </table>
    {/if}
</main>