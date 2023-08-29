
-------------- HTML ----------

  <center><p style="font-size: xx-large">{randomJoke}</p></center>
</template>
<template>
  <center><p style="font-size: xx-large">{randomJoke}</p></center>
</template>
---------------------- JS ---------
import { LightningElement } from "lwc";

export default class RestTest extends LightningElement {
  randomJoke;
  connectedCallback() {
    const calloutURL = "https://icanhazdadjoke.com";
    fetch(calloutURL, {
      method: "GET",
      headers: {
        Accept: "application/json"
      }
    })
      .then((response) => {
        if (response.ok) {
          return response.json();
        }
      })
      .then((responseJSON) => {
        this.randomJoke = responseJSON.joke;
      });
  }
}