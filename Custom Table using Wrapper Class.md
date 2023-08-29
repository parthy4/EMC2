------------------------------------  HTML ------------------------------------
<template>
  <div style="padding: 10px">
    <div style="padding: 10px">
      <table
        class="
          slds-table
          slds-table_cell-buffer
          slds-table_bordered
          slds-table_col-bordered
        "
        border="1"
        width="100%"
      >
        <thead>
          <tr class="slds-line-height_reset">
            <td>
              <lightning-input
                type="checkbox"
                onchange={handleAllChange}
              ></lightning-input>
            </td>
            <td style="font-weight: bold">Account Name</td>
            <td style="font-weight: bold">Phone</td>
          </tr>
        </thead>
        <template for:each={accList} for:item="acc">
          <tr key={acc.Id} class="slds-hint-parent">
            <td>
              <lightning-input
                type="checkbox"
                checked={acc.isSelected}
                onchange={handleCheckChange}
                value={acc.index}
              ></lightning-input>
            </td>
            <td>{acc.AccountName}</td>
            <td>{acc.Phone}</td>
          </tr>
        </template>
      </table>
    </div>
    <br />
    <lightning-button
      variant="brand"
      label="Get Selected Accounts"
      onclick={getSelectedAccounts}
    ></lightning-button>
  </div>
</template>
  <div style="padding: 10px">
    <div style="padding: 10px">
      <table
        class="
          slds-table
          slds-table_cell-buffer
          slds-table_bordered
          slds-table_col-bordered
        "
        border="1"
        width="100%"
      >
        <thead>
          <tr class="slds-line-height_reset">
            <td>
              <lightning-input
                type="checkbox"
                onchange={handleAllChange}
              ></lightning-input>
            </td>
            <td style="font-weight: bold">Account Name</td>
            <td style="font-weight: bold">Phone</td>
          </tr>
        </thead>
        <template for:each={accList} for:item="acc">
          <tr key={acc.Id} class="slds-hint-parent">
            <td>
              <lightning-input
                type="checkbox"
                checked={acc.isSelected}
                onchange={handleCheckChange}
                value={acc.index}
              ></lightning-input>
            </td>
            <td>{acc.AccountName}</td>
            <td>{acc.Phone}</td>
          </tr>
        </template>
      </table>
    </div>
    <br />
    <lightning-button
      variant="brand"
      label="Get Selected Accounts"
      onclick={getSelectedAccounts}
    ></lightning-button>
  </div>
</template>
------------------------------------ JS ------------------------------------

import { wire, LightningElement } from "lwc";
import getAccList from "@salesforce/apex/wrapperTable.getAccounts";

export default class WrapperTable extends LightningElement {
  selectedAccounts = [];
  accList;
  @wire(getAccList)
  wiredRecord({ error, data }) {
    if (error) {
      let message = "Unknown error";
      if (Array.isArray(error.body)) {
        message = error.body.map((e) => e.message).join(", ");
      } else if (typeof error.body.message === "string") {
        message = error.body.message;
      }
    } else if (data) {
      this.accList = JSON.parse(data);
    }
  }
  handleAllChange(event) {
    for (var i = 0; i < this.accList.length; i++) {
      this.accList[i].isSelected = event.target.checked;
    }
  }
  handleCheckChange(event) {
    this.accList[event.target.value].isSelected = event.target.checked;
  }
  getSelectedAccounts(event) {
    for (var i = 0; i < this.accList.length; i++) {
      if (this.accList[i].isSelected) {
        this.selectedAccounts.push(this.accList[i]);
      }
    }
    console.log("###Selected Accounts" + this.selectedAccounts.length);
    console.log("###Stringify : " + JSON.stringify(this.selectedAccounts));
  }
}

------------------------------------  APEX ------------------------------------ 

public with sharing class wrapperTable {
    public wrapperTable() {

    }
    @AuraEnabled(cacheable=true)
	public static String getAccounts(){
        Integer rowIndex = 0;
        List<accountWrap> accWrapList = new List<accountWrap>();
		try {
            List<Account> accList = [SELECT Id, Name, Phone FROM Account limit 10];
            for(Account a : accList){
            accWrapList.add(new accountWrap(a.Id,a.Name,a.Phone,rowIndex));
            rowIndex++;
            }
            return JSON.serialize(accWrapList);
		} catch (Exception e) {
			throw new AuraHandledException(e.getMessage());
		}
	}
    public class accountWrap{
        public String Id;
        public String AccountName;
        public String Phone;
        public Boolean isSelected;
        public Integer index;
	    public accountWrap(String Id, String AccountName, String Phone, Integer index){
            this.Id = Id;
            this.AccountName = AccountName;
            this.Phone = Phone;
            this.isSelected = false;
            this.index = index;
		}
            
    }
}