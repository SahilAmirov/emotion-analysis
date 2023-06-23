<template>
  <div>
    <h1>RapidAPI Post İsteği Örneği</h1>
    <button @click="makePostRequest">İstek Yap</button>
    <div v-if="response">
      <h2>İstek Sonucu</h2>
      <pre>{{ response }}</pre>
    </div>
    <button><a href="/translate/">Go Translate</a></button>
  </div>
</template>

<script>
import axios from 'axios';

export default {
  data() {
    return {
      response: null,
      inputText: null,
    };
  },
  methods: {
    async makePostRequest() {
  const encodedParams = new URLSearchParams();
  encodedParams.set(
    'text',
    'After living abroad for such a long time, seeing my family was the best present I could have ever wished for.'
  );

  const options = {
    method: 'POST',
    url: 'https://twinword-emotion-analysis-v1.p.rapidapi.com/analyze/',
    headers: {
      'content-type': 'application/x-www-form-urlencoded',
      'X-RapidAPI-Key': '013fa30825msh4c48907d61eeba0p13f979jsne53d24cda225',
      'X-RapidAPI-Host': 'twinword-emotion-analysis-v1.p.rapidapi.com',
    },
    data: encodedParams,
  };

  try {
    const response = await axios.request(options);
    this.response = response.data;
    console.log(response.data);
  } catch (error) {
    console.error(error);
  }
},

},
};
</script>
