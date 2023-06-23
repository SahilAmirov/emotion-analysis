<template>
  <div>
    <h1>RapidAPI Post İsteği Örneği</h1>
    <textarea rows="4" v-model="textInput"></textarea>
    <select v-model="selectedLanguage">
      <option v-for="(name, code) in langDict" :value="code" :key="code">
        {{ name }}
      </option>
    </select>

    <button @click="makePostRequest(textInput)">CEVIR</button>

    <div v-if="textOutput">
      <br />
      <span>{{ langDict[selectedLanguage] }} diline cevirisi: </span>
      <pre style="background-color: green">{{ textOutput }}</pre>
    </div>
  </div>
</template>

<script>
import axios from "axios";

export default {
  created() {
    this.getLangs();
    this.selectedLanguage = "tr";
  },

  data() {
    return {
      response: null,
      langs: null,
      langDict: {},
      selectedLanguage: null,
      textInput: null,
      textOutput: null,
    };
  },
  methods: {
    async makePostRequest(text) {
      const encodedParams = new URLSearchParams();
      encodedParams.set("source_language", "en");
      encodedParams.set("target_language", this.selectedLanguage);
      encodedParams.set("text", text);

      const options = {
        method: "POST",
        url: "https://text-translator2.p.rapidapi.com/translate",
        headers: {
          "content-type": "application/x-www-form-urlencoded",
          "X-RapidAPI-Key":
            "013fa30825msh4c48907d61eeba0p13f979jsne53d24cda225",
          "X-RapidAPI-Host": "text-translator2.p.rapidapi.com",
        },
        data: encodedParams,
      };

      try {
        const response = await axios.request(options);
        this.textOutput = response.data.data.translatedText;
        //console.log(response.data);
      } catch (error) {
        console.error(error);
      }
    },

    async getLangs() {
      const options = {
        method: "GET",
        url: "https://text-translator2.p.rapidapi.com/getLanguages",
        headers: {
          "X-RapidAPI-Key":
            "013fa30825msh4c48907d61eeba0p13f979jsne53d24cda225",
          "X-RapidAPI-Host": "text-translator2.p.rapidapi.com",
        },
      };

      try {
        const response = await axios.request(options);
        this.langs = response;
        //console.log(response.data);
        this.editLangs();
      } catch (error) {
        console.error(error);
      }
    },

    editLangs() {
      if (this.langs !== null) {
        this.langs.data.data.languages.forEach((language) => {
          this.$set(this.langDict, language.code, language.name);
        });
        //console.log(this.langDict);
      }
    },
  },
};
</script>
