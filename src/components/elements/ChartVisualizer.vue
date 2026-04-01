<template>

    <TextChart 
    v-if="chart_data.type=='text'" 
    :chart_data="chart_data" />

    <BarChart 
    v-if="chart_data.type=='bar'" 
    :chart_data="format_data(chart_data)" 
    :chart_options="format_options(chart_data)" />

    <LineChart 
    v-if="chart_data.type=='line'" 
    :chart_data="format_data(chart_data)" 
    :chart_options="format_options(chart_data)" />

    <PieChart 
    v-if="chart_data.type=='pie'" 
    :chart_data="format_data(chart_data)" 
    :chart_options="format_options(chart_data)" />

    <DoughnutChart 
    v-if="chart_data.type=='doughnut'" 
    :chart_data="format_data(chart_data)" 
    :chart_options="format_options(chart_data)" />


</template>

<script>

import BarChart from "@/components/charts/BarChart.vue";
import LineChart from "@/components/charts/LineChart.vue";
import PieChart from "@/components/charts/PieChart.vue";
import DoughnutChart from "@/components/charts/DoughnutChart.vue";
import TextChart from "@/components/charts/TextChart.vue";

export default {
    name: "ChartVisualizer",
    props: ['chart_data'],
    data() {
        return {
            var_data: '',
        }
    },
    methods: {
        
        format_data(d){

            //
            //
            if(d.type == 'bar' || d.type == 'line'){

                //
                //
                var return_value = {
                    labels: d.labels,
                    datasets: [{ 
                        label: d.title,
                        data: d.dataset,
                        backgroundColor: this.format_background(d),
                        borderColor: this.format_border(d),
                        borderWidth: 1, // line thickness
                    }]
                }

            }
            else {
                
                //
                //
                var return_value = {
                    labels: d.labels,
                    datasets: [{ 
                        label: d.labels,
                        data: d.dataset,
                        backgroundColor: this.format_background(d),
                        borderColor: this.format_border(d),
                        borderWidth: 1, // line thickness
                    }]
                }

            }

            //
            //
            //console.log(return_value);
            return return_value;

        },
        format_background(d){

            //
            //
            var data_count = d.dataset.length;
            var opacity = 0.8;
            var decrease = opacity / data_count;
            var palette = d.color_theme.r + ',' + d.color_theme.g + ',' + d.color_theme.b;
            var return_value = []

            //
            //
            for (let i = 0; i < data_count; i++) {
                return_value.push(
                    'rgba(' + palette + ', ' + String(opacity) + ')'
                );
                opacity -= decrease;
            }

            //
            //
            return return_value;

        },
        format_border(d){

            //
            //
            var data_count = d.dataset.length;
            var palette = d.color_theme.r + ',' + d.color_theme.g + ',' + d.color_theme.b;
            var return_value = []

            var ret = [ ]

            //
            //
            for (let i = 0; i < data_count; i++) {
                return_value.push(
                    'rgba(' + palette + ', 1)'
                )
            }

            //
            //
            return return_value;

        },
        format_options(d){

            //
            //
            var return_value = {
                responsive: true,
                plugins: {
                     title: {
                        display: true,
                        text: d.title
                    },
                    legend: {
                        display: d.show_labels
                    },
                    font: {
                        size: 24, // in px
                        family: 'Space Grotesk',
                        weight: 'normal'
                    },
                }
            }

            //
            //
            //console.log(return_value);
            return return_value;

        },

    },
    components: {
        BarChart,
        LineChart,
        PieChart,
        DoughnutChart,
        TextChart,
    },
};
</script>

<style scoped>
/* Add any custom styles here if needed */
</style>