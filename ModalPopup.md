------------------------- HTML -------------------------

<template if:true={showModal}>
    <section
      role="dialog"
      tabindex="-1"
      aria-labelledby="modal-heading-01"
      aria-modal="true"
      aria-describedby="modal-content-id-1"
      class="slds-modal slds-fade-in-open"
    >
      <div class="slds-modal__container">
        <!-- Header Start -->
        <header class="slds-modal__header">
          <lightning-button-icon
            class="slds-modal__close"
            title="Close"
            icon-name="utility:close"
            icon-class="slds-button_icon-inverse"
            onclick={handleDialogClose}
          ></lightning-button-icon>

          <h2 class="slds-text-heading_medium slds-hyphenate header-string">
            My Modal Popup
          </h2>
        </header>
        <!-- Header End -->
        <div
          class="slds-modal__content slds-p-around_medium"
          id="modal-content-id-1"
        >
          <slot>
            <p>This is a custom modal popup component.</p>
          </slot>
        </div>
      </div>
    </section>
    <div class="slds-backdrop slds-backdrop_open"></div>
  </template>
</template>

------------------------- JS -------------------------

import { LightningElement, api } from "lwc";


export default class Modal extends LightningElement {
  showModal = false;

  @api show() {
    this.showModal = true;
  }
  handleDialogClose() {
    this.showModal = false;
  }
}


------------------------- Parent~ HTML -------------------------

<template>
  <lightning-card title="MiscModal" icon-name="custom:custom19">
    <div class="slds-var-m-around_medium">
      <lightning-button
        label="Show modal"
        onclick={handleShowModal}
      ></lightning-button>
      <c-modal-Popup> </c-modal-Popup>
    </div>
  </lightning-card>
</template>

------------------------- Parent~ JS -------------------------

import { LightningElement } from "lwc";

export default class PreviewModal extends LightningElement {
  handleShowModal() {
    const modal = this.template.querySelector("c-modal-Popup");
    modal.show();
  }
}