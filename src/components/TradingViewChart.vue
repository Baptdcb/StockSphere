<template>
    <div class="tradingview-widget-container">
        <div id="tradingview-widget" class="tradingview-widget-container__widget"></div>
        <div class="tradingview-widget-copyright">
            
        </div>
    </div>
</template>
    
<script>
export default {
    name: 'TradingViewChart',
    props: {
        symbol: {
            type: String,
            required: true
        },
        variation: {
            type: Number,
            required: true
        }
    },
    watch: {
        symbol() {
            this.symbolChanged = true;
        },

        variation() {
            if (this.symbolChanged) {
                this.updateChart();
            }
        }
    },
    methods: {
        updateChart() {
        const container = document.getElementById('tradingview-widget');
        if (container) {
            container.innerHTML = '';
        }
        this.createChart();
        },
        createChart() {
            const script = document.createElement('script');
            let couleur;
            if (this.variation < 0) {
                couleur = 'rgb(255, 0, 51)';
            } else if (this.variation > 0) {
                couleur = 'rgb(0, 208, 0)';
            } else {
                couleur = 'rgb(44, 0, 62)'; 
            }
            console.log('Couleur:', couleur);
            script.src = 'https://s3.tradingview.com/external-embedding/embed-widget-symbol-overview.js';
            script.async = true;
            script.innerHTML = JSON.stringify({
                "symbols": [
                    [
                        this.symbol,
                        `${this.symbol}|1D`
                    ]
                ],
                "chartOnly": true,
                "width": "100%",
                "height": "100%",
                "locale": "en",
                "colorTheme": "dark",
                "autosize": true,
                "showVolume": false,
                "showMA": false,
                "hideDateRanges": false,
                "hideMarketStatus": false,
                "hideSymbolLogo": false,
                "scalePosition": "right",
                "scaleMode": "Normal",
                "fontFamily": "-apple-system, BlinkMacSystemFont, Trebuchet MS, Roboto, Ubuntu, sans-serif",
                "fontSize": "10",
                "noTimeScale": false,
                "valuesTracking": "1",
                "changeMode": "price-and-percent",
                "chartType": "area",
                "maLineColor": "#2962FF",
                "maLineWidth": 1,
                "maLength": 9,
                "headerFontSize": "medium",
                "backgroundColor": "rgba(0, 188, 212, 0)",
                "lineWidth": 2,
                "lineType": 0,
                "dateRanges": [
                    "1d|1",
                    "1m|30",
                    "3m|60",
                    "12m|1D",
                    "60m|1W",
                    "all|1M"
                ],
                "lineColor": couleur,
                "topColor": "rgba(0, 0, 0, 0)",
                "bottomColor": "rgba(0, 0, 0, 0)"
            });
            
            document.getElementById('tradingview-widget').appendChild(script);
        }
    },
    mounted() {
        this.createChart();
    }
}
</script>
    
<style scoped>
.tradingview-widget-container {
    width: 100%;
    height: 500px; 
}
</style>