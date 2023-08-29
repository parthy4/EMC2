the blog reference
[ https://www.salesforcebolt.com/2021/11/file-preview-modal-in-lightning-web-component.html ]


---------------------------- Parent HTML ----------------------------

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
            onclick={closeModal}
          ></lightning-button-icon>

          <h2 id="id-of-modalheader-h2" class="slds-text-heading_large">
            File Preview
          </h2>
        </header>
        <!-- Header End -->
        <div class="slds-modal__content" id="modal-content-id-1">
          <lightning-layout>
            <lightning-layout-item flexibility="auto">
              <article class="slds-card">
                <div
                  class="slds-card__body slds-card__body_inner"
                  style="margin: 0"
                >
                  <template if:false={showFrame}>
                    <img src={url} />
                  </template>
                  <template if:true={showFrame}>
                    <iframe
                      src={url}
                      style="width: 100%; height: 800px"
                    ></iframe>
                  </template>
                </div>
              </article>
            </lightning-layout-item>
          </lightning-layout>
        </div>
        <footer class="slds-modal__footer slds-grid slds-grid_align-spread">
          <lightning-button
            variant="brand-outline"
            label="Cancel"
            onclick={closeModal}
            title="Cancel"
            class="slds-var-m-left_x-small"
          ></lightning-button>
        </footer>
      </div>
    </section>
    <div class="slds-backdrop slds-backdrop_open"></div>
  </template>
</template>

---------------------------- JS ----------------------------

import { LightningElement, api } from "lwc";

export default class PreviewFileModal extends LightningElement {
  @api url;
  @api fileExtension;
  showFrame = false;
  showModal = false;
  @api show() {
    console.log("###showFrame : " + this.fileExtension);
    if (this.fileExtension === "pdf") this.showFrame = true;
    else this.showFrame = false;
    this.showModal = true;
  }
  closeModal() {
    this.showModal = false;
  }
}

------------------- CSS -------------------------------
.slds-modal__container {
  width: 90% !important;
  max-width: 90% !important;
}


----------------------------  PreviewFileThumbnailCard HTML ---------------------------- 

  <template if:true={file}>
    <c-preview-file-modal
      url={file.downloadUrl}
      file-extension={file.Extension}
    ></c-preview-file-modal>
    <li
      class="
        slds-col
        slds-var-p-vertical_x-small
        slds-small-size_1-of-2
        slds-medium-size_1-of-4
        slds-large-size_1-of-6
      "
    >
      <div
        class="slds-file slds-file_card slds-has-title"
        style="width: 14rem"
        onclick={filePreview}
      >
        <figure>
          <a class="slds-file__crop slds-file__crop_4-by-3 slds-m-top_x-small">
            <img src={thumbnail} alt={file.title} onclick={filePreview} />
          </a>
          <figcaption class="slds-file__title slds-file__title_card">
            <div class="slds-media slds-media_small slds-media_center">
              <lightning-icon
                icon-name={iconName}
                alternative-text={file.Title}
                title={file.Title}
                size="xx-small"
              >
                <span class="slds-assistive-text"
                  >{file.Title}.{file.Extension}</span
                >
              </lightning-icon>
              <div class="slds-media__body slds-var-p-left_xx-small">
                <span class="slds-file__text slds-truncate" title={file.Title}
                  >{file.Title}.{file.Extension}</span
                >
              </div>
            </div>
          </figcaption>
        </figure>

        <div class="slds-is-absolute" style="top: 3px; right: 5px; z-index: 10">
          <lightning-button-menu
            alternative-text="Show File Menu"
            variant="border-filled"
            icon-size="xx-small"
          >
            <lightning-menu-item
              value="preview"
              label="Preview"
            ></lightning-menu-item>
            <lightning-menu-item
              value="delete"
              label="Delete"
            ></lightning-menu-item>
          </lightning-button-menu>
        </div>
      </div>
    </li>
  </template>
</template>
----------------------------  PreviewFileThumbnailCard JS ---------------------------- 
import { LightningElement, api } from "lwc";

export default class PreviewFileThumbnailCard extends LightningElement {
  @api file;
  @api recordId;
  @api thumbnail;

  get iconName() {
    if (this.file.Extension) {
      if (this.file.Extension === "pdf") {
        return "doctype:pdf";
      }
      if (this.file.Extension === "ppt") {
        return "doctype:ppt";
      }
      if (this.file.Extension === "xls") {
        return "doctype:excel";
      }
      if (this.file.Extension === "csv") {
        return "doctype:csv";
      }
      if (this.file.Extension === "txt") {
        return "doctype:txt";
      }
      if (this.file.Extension === "xml") {
        return "doctype:xml";
      }
      if (this.file.Extension === "doc") {
        return "doctype:word";
      }
      if (this.file.Extension === "zip") {
        return "doctype:zip";
      }
      if (this.file.Extension === "rtf") {
        return "doctype:rtf";
      }
      if (this.file.Extension === "psd") {
        return "doctype:psd";
      }
      if (this.file.Extension === "html") {
        return "doctype:html";
      }
      if (this.file.Extension === "gdoc") {
        return "doctype:gdoc";
      }
    }
    return "doctype:image";
  }

  filePreview() {
    console.log("###Click");
    const showPreview = this.template.querySelector("c-preview-file-modal");
    showPreview.show();
  }
}
---------------------------- APEX ----------------------------

public with sharing class FileController{
@AuraEnabled(cacheable=true)
    public static List<ContentVersion> getVersionFiles(String recordId){
        try {
            return [
		SELECT
		Id,
                Title,
                ContentDocumentId,
                FileType, 
		ContentSize,
		FileExtension,
		VersionNumber,
		CreatedDate,
		VersionData,
                FirstPublishLocationId
		FROM ContentVersion
		WHERE FirstPublishLocationId =:recordId
			ORDER BY CreatedDate DESC
		];
        
        } catch (Exception e) {
            throw new AuraHandledException(e.getMessage());
        }
    }
}