<template>

    <div class="card app-widget bg-transparent">
        <div class="card-header d-flex justify-content-between align-items-center">

        <h6 class="m-0 pb-0 mb-0 d-inline-block">
            {{widget_index + 1}} - {{widget_data.name.replace(' ','_')}}
        </h6>

        <div class="btn-group float-end">

            <WidgetHeaderButton @click="update_panel(true)">
                <GearSVG w="12" h="12" c="var(--bs-light)" />
            </WidgetHeaderButton>

        </div>

        </div>
        <div class="card-body p-0">

            <!-- TradingView Widget BEGIN -->
            <div v-if="show_chart==true" class="chart-wrapper">
                <div :id="container_id" class="chart-container" />
            </div>
            <!-- TradingView Widget END -->

        </div>
    </div>

<TradingChartSP 
v-if="widget_data.show_panel==true" 
@update-panel="update_panel" 
:widget_data="widget_data" 
/>

</template>

<script>

import GearSVG from "@/components/svg/GearSVG.vue";
import WidgetHeaderButton from "@/components/elements/WidgetHeaderButton.vue";
import TradingChartSP from "@/components/sidepanels/TradingChartSP.vue";

export default {
    name: "TradingChartWidget",
    props: ['widget_index','widget_data'],
    data() {
        return {
            show_chart: true,
            container_id: 'tradingview_chart_container_' + Date.now().toString(),
            base_url: 'https://www.youtube.com/embed/',
            youtube_id: 'ewx1aoA4FwQ&',
        }
    },
    mounted(){

        this.load_chart();

    },
    methods: {
        move_widget(direction){

            //
            //
            this.$parent.move_widget(this.widget_index,direction);

        },
        delete_widget(){
            this.$parent.delete_widget(this.widget_index);
        },
        update_panel(v) {
            this.$parent.hide_all_panel();
            this.widget_data.show_panel = v;
        },

        //
        //
        update_session(){
            this.$parent.update_session();
        },
        
        //
        //
        load_chart(){

            //
            // 1. Create the script element
            const script = document.createElement('script');
            script.src = 'https://s3.tradingview.com/tv.js';
            script.async = true;

            //
            // 2. Define the configuration once the script loads
            script.onload = () => {
                if (window.TradingView) {
                new window.TradingView.widget({
                    "autosize": true,
                    "width": "100%",
                    "height": "100%",
                    "symbol": this.widget_data.code,
                    "interval": "D",
                    "timezone": "Etc/UTC",
                    "theme": "dark",
                    "style": "1",
                    "locale": "en",
                    "backgroundColor": "#0F0F0F",
                    "gridColor": "rgba(242, 242, 242, 0.06)",
                    "toolbar_bg": "#f1f3f6",
                    "enable_publishing": false,
                    "allow_symbol_change": false,
                    "calendar": false,
                    "details": false,
                    "hide_side_toolbar": true,
                    "hide_top_toolbar": true,
                    "hide_legend": true,
                    "hide_volume": false,
                    "hotlist": true,
                    "watchlist": [],
                    "withdateranges": false,
                    "compareSymbols": [],
                    "studies": [],
                    "container_id": this.container_id,
                });
                }
            };

            // 3. Append script to the document
            document.head.appendChild(script);

        },

    },
    components: {
        GearSVG,
        WidgetHeaderButton,
        TradingChartSP,
    },
};
</script>

<style scoped>

    .note-bullet {
        font-size: 0.75rem;
    }

    .note-bullet::before {
        content: "> ";
    }

</style>