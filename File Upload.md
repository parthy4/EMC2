---------------------------- Parent HTML ----------------------------
<lightning-file-upload
    name="fileUploader"
    accept={acceptedFormats}
    record-id={recordId}
    onuploadfinished={handleUploadFinished}
    multiple
  >
  </lightning-file-upload>

  <br />
  <div
    class="
      slds-page-header__row
      slds-var-m-top_x-small
      slds-var-m-left_medium
      slds-grid slds-wrap
    "
  >
    <ul class="slds-grid slds-wrap slds-gutters">
      <template if:true={loaded}>
        <template for:each={files} for:item="file">
          <c-preview-file-thumbnail-card
            key={file.Id}
            file={file}
            record-id={recordId}
            thumbnail={file.thumbnailFileCard}
          ></c-preview-file-thumbnail-card>
        </template>
      </template>
    </ul>
  </div>
</template>
---------------------------- JS ----------------------------
import { LightningElement, wire, api, track } from "lwc";
import { refreshApex } from "@salesforce/apex";
import { ShowToastEvent } from "lightning/platformShowToastEvent";
import getFileVersions from "@salesforce/apex/FileController.getVersionFiles";

export default class PreviewFileThumbnails extends LightningElement {
  loaded = false;
  @track fileList;
  @api recordId;
  @track files = [];
  get acceptedFormats() {
    return [".pdf", ".png", ".jpg", ".jpeg"];
  }

  @wire(getFileVersions, { recordId: "$recordId" })
  fileResponse(value) {
    this.wiredActivities = value;
    const { data, error } = value;
    this.fileList = "";
    this.files = [];
    if (data) {
      this.fileList = data;
      for (let i = 0; i < this.fileList.length; i++) {
        let file = {
          Id: this.fileList[i].Id,
          Title: this.fileList[i].Title,
          Extension: this.fileList[i].FileExtension,
          ContentDocumentId: this.fileList[i].ContentDocumentId,
          ContentDocument: this.fileList[i].ContentDocument,
          CreatedDate: this.fileList[i].CreatedDate,
          thumbnailFileCard:
            "/sfc/servlet.shepherd/version/renditionDownload?rendition=THUMB720BY480&versionId=" +
            this.fileList[i].Id +
            "&operationContext=CHATTER&contentId=" +
            this.fileList[i].ContentDocumentId
        };
        this.files.push(file);
      }
      this.loaded = true;
    } else if (error) {
      this.dispatchEvent(
        new ShowToastEvent({
          title: "Error loading Files",
          message: error.body.message,
          variant: "error"
        })
      );
    }
  }
  handleUploadFinished(event) {
    const uploadedFiles = event.detail.files;
    refreshApex(this.wiredActivities);
    this.dispatchEvent(
      new ShowToastEvent({
        title: "Success!",
        message: uploadedFiles.length + " Files Uploaded Successfully.",
        variant: "success"
      })
    );
  }
}


---------------------------- PreviewFileThumbnailCard HTML ----------------------------

  <template if:true={file}>
    <li
      class="
        slds-col
        slds-var-p-vertical_x-small
        slds-small-size_1-of-2
        slds-medium-size_1-of-4
        slds-large-size_1-of-6
      "
    >
      <div class="slds-file slds-file_card slds-has-title" style="width: 14rem">
        <figure>
          <a class="slds-file__crop slds-file__crop_4-by-3 slds-m-top_x-small">
            <img src={thumbnail} alt={file.title} />
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

---------------------------- PreviewFileThumbnailCard JS ----------------------------

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