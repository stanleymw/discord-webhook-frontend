<script>
  import Main from './main.svelte';

  let webhook_id = "";
  $: real = webhook_id.trim();

  let success = false;
  
  async function getWebhook(url) {
    if (!url.startsWith("https://discord.com/api/webhooks/")) {
      success = false
      throw new Error("Must be a Discord Webhook URL!")
    }

    const res = await fetch(url);
    if (res.ok) {
      success = true;
      return res.json();
    }
    success = false;
    throw new Error('Request Failed');
  };

  $: prom = getWebhook(real);

</script>

<div class="flex-container">
  <h1>Stanley's Discord Webhook Frontend</h1>

  <label id="webhook_box">
    <input type="string" bind:value={webhook_id} placeholder="https://discord.com/api/webhooks/xxxxxxxxxxxxxxx/xxxxxxxxxxxxxxxxxxxxxxxx">
  </label>
</div>

{#await prom}
  <code>Resolving webhook...</code>
{:then ret}
  <code>Webhook resolved.</code>
  <div>
    <Main webhook_obj={ret}/>
  </div>
{:catch}
  <code>Error in resolving webhook, which may be due to an invalid webhook URL. Check the URL</code>
{/await}

<style>
  .flex-container {
    display: flex;
    flex-direction: column;
    align-items: center;
  }

  h1 {
    text-align: center;
  }

  #webhook_box {
    width: 100%;
  }

  input {
    width: 95%;
  }
</style>
