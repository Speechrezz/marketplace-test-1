<script>
  import { browser } from '$app/environment'
	import { enhance } from '$app/forms';
  import { supabaseClient } from '$lib/supabase'
  import { goto } from '$app/navigation'
  import { fail, redirect } from '@sveltejs/kit';

  let formEmail, formPassword;

  const submitRegister = async ({ cancel }) => {
    console.log("Submit register")
    const { error } = await supabaseClient.auth.signUp({
      email: formEmail,
      password: formPassword
    });
    if (error){
      console.log(error)
      cancel();
      return;
    }
    if (!browser) throw redirect(303, "/")
    else goto("/")
  }
</script>

<main class='w-full h-screen flex flex-col align-middle justify-center pb-16 bg-slate-100'>
  <div class='bg-white flex flex-col mx-auto p-8 pt-12 rounded-lg shadow-sm'>
    <h1 class='text-xl font-medium mx-auto text-slate-900'>Create an account</h1>
    <form action="?/register" method="POST" use:enhance={submitRegister}
      class='flex flex-col align-middle mx-auto mt-8 w-80'>
      <label for="email" class="text-slate-500">Email</label>
      <input type="email" name='email' bind:value={formEmail}>
      <label for="password" class='mt-4 text-slate-500'>Password</label>
      <input type="password" name='password' bind:value={formPassword}>
      <button type='submit' class='btn btn-primary mt-8'>Sign up</button>
    </form>
  </div>
  <div class="bg-slate-200 flex flex-col mx-auto py-8 px-8 mt-8 rounded-lg shadow-sm">
    <span class='w-80 text-slate-600 mx-auto text-center'>Already have an account? 
      <a href="/login" class='text-slate-900 font-medium hover:underline'>Sign In</a>
    </span>
  </div>
</main>