------------------------- VF Page------------------------- 
<apex:page>
    <br/>
<input id="message" type="text"/>
<button onclick="sendToLWC()">Send to LWC</button>

<script>
    var lexOrigin="https://enterprise-nosoftware-7728-dev-ed.lightning.force.com"
    function sendToLWC() {
        var payload = document.getElementById("message").value;
        var message = {
            name:"SampleVFToLWCMessage",
            payload:payload
        };
        parent.postMessage(message,lexOrigin);
    }
    
</script>
</apex:page>

------------------------- HTML ------------------------- 

<template>
  <lightning-card title="Visualforce to LWC">
    <div class="slds-p-around_small">
      Hello I am a Lightning Web Component
      <br />
      <p style="font-weight: bold; font-size: x-large">{messageFromVF}</p>
      <br />
      <iframe id="vfIframe" src="/apex/SampleVFToLWC"> </iframe>
    </div>
  </lightning-card>
</template>

-------------------------  JS ------------------------- 

import { LightningElement } from "lwc";

export default class SendToLWC extends LightningElement {
  messageFromVF;
  connectedCallback() {
    var VfOrigin =
      "https://enterprise-nosoftware-7728-dev-ed--c.visualforce.com";
    window.addEventListener("message", (message) => {
      if (message.origin !== VfOrigin) {
        //Not the expected origin
        return;
      }

      //handle the message
      if (message.data.name === "SampleVFToLWCMessage") {
        this.messageFromVF = message.data.payload;
      }
    });
  }
}


================================================

blog -[ https://www.salesforcebolt.com/2022/06/communication-between-lightning-web-component-and-visualforce-page.html ]
video - [ https://www.youtube.com/watch?v=-hXmO7wszIc&list=PL1qNeQdVTMhF51iyP2Ieky5-B-zG1_YTl&index=69 ]