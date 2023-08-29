--------------------------------------------- HTML ---------------------------------------------

    <lightning-card title="Bind Imperative Method with Param" icon-name="utility:user">
        <div class="slds-var-m-around_medium">
            <lightning-input onchange={handleonchange} label="Search Account" 
            value={searchKey}>
        </lightning-input>
        </div>
        <div class="slds-var-m-around_medium">
            <lightning-button label="Get Accounts" onclick={buttonClick}>

            </lightning-button>
        </div>
        <template if:true={accounts}>
            <div class="slds-var-m-around_medium">
                <template for:each={accounts} for:item="acc">
                    <lightning-layout key={acc.Id} class="slds-var-m-vertical_x-small">
                        <lightning-layout-item flexibility="grow">
                            {acc.Name}
                        </lightning-layout-item>
                        
                    </lightning-layout>
                </template>
            </div>
            <div class="slds-var-m-around_medium">
                <br/>
                {error}
            </div>
        </template>
    </lightning-card>
</template>
<lightning-card title="Bind Imperative Method with Param" icon-name="utility:user">
        <div class="slds-var-m-around_medium">
            <lightning-input onchange={handleonchange} label="Search Account" 
            value={searchKey}>
        </lightning-input>
        </div>
        <div class="slds-var-m-around_medium">
            <lightning-button label="Get Accounts" onclick={buttonClick}>

            </lightning-button>
        </div>
        <template if:true={accounts}>
            <div class="slds-var-m-around_medium">
                <template for:each={accounts} for:item="acc">
                    <lightning-layout key={acc.Id} class="slds-var-m-vertical_x-small">
                        <lightning-layout-item flexibility="grow">
                            {acc.Name}
                        </lightning-layout-item>
                        
                    </lightning-layout>
                </template>
            </div>
            <div class="slds-var-m-around_medium">
                <br/>
                {error}
            </div>
        </template>
    </lightning-card>
<lightning-card title="Bind Imperative Method with Param" icon-name="utility:user">
        <div class="slds-var-m-around_medium">
            <lightning-input onchange={handleonchange} label="Search Account" 
            value={searchKey}>
        </lightning-input>
        </div>
        <div class="slds-var-m-around_medium">
            <lightning-button label="Get Accounts" onclick={buttonClick}>

            </lightning-button>
        </div>
        <template if:true={accounts}>
            <div class="slds-var-m-around_medium">
                <template for:each={accounts} for:item="acc">
                    <lightning-layout key={acc.Id} class="slds-var-m-vertical_x-small">
                        <lightning-layout-item flexibility="grow">
                            {acc.Name}
                        </lightning-layout-item>
                        
                    </lightning-layout>
                </template>
            </div>
            <div class="slds-var-m-around_medium">
                <br/>
                {error}
            </div>
        </template>
</lightning-card>
<lightning-card title="Bind Imperative Method with Param" icon-name="utility:user">
        <div class="slds-var-m-around_medium">
            <lightning-input onchange={handleonchange} label="Search Account" 
            value={searchKey}>
        </lightning-input>
        </div>
        <div class="slds-var-m-around_medium">
            <lightning-button label="Get Accounts" onclick={buttonClick}>

            </lightning-button>
        </div>
        <template if:true={accounts}>
            <div class="slds-var-m-around_medium">
                <template for:each={accounts} for:item="acc">
                    <lightning-layout key={acc.Id} class="slds-var-m-vertical_x-small">
                        <lightning-layout-item flexibility="grow">
                            {acc.Name}
                        </lightning-layout-item>
                        
                    </lightning-layout>
                </template>
            </div>
            <div class="slds-var-m-around_medium">
                <br/>
                {error}
            </div>
        </template>
</lightning-card>
<template>
    <lightning-card title="Bind Imperative Method with Param" icon-name="utility:user">
        <div class="slds-var-m-around_medium">
            <lightning-input onchange={handleonchange} label="Search Account" 
            value={searchKey}>
        </lightning-input>
        </div>
        <div class="slds-var-m-around_medium">
            <lightning-button label="Get Accounts" onclick={buttonClick}>

            </lightning-button>
        </div>
        <template if:true={accounts}>
            <div class="slds-var-m-around_medium">
                <template for:each={accounts} for:item="acc">
                    <lightning-layout key={acc.Id} class="slds-var-m-vertical_x-small">
                        <lightning-layout-item flexibility="grow">
                            {acc.Name}
                        </lightning-layout-item>
                        
                    </lightning-layout>
                </template>
            </div>
            <div class="slds-var-m-around_medium">
                <br/>
                {error}
            </div>
        </template>
    </lightning-card>
</template>
--------------------------------------------- JS ---------------------------------------------

import { LightningElement } from 'lwc';
import findAccountList from '@salesforce/apex/AccountController.findAccList';

export default class BindImperativewithParam extends LightningElement {
    searchKey='';
    accounts;
    error;

    handleonchange(event){
        this.searchKey = event.target.value;
    }

    buttonClick(){
        findAccountList({keyword: this.searchKey})
        .then((result) =>{
            this.accounts = result;
            this.error = undefined;
        })
        .catch((error)=>{
            this.error = error;
            this.accounts = undefined;
        });

    }
}


--------------------------------------------- APEX ---------------------------------------------

public with sharing class AccountController{
    @AuraEnabled(cacheable=true)
    public static List<Account> getAccList(){
        return [SELECT Id, Name FROM ACCOUNT ORDER BY CreatedDate desc Limit 10];
    }

    @AuraEnabled(cacheable=true)
    public static List<Account> findAccList(String keyword){
        String key='%'+keyword+'%';
        return [SELECT Id, Name,Phone FROM ACCOUNT WHERE Name LIKE:key ORDER BY CreatedDate desc Limit 10];
    }
    @AuraEnabled(cacheable=true)
    public static Account getSingleAccount(){
        return [SELECT Id, Name, Phone FROM ACCOUNT ORDER BY CreatedDate desc Limit 1];
    }

}