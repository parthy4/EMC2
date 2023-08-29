--------------------------------- HTML ---------------------------------
<template>
  <lightning-card
    title="Internationalization Date in LWC"
    icon-name="utility:date_input"
  >
    <div class="slds-var-p-around_small">
      <b>
        Date : {date}
        <br />
        Formatted Date : {formattedDate}
      </b>
      <br />
      <br />
      <lightning-button
        label="Get Formatted Date"
        onclick={handleFormattedDate}
      >
      </lightning-button>
    </div>
  </lightning-card>
</template>

--------------------------------- JS ---------------------------------
import { LightningElement } from "lwc";
import LOCALE from "@salesforce/i18n/locale";

export default class Internationalization extends LightningElement {
  date = new Date(2022, 6, 25);
  formattedDate;

  handleFormattedDate() {
    this.formattedDate = new Intl.DateTimeFormat(LOCALE).format(this.date);
  }
}

=============================================================================

Blog - https://www.salesforcebolt.com/2022/06/internationalization-in-lightning-web-component-currency-language-timezone.html
video - https://www.youtube.com/watch?v=bhFrplLlrKo&list=PL1qNeQdVTMhF51iyP2Ieky5-B-zG1_YTl&index=70