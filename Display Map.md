for Map=> please add custom fields in object like ( address, state, city, zipcode ) because google map will recognized it easily

---------------------------------  HTML ---------------------------------
<template>
    <lightning-card title="Housing Map">
      <!-- Explore all the base components via Base component library 
      (https://developer.salesforce.com/docs/component-library/overview/components)-->
        <lightning-map map-markers={mapMarkers}> </lightning-map>
    </lightning-card>
  </template>

--------------------------------- JS -----------------------------------
import { LightningElement, wire } from "lwc";
import getHouses from "@salesforce/apex/HouseService.getRecords";

export default class HousingMap extends LightningElement {
    mapMarkers;
    error;
  
    @wire(getHouses)
    wiredHouses({ error, data }) {
        if (data) {
    // We are using Javascript Map function to transform the
      this.mapMarkers = data.map((element) => {
        return {
          location: {
            Street: element.Address__c,
            City: element.City__c,
            State: element.State__c
          },

          title: element.Name
        };
      });
      this.error = undefined;
    } else if (error) {
      this.error = error;
      this.mapMarkers = undefined;
    }
  }
}

--------------------------------- APEX ---------------------------------
public with sharing class HouseService {
    @AuraEnabled(cacheable=true)
    public static List<House__c> getRecords() { 
        try {
            // Create a list of House records from a SOQL query
            List<House__c> lstHouses = [
                SELECT
                   Id,
                   Name,
                   Address__c,
                   State__c, 
                   City__c,
                   Zip__c
                   FROM House__c
                   WITH SECURITY_ENFORCED
                   ORDER BY CreatedDate
                   LIMIT 10
                ];
                  return lstHouses;
        }
        // Code to handle exception
        catch (Exception e) {
           throw new AuraHandledException(e.getMessage());
        }
    }
}

================== Other way this is Manual entry to display Map ===========
[https://www.youtube.com/watch?v=aNhRTsxwiOc&list=PL1qNeQdVTMhF51iyP2Ieky5-B-zG1_YTl&index=47]
[https://www.salesforcebolt.com/2021/12/display-maps-in-lightning-web-component.html]