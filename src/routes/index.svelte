<script context="module">
  import { browser } from '$app/env'
  import { onMount } from 'svelte'

  // /** @type {import('@sveltejs/kit').Load} */
  export async function load({ page, fetch, session, stuff }) {
    console.log('onLoad...')
    console.log(page)
    console.log(fetch)
    console.log(session)
    console.log(stuff)

    if (browser) {
      window.solana.on('connect', () => console.log('wallet connected!'))
    }
    return {}
  }
</script>

<script>
  let isPhantomInstalled = false

  if (browser) {
    isPhantomInstalled = window.solana && window.solana.isPhantom
    console.log(`isPhantomInstalled? : ${isPhantomInstalled}`)
  }

  onMount(() => {
    console.log('onMount...')
  })

  async function connectWallet() {
    try {
      const resp = await window.solana.connect({ onlyIfTrusted: true })
      console.log(window.solana)
      console.log(resp)
      resp.publicKey.toString()
    } catch (err) {
      // { code: 4001, message: 'User rejected the request.' }
      console.error('User rejected the connect request')
      console.error(err)
    }
  }
</script>

<section class="flex bg-gray-100 justify-around">
  <div class="mt-2">
    <h1>Welcome to Polyglot</h1>
    <p>
      Visit <a href="https://kit.svelte.dev">kit.svelte.dev</a> to read the documentation
    </p>

    {#if isPhantomInstalled}
      <p class="text-green-700">Wallet is installed! You can move ahead!</p>
    {:else}
      <p class="text-red-700">Unable to find the wallet extention</p>
    {/if}
  </div>

  <div class="mt-2">
    {#if isPhantomInstalled}
      <p class="p-4 bg-blue-900 rounded-lg" on:click={connectWallet}>Connect Wallet</p>
    {:else}
      <p>No wallet detected</p>
    {/if}
  </div>
</section>
