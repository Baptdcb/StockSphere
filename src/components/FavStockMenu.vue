<template>
    <div>
        <div class="fav-stock-menu">

        <ul>
            <li v-for="(stock) in favStocks" :key="stock.symbol">
            
            <div class="stock" @click="$emit('ubdate',stock.symbol)">{{ stock.symbol }} </div>
            <button @click="$emit('remove', stock.symbol)">
                <img src="@/assets/images/croix.png" alt="close" width="12"  style="padding-top: 2px;">
            </button>
            </li>
        </ul>
        </div>
    </div>
</template>

<script lang="ts">
import { defineComponent } from 'vue';

export default defineComponent({
    name: 'FavStockMenu',
    props: {
        favStocks: {
            type: Array as () => { symbol: string, name: string }[],
            required: true
        }
    },
    methods: {
        logFavStocks() {
            console.log(this.favStocks);
        },
        
    }
});
</script>

<style scoped>
ul{
    display: flex;
    justify-content: center;
    padding: 0;
    gap: 10px;
}
li{
    width: 100px;
    list-style: none;
    display: flex;
    align-items: center;
    padding: 5px 5px; 
    justify-content: space-between;
    background-color: #18001f9a; 
    color: white; 
    border-radius: 20px; 
    font-size: 16px; 
}
.stock{
    padding-left: 10px;
    cursor: pointer;

}
button{
    background: none;
    color: rgba(54, 54, 54, 0.986);
    border: none;
    cursor: pointer;
}
</style>