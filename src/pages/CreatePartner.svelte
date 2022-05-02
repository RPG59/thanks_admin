<script>
import { onDestroy } from "svelte";

import { writable } from "svelte/store";


  let pageName = "CreatePartner";
  let title = '';
  let description = '';
  let logo = '';
  let discount = '';
  let showSuccessBanner = writable(false);
  let image = null;
  let _timerDescriptor = null;

  onDestroy(() => {
    clearTimeout(_timerDescriptor);
  })

  const onChangeForm = () => {
    showSuccessBanner.set(false);
  }

  const clearForm = () => {
    title = '';
    logo = '';
    description = '';
    discount =  '';
  }
  
  const onSubmit = () => {
    const formData = new FormData();

    formData.append("title", title);
    formData.append("description", description);
    formData.append("discount", discount);
    formData.append("logo", image.files[0]);

    console.log(formData.keys());


    fetch(`${API_BASE_URL}/api/create-partner`, {
      method: "POST",
      // headers: {
      //   'Content-Type': 'multipart/form-data'
      // },
      body: formData
        // headers: {
        //   'Content-Type': 'application/json'
        // },
        // body: JSON.stringify({
        //   title,
        //   description,
        //   discount,
        //   logo
        // })
    }).then(res => {
      if(res.ok) {
        showSuccessBanner.set(true);
        clearForm();

        _timerDescriptor = setTimeout(() => {
          showSuccessBanner.set(false);
        }, 1000);
      }
    })
  }
</script>

<main class="main-container">
  <h1>Create partner</h1>
<form class="form" on:submit|preventDefault={onSubmit} on:change={onChangeForm}>
  <input required placeholder="title" type="text" bind:value={title}>
  <input required placeholder="discount" type="text" bind:value={discount}>
  <input required placeholder="logo" type="text" bind:value={logo}>
  <input placeholder="description" type="text" bind:value={description}>
  <input type="file" placeholder="image" accept="image/*" bind:this={image}>
  <button type="submit">Submit</button>
</form>
{#if $showSuccessBanner}
  <div class="success-banner">Partner Created</div>
{/if}
</main>