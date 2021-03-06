<script>
  import LoginLists from './LoginLists.svelte'
  import { items } from './items'
  export let user
  export let supabase
  let lists
  $: lists = items()
</script>

<LoginLists user_id="{user.id}" lists="{lists}" />
<button
  class="w-full flex items-center justify-center px-8 py-3 border border-transparent text-base leading-6 font-medium rounded text-green-700 border-green-500 hover:text-green-500 hover:bg-green-50 focus:outline-none focus:shadow-outline focus:border-green-300 transition duration-150 ease-in-out md:py-4 md:text-lg md:px-10 float-left"
  on:click="{() => {
    supabase.auth
      .signOut()
      .then((response) => {
        document.querySelector('#access-token').value = ''
        document.querySelector('#refresh-token').value = ''
        user = null
        localStorage.removeItem('user-todolist')
        alert('Logout successful')
      })
      .catch((err) => {
        console.log(err)
        alert(JSON.stringify(err))
      })
      .finally(() => {
        window.location.reload();
      })

  }}"
>Logout</button>
