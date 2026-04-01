<template>

    <div class="card app-widget bg-transparent">
        <div class="card-header d-flex justify-content-between align-items-center">

        <h6 class="m-0 pb-0 mb-0 d-inline-block">Metrics</h6>

        <div class="btn-group float-end">

            <WidgetHeaderButton @click="show_panel=true">O</WidgetHeaderButton>

        </div>

        </div>
        <div class="card-body p-3">

            <ChartVisualizer :chart_data="chart_data" />

        </div>
    </div>

<MetricsSP 
v-if="show_panel==true" 
@update-panel="update_panel" 
@update-chart-data="update_chart_data" 
:chart_data="chart_data" 
/>

</template>

<script>

import WidgetHeaderButton from "@/components/elements/WidgetHeaderButton.vue";
import MetricsSP from "@/components/sidepanels/MetricsSP.vue";
import ChartVisualizer from "@/components/elements/ChartVisualizer.vue";

export default {
    name: "MetricsWidget",
    props: ['widget_index'],
    data() {
        return {
            show_panel: false,
            chart_data: {
                'type': 'doughnut',
                'title': 'Monthly Sales',
                'show_title': true,
                'labels': ['Alpha','Beta'],
                'show_labels': true,
                'dataset': [10,100],
                'color_theme': {
                    'r': '0',
                    'g': '255',
                    'b': '65',
                },
            },
        }
    },
    methods: {
        update_panel(v) {
            this.show_panel = v;
        },
        update_chart_data(d){
            this.chart_data = d;
        },
    },
    components: {
        WidgetHeaderButton,
        MetricsSP,
        ChartVisualizer,
    },
};
</script>

<style scoped>
/* Add any custom styles here if needed */
</style>