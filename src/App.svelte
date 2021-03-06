<script>
  import { onMount } from 'svelte'
  // import {CHANNEL_STATES} from '@svelte/realtime-js'
  import { createClient } from '@supabase/supabase-js'

  import TailwindStyles from './TailwindStyles.svelte'
  import Login from './Login.svelte'
  import LoggedIn from './LoggedIn.svelte'
  let importEnv = true
  try {
    if (process.env.NODE_ENV === 'test') importEnv = false
  } catch (error) {}

  const { SNOWPACK_PUBLIC_SUPABASE_URL, SNOWPACK_PUBLIC_SUPABASE_KEY } = !importEnv
    ? process.env
    : import.meta.env
  const supabase = createClient(SNOWPACK_PUBLIC_SUPABASE_URL, SNOWPACK_PUBLIC_SUPABASE_KEY, { detectSessionInUrl: true })
  let user
  onMount(async () => {
    setInterval(() => {
      if (supabase.auth.session()) {
        user = supabase.auth.session()
      }
    }, 500)
  })
</script>

{#if user}
  <LoggedIn bind:user supabase={supabase} />
{:else}
  <Login bind:user supabase={supabase} />
{/if}
