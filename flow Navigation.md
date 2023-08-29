----------------------- HTML ----------------------- 

<template>
  <lightning-button label="Back" onclick={handleBack}> </lightning-button>
  <br />
  <br />
  <lightning-button label="Next" onclick={handleNext} variant="brand">
  </lightning-button>
</template>

----------------------- JS  ----------------------- 
import { LightningElement, api } from "lwc";
import {
  FlowNavigationBackEvent,
  FlowNavigationNextEvent
} from "lightning/flowSupport";
export default class LwcForFlow extends LightningElement {
  @api availableActions = [];
  handleNext() {
    if (this.availableActions.find((action) => action === "NEXT")) {
      const navigateNextEvent = new FlowNavigationNextEvent();
      this.dispatchEvent(navigateNextEvent);
    }
  }
  handleBack() {
    if (this.availableActions.find((action) => action === "BACK")) {
      const navigateBackEvent = new FlowNavigationBackEvent();
      this.dispatchEvent(navigateBackEvent);
    }
  }
}

-----------------------  XML ----------------------- 
<?xml version="1.0" encoding="UTF-8"?>
<LightningComponentBundle xmlns="http://soap.sforce.com/2006/04/metadata">
    <apiVersion>54.0</apiVersion>
    <isExposed>true</isExposed>
    <targets>
      <target>lightning__FlowScreen</target>
  </targets>
</LightningComponentBundle>


======================== code Reference======================
[ https://www.salesforcebolt.com/2022/04/navigation-in-flow-using-lightning-web-component.html ]
[ https://www.youtube.com/watch?v=Z3lZwPLsGcc&list=PL1qNeQdVTMhF51iyP2Ieky5-B-zG1_YTl&index=62 ]