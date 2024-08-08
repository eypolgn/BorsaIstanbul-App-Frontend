<template>
  <div class="stock-details">
    <h1 class="title">Borsa İstanbul Hisse Verileri</h1>
    <input
      class="input"
      v-model="symbol"
      placeholder="Hisse Sembolü Girin"
      @keyup.enter="handleEnter"
    />
    <select class="select" v-model="selectedOption" @change="clearErrorMessage">
      <option disabled value="">Seçiniz</option>
      <option value="1d">Anlık Fiyatı</option>
      <option value="1mo">Son 1 Aylık Fiyat Verileri</option>
      <option value="3mo">Son 3 Aylık Fiyat Verileri</option>
      <option value="6mo">Son 6 Aylık Fiyat Verileri</option>
      <option value="1y">Son 1 Yıllık Fiyat Verileri</option>
      <option value="info">Hisse Senedi Bilgileri</option>
    </select>
    <button class="button" @click="getStockData">Göster</button>

    <div v-if="resultData" class="result-container">
      <h3 class="result-title">Sonuç:</h3>
      <pre>{{ resultData }}</pre>
    </div>

    <div v-if="errorMessage" class="error-message">{{ errorMessage }}</div>
  </div>
</template>

<script>
import axios from "axios";

export default {
  data() {
    return {
      symbol: "",
      selectedOption: "",
      resultData: null,
      errorMessage: "",
    };
  },
  methods: {
    handleEnter() {
      if (!this.selectedOption) {
        this.errorMessage = "Lütfen bir seçenek seçin.";
      } else {
        this.errorMessage = "";
        this.getStockData();
      }
    },
    clearErrorMessage() {
      this.errorMessage = "";
    },
    async getStockData() {
      try {
        const response = await axios.get(
          `http://localhost:5000/api/stock/${this.symbol}`,
          {
            params: { option: this.selectedOption },
          }
        );
        this.resultData = response.data;
      } catch (error) {
        this.errorMessage = "Hisse verisi alınamadı.";
      }
    },
  },
};
</script>

<style>
@import url("https://fonts.googleapis.com/css2?family=Roboto:wght@400;700&display=swap");

body {
  font-family: "Roboto", sans-serif;
  background-color: rgb(189, 189, 189);
  margin: 0;
}

.stock-details {
  margin: 10px auto;
  width: calc(100% - 40px);
  max-width: 600px;
  padding: 20px;
  box-shadow: 0 0 10px rgba(0, 0, 0, 0.2);
  background-color: #ffffff;
  border-radius: 8px;
}

.title {
  text-align: center;
  color: #333;
  font-size: 24px;
}

.input,
.select,
.button {
  width: 100%;
  margin: 10px 0px;
  padding: 10px;
  border: 1px solid #ddd;
  border-radius: 4px;
  box-sizing: border-box;
}

.input,
.select {
  max-width: 100%; 
  margin: 10px auto;
}

.button {
  max-width: 150px;
  margin: 20px auto;
  display: block;
  background-color: #007bff;
  color: white;
  cursor: pointer;
  border: none;
}

.input:focus,
.select:focus {
  border-color: #007bff;
  outline: none;
}

.button:hover {
  background-color: #0056b3;
}

.result-container {
  margin-top: 20px;
  padding: 10px;
  border: 1px solid #007bff;
  border-radius: 4px;
  background-color: #f0f8ff;
}

.result-title {
  margin: 0;
}

.error-message {
  color: red;
  margin-top: 10px;
  text-align: center;
}
</style>
