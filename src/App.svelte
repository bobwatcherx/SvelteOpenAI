<script>
  import {Configuration,OpenAIApi} from 'openai'
  const config = new Configuration({
 apiKey:import.meta.env.VITE_OPEN_AI_KEY

  })
  const openapi = new OpenAIApi(config)
const genchat = async(msg)=>{
  const res = await openapi.createCompletion({
    model:"text-davinci-002",
    prompt:msg,
    temperature:0.7,
    max_tokens:256
  })
  // SEND RESPONSE
  return res.data.choices[0].text
}
  let mymessage = ""
  let allchat  = []

const btnsend  = async()=>{
  // YOU TYPE HERE
  allchat = [...allchat,{type:'user',text:mymessage}]
  // CLEAN MESSAGE YOU
  mymessage = ""

  const printresponse = await genchat(allchat.map((m)=>m.text).join("/n"))

  // PUSH TO OBJECT IF CHATBOT RESPONSE

  allchat = [...allchat,{type:'bot',text:printresponse}]
}
</script>
<div>
  {#each allchat as res_msg}
  <div class="message {res_msg.type}">
    <span class="whosend">
      <!-- WHO SEND -->
      {res_msg.type == 'user' ? 'You':'chatBot'}
    </span>
    <!-- SHOW MESSAGE HERE -->
    {res_msg.text}
  </div>

  {/each}

</div>

<!-- CREATE INPUT TEXT MESSAGE -->

<div style="display: flex;flex-direction: row;justify-content: center;">
  <input 
  placeholder="Type Message You Here ....." 
  class="inputtext" type="text" bind:value={mymessage}>

  <button class="btnyousend" on:click={btnsend}>send</button>
</div>

<!-- STYLING -->
<style>
  .message{
    color: white;
    padding: 20px;
    font-size: 20px;
    opacity: 0;
    display: flex;
    flex-direction: row;
    justify-content: center;
  }

/*  IF SEND IS USER*/
  .message.user{
    text-align: right;
    background-color: white;
    color: black;
    box-shadow: 0px 0px 15px rgba(0,0,0,0.5);
    opacity: 1;
    transition: all 2s ease;
  }

/*  IF SEND IS CHATBOT*/
  .message.bot{
    text-align: left;
    background-color: green;
    color: white;
    box-shadow: 0px 0px 15px rgba(0,0,0,0.5);
       opacity: 1;
    transition: all 2s ease
  }

/*STYLE FOR INPUT*/
.inputtext{
  color: black;
  height: 80px;
  width: 80%;
  font-size: 22px;
  text-align: center;
}

/*STYLE FOR BUTTON*/
.btnyousend{
  background-color: yellow;
  color: black;
}

/*STYLE FOR WHOSEND*/
.whosend{
  color: white;
  font-size: 12px;
  background-color: blue;
  padding: 10px;
  font-weight: bold;
}
</style>