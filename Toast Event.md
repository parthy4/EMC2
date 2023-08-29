---------------------------------------------  HTML ---------------------------------------------

    <lightning-card title="Toast Events" icon-name="utility:user">
        <div class="slds-var-m-around_medium">
            <div style="padding:10px">
                <lightning-button label="Error" onclick={showError}></lightning-button>
                <lightning-button label="Warning" onclick={showWarning}></lightning-button>
                <lightning-button label="Success" onclick={showSuccess}></lightning-button>
                <lightning-button label="Info" onclick={showInfo}></lightning-button>
            </div>
        </div>
    </lightning-card>

<template>
    <lightning-card title="Toast Events" icon-name="utility:user">
        <div class="slds-var-m-around_medium">
            <div style="padding:10px">
                <lightning-button label="Error" onclick={showError}></lightning-button>
                <lightning-button label="Warning" onclick={showWarning}></lightning-button>
                <lightning-button label="Success" onclick={showSuccess}></lightning-button>
                <lightning-button label="Info" onclick={showInfo}></lightning-button>
            </div>
        </div>
    </lightning-card>

</template>  
<template>
    <lightning-card title="Toast Events" icon-name="utility:user">
        <div class="slds-var-m-around_medium">
            <div style="padding:10px">
                <lightning-button label="Error" onclick={showError}></lightning-button>
                <lightning-button label="Warning" onclick={showWarning}></lightning-button>
                <lightning-button label="Success" onclick={showSuccess}></lightning-button>
                <lightning-button label="Info" onclick={showInfo}></lightning-button>
            </div>
        </div>
    </lightning-card>

</template>  

--------------------------------------------- JS ---------------------------------------------

import { LightningElement } from 'lwc';
import { ShowToastEvent } from 'lightning/platformShowToastEvent'

export default class ShowToastEvents extends LightningElement {
    showError() {
        console.log('###Click');
        const evt = new ShowToastEvent({
            title: 'Salesforce Toast',
            message: 'Salesforce Bolt LWC Stack Example',
            variant: 'error'
        });
        this.dispatchEvent(evt);
    }
    showWarning() {
        const evt = new ShowToastEvent({
            title: 'Salesforce Toast',
            message: 'Salesforce Bolt LWC Stack Example',
            variant: 'warning'
        });
        this.dispatchEvent(evt);
    }
    showSuccess() {
        const evt = new ShowToastEvent({
            title: 'Salesforce Toast',
            message: 'Salesforce Bolt LWC Stack Example',
            variant: 'success'
        });
        this.dispatchEvent(evt);
    }
    showInfo() {
        const evt = new ShowToastEvent({
            title: 'Salesforce Toast',
            message: 'Salesforce Bolt LWC Stack Example',
            variant: 'info'
        });
        this.dispatchEvent(evt);
    }
}