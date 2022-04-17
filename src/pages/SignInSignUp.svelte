<script>
  import {writable} from 'svelte/store';
	import {navigate} from "svelte-routing";

  let pageName = "SignIn/SignUp";
  let username = '';
  let password = '';
  let secret = '';
  let pageState = new URL(location.href).searchParams.get('state')
  let showErrorBanner = writable(false);

  const onFieldChange = () => {
    showErrorBanner.set(false);
  }

  const onSubmit = () => {
    // TODO: hash password
    if (pageState === 'up') {
      fetch('http://127.0.0.1:8080/api/create-user', {
        method: 'POST',
        headers: {
          'Content-Type': 'application/json'
        },
        body: JSON.stringify({username, password, secret})
      }).then(x => {
        if(x.status === 201) {
          navigate('/');
          return;
        }

        if(x.status === 403) {
          showErrorBanner.set(true)
        }
      })
    } else {
      fetch('http://127.0.0.1:8080/api/login', {
        method: 'POST',
        headers: {
          'Content-Type': 'application/json'
        },
        body: JSON.stringify({username, password})
      }).then(x => {
        if(x.status === 403) {
          showErrorBanner.set(true)
        }
        console.log(getHeaders(x.headers));
      }).catch(err => {
        console.log('CATCH');
      })
    }
  }
</script>

<main class="main-container">
  <div>
    <h2><span>Sing In</span> / <span>Sing Up</span></h2>
  </div>
  <form on:submit|preventDefault={onSubmit} class="form">
    <input type="text" required placeholder="Username" bind:value={username} on:change={onFieldChange}/>
    <input type="password" required placeholder="Password" bind:value={password} on:change={onFieldChange}/>
    {#if pageState == 'up'}
      <input type="text" placeholder="Secret key" bind:value={secret} on:change={onFieldChange}/>
    {/if}
    <button type="submit" disabled={$showErrorBanner}>Submit</button>
  </form>
  {#if $showErrorBanner}
    <div class="error-banner">Access Denied</div>
  {/if}
</main>
