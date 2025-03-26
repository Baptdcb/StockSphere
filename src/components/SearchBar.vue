<template>
    <div class="recherche">
        <input v-model="query" @input="fetchSuggestions" placeholder="Search for a stock" />
        <div class="suggestion">
            <div class="suggestion-box" v-if="suggestions.length">
                <ul>
                    <li v-for="suggestion in suggestions" :key="suggestion.displaySymbol" class="suggestion-item">
                        <div class="suggestion-content" @click="selectSuggestion(suggestion.displaySymbol), storeStockName()">
                            {{ suggestion.description }} ({{ suggestion.displaySymbol }})  
                        </div>
                        <button class="favorite-button" @click.stop="addToFavorites(suggestion)">
                            <span>★</span>
                        </button>
                    </li>
                </ul>
            </div>
        </div>
    </div>
</template>

<script lang="ts">
import { defineComponent } from 'vue';
import axios from 'axios';

export default defineComponent({
    name: 'SearchBar',
    data() {
        return {
            query: '',
            suggestions: [] as Array<{ description: string; displaySymbol: string }>,
        };
    },
    methods: {
        async fetchSuggestions() {
            if (this.query.length < 2) {
                this.suggestions = [];
                return;
            }

            const apiKey = 'cvcjso9r01qhnuvtirp0cvcjso9r01qhnuvtirpg'; // Remplacez par votre clé API Finnhub
            const url = `https://finnhub.io/api/v1/search?q=${this.query}&exchange=US&token=${apiKey}`;

            try {
                const response = await axios.get(url);
                this.suggestions = response.data.result
                    .map((item: { symbol: string; description: string; displaySymbol: string; }) => ({
                        description: item.description,
                        displaySymbol: item.displaySymbol,
                    }))
                    .slice(0, 5); // Limiter les suggestions à 5 résultats
            } catch (error) {
                console.error('Erreur lors de la récupération des suggestions :', error);
            }
        },
        selectSuggestion(symbol: string) {
            this.query = symbol;
            this.suggestions = [];
            this.$emit('select', symbol);
        },
        storeStockName() {
            this.$emit('store', this.query);
            console.log('Stock name stored:', this.query);
        },
       
        
        addToFavorites(suggestion: { description: string; displaySymbol: string }) {
            // Emit an event to add this stock to favorites
            this.$emit('addFavorite', {
                symbol: suggestion.displaySymbol,
                name: suggestion.description,
                
            });
            console.log('Adding to favorites:', suggestion.displaySymbol);
        }
        
    },
});
</script>

<style scoped>
input {
    width: 100%;
    padding: 15px 0px 15px 20px;
    background-color: #00000095;
    backdrop-filter: blur(10px);
    height: 60px;
    color: rgb(255, 255, 255);
    border: 2px solid #140025;
    font-family: Arial, Helvetica, sans-serif;
    border-radius: 30px;
    font-size: 1.5em;
    z-index: 3;
    box-sizing: border-box;
    box-shadow: 0 0 15px rgba(174, 0, 255, 0.5), 0 0 30px rgba(255, 255, 255, 0.3); 
    position: relative; 
    transition: box-shadow 0.4s ease-in-out;

}

input:hover {
    box-shadow: 0 0 25px rgba(214, 67, 255, 0.856), 0 0 30px rgba(255, 255, 255, 0.3); 
    transition: box-shadow 0.3s ease-in-out;
    
}

input::placeholder {
    color: #3f045e; /* Changez la couleur selon vos préférences */
    
}
input:focus {
    outline: none;
    box-shadow: 0 0 25px rgba(214, 67, 255, 0.856), 0 0 30px rgba(255, 255, 255, 0.3); 

}
.recherche {
    display: flex;
    max-width: 800px;
    margin: 0 auto;
    box-sizing: border-box;
    height:12dvh;
}

.suggestion {
    position: relative;
    right: 800px;
}

.suggestion-box {
    position: absolute;
    width: 800px;
    box-sizing: border-box;
    background-color: #00000095;
    backdrop-filter: blur(10px);
    color: rgb(255, 255, 255);
    font-family: Arial, Helvetica, sans-serif;
    font-size: 1.5em;
    border-radius: 30px;
    z-index: 2;
    box-shadow: 0 0 15px rgba(137, 44, 220, 0.5), 0 0 30px rgba(137, 44, 220, 0.3);
    padding-top: 50px; 
}

ul {
    width: 100%;
    list-style-type: none;
    padding: 0;
}

li {
    padding-left: 15px;
    padding-right: 15px;
    cursor: pointer;
    border-bottom: 2px solid hsla(274, 93%, 27%, 0.09); 
    display: flex;
    justify-content: space-between;
    align-items: center;
    color: #ffffff;
    text-align: left;
}

li:hover {
    background-color: #52057B;
}

.suggestion-content {
    flex-grow: 1;
    cursor: pointer;
}

.favorite-button {
    background: none;
    border: none;
    color: #5e048f;
    cursor: pointer;
    font-size: 25px;
    padding: 0 0 0 10px;
    transition: color 0.2s;
}

.favorite-button:hover {
    color: rgb(183, 0, 255);
}

.favorite-button:focus {
    outline: none;
}
</style>
