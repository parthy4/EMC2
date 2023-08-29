------------------------- HTML -------------------------
<lightning-card title="Navigations" icon-name="utility:user">
    <div class="slds-var-m-around_medium">
      <lightning-button
        label="Navigate to Home"
        class="slds-var-m-around_medium"
        onclick={navigateToHome}
      ></lightning-button>
      <br /><br />
      <lightning-button
        label="Navigate to New Contact"
        class="slds-var-m-around_medium"
        onclick={navigateToNewContact}
      ></lightning-button>
      <br /><br />
      <lightning-button
        label="Navigate to New Contact with Default"
        class="slds-var-m-around_medium"
        onclick={navigateToNewContactWithDefault}
      ></lightning-button>
      <br /><br />
      <lightning-button
        label="Navigate to Contact ListView"
        class="slds-var-m-around_medium"
        onclick={navigateToContactListView}
      ></lightning-button>
      <br /><br />
      <lightning-button
        label="Navigate to Tab"
        class="slds-var-m-around_medium"
        onclick={navigateToTab}
      ></lightning-button>
    </div>
  </lightning-card>

</template>

------------------------- JS -------------------------

import { LightningElement } from "lwc";
import { NavigationMixin } from "lightning/navigation";
import { encodeDefaultFieldValues } from "lightning/pageReferenceUtils";

export default class Navigations extends NavigationMixin(LightningElement) {
  navigateToHome() {
    this[NavigationMixin.Navigate]({
      type: "standard__namedPage",
      attributes: {
        pageName: "home"
      }
    });
  }
  navigateToNewContact() {
    this[NavigationMixin.Navigate]({
      type: "standard__objectPage",
      attributes: {
        objectApiName: "Contact",
        actionName: "new"
      }
    });
  }
  navigateToNewContactWithDefault() {
    const defaultValues = encodeDefaultFieldValues({
      FirstName: "Salesforce",
      LastName: "Bolt"
    });

    this[NavigationMixin.Navigate]({
      type: "standard__objectPage",
      attributes: {
        objectApiName: "Contact",
        actionName: "new"
      },
      state: {
        defaultFieldValues: defaultValues
      }
    });
  }
  navigateToContactListView() {
    this[NavigationMixin.Navigate]({
      type: "standard__objectPage",
      attributes: {
        objectApiName: "Contact",
        actionName: "list"
      },
      state: {
        filterName: "Recent"
      }
    });
  }
  navigateToTab() {
    this[NavigationMixin.Navigate]({
      type: "standard__navItemPage",
      attributes: {
        apiName: "LWCStack"
      }
    });
  }
}