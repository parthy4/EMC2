--------------------------------------------- HTML ---------------------------------------------

    <lightning-card title="Delete Records without server-side script" icon-name="utility:user">
        <template if:true={accounts}>
            <div class="slds-var-m-around_medium">
                <template for:each={accounts} for:item="acc">
                    <lightning-layout key={acc.Id} class="slds-var-m-vertical_x-small">
                        <lightning-layout-item flexibility="grow" style="min-width:350px" >
                            {acc.Name}
                        </lightning-layout-item>
                        <lightning-layout-item flexibility="grow">
                            <lightning-button-icon icon-name="utility:delete" onclick={deleteAccount} data-recordid={acc.Id}>

                            </lightning-button-icon>
                        </lightning-layout-item>
                    </lightning-layout>
                </template>

            </div>
        </template>
    </lightning-card>


--------------------------------------------- JS ---------------------------------------------

import {LightningElement, wire } from 'lwc';
import {ShowToastEvent} from 'lightning/platformShowToastEvent';
import {refreshApex} from '@salesforce/apex';
import {deleteRecord} from 'lightning/uiRecordApi';
import getAccountList from '@salesforce/apex/AccountController.getAccList';

export default class DeleteAccounts extends LightningElement {
    accounts;
    error;
    wiredAccountsResult;

    @wire(getAccountList)
    wiredAccounts(result){
        this.wiredAccountsResult = result;
        if(result.data){
            this.accounts = result.data;
            this.error = undefined;

        } else if(result.error){
            this.error = result.error;
            this.accounts = undefined;
        }
    }

    deleteAccount(event){
        const recordId = event.target.dataset.recordid;
        deleteRecord(recordId)
        .then(()=>{
this.dispatchEvent(
    new ShowToastEvent({
        title : 'Success',
        message : 'Account Deleted',
        variant : 'success'
    })
);
        return refreshApex(this.wiredAccountsResult);
        })
        .catch((error)=>{
            this.dispatchEvent(
                new ShowToastEvent({
                    title : 'Error',
                    message : 'Error Deleting record',
                    variant : 'error'
                })
            );
        });
    }
}



--------------------------------------------- APEX ---------------------------------------------

public with sharing class AccountController{
    @AuraEnabled(cacheable=true)
    public static List<Account> getAccList(){
        return [SELECT Id, Name, Phone FROM ACCOUNT ORDER BY CreatedDate desc Limit 10];
    }
}