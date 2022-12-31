<script>
	import { enhance } from '$app/forms';
  import { supabaseClient } from '$lib/supabase'
  export let data;

  const submitLogout = async ({ cancel }) => {
    console.log("Submit logout")
    const { error } = await supabaseClient.auth.signOut();
    if (error){
      console.log(error)
    }
    cancel();
  }
</script>

<main class='flex flex-col justify-center align-middle w-full h-screen bg-slate-100 pb-16'>
  <div class='flex flex-col align-middle mx-auto bg-white p-12 pt-16 rounded-lg shadow-sm'>
    <h1 class='text-2xl font-medium mx-auto'>Welcome to SvelteKit</h1>
    {#if data.session}
      <p class='mx-auto mt-2 text-slate-600'>Welcome, {data.session.user.email}</p>
      <form action="/logout" method="POST" use:enhance={submitLogout} class='flex justify-center mt-16'>
        <button type='submit' class='mt-4 mx-auto btn btn-primary'>Logout</button>
      </form>
    {:else}
    <p class='mx-auto mt-2 text-slate-600'>Visit 
      <a class='font-medium text-slate-900' href="https://kit.svelte.dev">kit.svelte.dev</a> 
      to read the documentation</p>
    <div class='mx-auto mt-16 flex space-x-4'>
      <a href="/register" class='btn btn-secondary'>Register</a>
      <a href="/login" class='btn btn-primary'>Login</a>
    </div>
    {/if}
  </div>
</main>