------------------------- HTML ------------------------- 
<template>
  <lightning-card title="LWC to Visualforce">
    <div class="slds-p-around_small">
      <lightning-input
        type="text"
        label="Enter some text"
        value={message}
        onchange={handleOnChange}
      ></lightning-input>
      <br />
      <lightning-button
        variant="brand"
        label="Send to VF"
        title="Primary action"
        onclick={handleClick}
        class="slds-m-left_x-small"
      ></lightning-button>
      <br />
      <br />
      <iframe id="vfIframe" src="/apex/SampleVF"> </iframe>
    </div>
  </lightning-card>
</template>

------------------------- JS ------------------------- 

import { LightningElement } from "lwc";

export default class SendToVF extends LightningElement {
  vfRoot = "https://enterprise-nosoftware-7728-dev-ed--c.visualforce.com/";
  message = "";
  handleOnChange(event) {
    this.message = event.target.value;
  }
  handleClick() {
    var vfWindow = this.template.querySelector("iframe").contentWindow;
    vfWindow.postMessage(this.message, this.vfRoot);
  }
}

-------------------------  VF Page ------------------------- 
<apex:page>
<!-- Begin Default Content REMOVE THIS -->
<h1>Congratulations</h1>
This is your new Page
<!-- End Default Content REMOVE THIS -->
<p id="messageFromLWC" style="font-weight:bold; font-size:x-large"> </p>
<script>
    var lexOrigin ="https://enterprise-nosoftware-7728-dev-ed.lightning.force.com";
    window.addEventListener("message",function(event){
        if(event.origin !== lexOrigin){
            //Not the expected origin
            return;
        }
        document.getElementById("messageFromLWC").innerHTML = 'Text from LWC : '+event.data;
    },false);
</script>
</apex:page>