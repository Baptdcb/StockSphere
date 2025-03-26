<template>
  <div>
    <FavStockMenu class="FavStockMenu" :favStocks="favorites" @remove="removeFromFavorites" @ubdate="updateStockName"/>
    
  
    <SearchBar @store="updateStockName" @addFavorite="addToFavorites"/>
    <br>
    <br> 
    <div class="stock-info">
      <div class="stock-info-card" id="stockPrice">
        <div class="top-card">
            <div v-if="logoStockName">
              <img :src="logoStockName" alt="Logo de l'action" width="50" height="50">  
            </div>
            <div>Prix de l'action {{ stockName }} :</div>
        </div>
        <div class="bottom-card">
          <div class="stock-price" v-if="stockPrice" > {{ stockPrice }} $ (USD) </div>
          <div v-else>Chargement...</div>
          <div class="variation-price" v-if="stockVariation">
            <div style="font-size: 0.8em;">Dernière 24h :</div>
            <div :class="{'negative': stockVariation < 0, 'positive': stockVariation >= 0}">
              <span v-if="stockVariation < 0">▼</span>
              <span v-else>▲</span>
              {{ Math.abs(stockVariation) }} % 
            </div>
          </div>
        </div>
      </div>
      <div class="stock-info-card" id="TradingView"> 
        <div class="top-card">
          <div>Graphique de l'action {{ stockName }} :</div>
        </div>
        <div class="bottom-card">
          <TradingViewChart :symbol="stockName" :variation="stockVariation"/>
        </div>
      </div>
    </div>
    <div v-if="error" class="error">{{ error }}</div>
  </div>
</template>

<script lang="ts">
import { defineComponent } from 'vue';
import axios from 'axios';
import SearchBar from './SearchBar.vue';
import TradingViewChart from './TradingViewChart.vue';
import FavStockMenu from './FavStockMenu.vue';

export default defineComponent({
  name: 'StockPrice',
  components: {
    SearchBar,
    TradingViewChart,
    FavStockMenu,
  },
  data() {
    return {
      stockName: 'AAPL', // Symbole de l'action
      newStockName: '',  // Nouveau symbole de l'action
      stockPrice: 0,     // Prix de l'action
      error: '',         // Message d'erreur
      logoStockName: '', // Logo de l'action
      stockVariation: 0, // Variation du prix de l'action
      favorites: [] as { symbol: string, name: string }[], // Liste des actions favorites
    };
  },
  mounted() {
    this.getStockPrice();
  },
  methods: {
    async getStockPrice() {
      const apiKey = 'cvcjso9r01qhnuvtirp0cvcjso9r01qhnuvtirpg'; // Remplacez par votre clé API Finnhub
      const symbol = this.stockName;
      const url = `https://finnhub.io/api/v1/quote?symbol=${symbol}&token=${apiKey}`;
      const url2 = `https://finnhub.io/api/v1/stock/profile2?symbol=${symbol}&token=${apiKey}`;
     
      try {
        const response = await axios.get(url);
        const response2 = await axios.get(url2);
        console.log('Réponse de l\'API Finnhub :', response.data);

        if (response.data && response.data.c) {
          this.stockPrice = response.data.c; // 'c' est le prix actuel
          console.log(`Prix actuel de ${symbol} : ${this.stockPrice}`);
          this.stockVariation = response.data.dp;
          this.error = '';
          this.logoStockName= response2.data.logo;
          console.log(`Logo de ${symbol} : ${this.logoStockName}`);
        } else {
          throw new Error('Invalid data format');
        }
      } catch (error) {
        this.error = `Erreur lors de la récupération du cours de l'action : ${(error as any).message}`;
        console.error(this.error);
      }
    },
 
    updateStockName(stockName: string) {
      this.stockName = stockName;
      this.getStockPrice();
    },
    addToFavorites(stock: { symbol: string, name: string  }) {
      if (!this.favorites.some(fav => fav.symbol === stock.symbol)) {
        this.favorites.push(stock);
      }
    },
    removeFromFavorites(stockSymbol: string) {
      this.favorites = this.favorites.filter(fav => fav.symbol !== stockSymbol);
    },
  },
});
</script>

<style scoped>
h1 {
  color: #ffffff;
}
p {
  font-size: 1.2em;
}
.negative {
  color: rgb(255, 0, 51);
}

.positive {
  color: rgb(0, 208, 0);
}
.error {
  color: red;
}
.stock-info {
  display: flex;
  justify-content: left;
  align-items: center;
  height: 40dvh;
  /* border: 5px solid #ff006af3; */
  gap: 5%;

}



#TradingView {
  margin-right: 15%;
  /*border: 5px solid #0c07fff3;*/
}

#stockPrice{
  margin-left: 15%;
  /*border: 5px solid #ffbd07f3;*/

}


.stock-info-card {
  width: 100%;
  height: 100%;
  border-radius: 30px;
  font-size: 1.5em;
  box-shadow: rgba(0, 0, 0, 0.4) 0px 0px 55px;
}
.top-card {
  display: flex;
  height: 30%;
  align-items: center;
  box-sizing: border-box;
  border-bottom: 3px solid #2e035328; 
  padding: 20px;
  
}

.top-card img {
  border-radius: 50%;
  padding: 10px;
}
.bottom-card {
  height: 70%;
  box-sizing: border-box;
  display: flex;
  padding: 20px;
}
.stock-price {
  width: 60%;
  height: 100%;
  align-self: start;
  box-sizing: border-box;
  font-size: 1.3em;
  display: flex;
  justify-content: center;
  align-items: center;
}
.variation-price {
  width: 40%;
  height: 100%;
  align-self: start;
  box-sizing: border-box;
  flex-direction: column;
  gap: 5px;
  font-size: 1em;
  display: flex;
  justify-content: center;
  align-items: center;
}

.FavStockMenu{
  padding: 5px 0;
}
</style>
