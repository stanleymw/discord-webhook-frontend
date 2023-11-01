<script>
  export let webhook_obj = {};

  let msg = "";
</script>

<div class="main-form">
  <code>
    Webhook Name: {webhook_obj['name']} <br>
    Server ID: {webhook_obj['guild_id']} <br>
    Channel ID: {webhook_obj['channel_id']} <br>
  </code>

  <form>
    <input type="text" placeholder="Enter a message to send..." bind:value={msg}>

    <button on:click={() => {
      fetch(webhook_obj['url'], {
        method: "POST",

        headers: {
          "Content-type": "application/json; charset=UTF-8"
        },

        body: JSON.stringify({
          content: msg
        })
      })
    }}>Send Message</button>
  </form>
</div>

<style>
  .main-form {
    display: flex;
    flex-direction: column;
    align-items: center;
  }
</style>
