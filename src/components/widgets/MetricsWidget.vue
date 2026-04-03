<template>

    <div class="card app-widget bg-transparent">
        <div class="card-header d-flex justify-content-between align-items-center">

        <h6 class="m-0 pb-0 mb-0 d-inline-block">
            {{widget_index + 1}} - {{widget_data.name.replace(' ','_')}}
        </h6>

        <div class="btn-group float-end">

            <WidgetHeaderButton @click="widget_data.show_panel=true">
                <GearSVG w="12" h="12" c="var(--bs-light)" />
            </WidgetHeaderButton>

        </div>

        </div>
        <div class="card-body p-3">

            <ChartVisualizer :chart_data="widget_data.chart_data" />

        </div>
    </div>

<MetricsSP 
v-if="widget_data.show_panel==true" 
@update-panel="update_panel" 
@update-chart-data="update_chart_data" 
:widget_data="widget_data" 
/>

</template>

<script>

import GearSVG from "@/components/svg/GearSVG.vue";
import WidgetHeaderButton from "@/components/elements/WidgetHeaderButton.vue";
import MetricsSP from "@/components/sidepanels/MetricsSP.vue";
import ChartVisualizer from "@/components/elements/ChartVisualizer.vue";

export default {
    name: "MetricsWidget",
    props: ['widget_index','widget_data'],
    data() {
        return {
            // chart_data: {
            //     'type': 'doughnut',
            //     'title': 'Monthly Sales',
            //     'show_title': true,
            //     'labels': ['Alpha','Beta'],
            //     'show_labels': true,
            //     //'dataset': [10,100],
            //     'data': [
            //         {
            //             'label': 'Alpha',
            //             'dataset': 10,
            //         },
            //         {
            //             'label': 'Beta',
            //             'dataset': 100,
            //         },
            //     ],
            //     'color_theme': {
            //         'r': '0',
            //         'g': '255',
            //         'b': '65',
            //     },
            // },
        }
    },
    methods: {
        move_widget(direction){

            //
            //
            this.$parent.move_widget(this.widget_index,direction);

        },
        delete_widget(){
            this.$parent.widgets_array.splice(this.widget_index,1);
        },
        update_panel(v) {
            this.$parent.hide_all_panel();
            this.widget_data.show_panel = v;
        },
        update_chart_data(d){
            this.widget_data.chart_data = d;
        },
        add_labels_dataset(){
            
            //
            //
            this.widget_data.chart_data.data.push(
                {
                    'label': 'Label',
                    'dataset': 10,
                }
            )
        },
    },
    components: {
        GearSVG,
        WidgetHeaderButton,
        MetricsSP,
        ChartVisualizer,
    },
};
</script>

<style scoped>
/* Add any custom styles here if needed */
</style>