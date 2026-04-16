<template>

    <div class="d-block w-100 h-100 py-0 px-5 d-flex align-items-center">

        <Doughnut
            id="my-chart-id"
            :options="chart_options"
            :data="chart_data"
        />

    </div>

</template>

<script>

import { Doughnut } from 'vue-chartjs'
import { 
    Chart as ChartJS, 
    ArcElement, Tooltip, Legend
} from 'chart.js'

ChartJS.register(
    ArcElement, Tooltip, Legend
)

ChartJS.register(ArcElement, Tooltip, Legend)

export default {
    name: "PieChart",
    props: ['chart_data','chart_options'],
    data() {
        return {
            chartData: {
                labels: [ 'January', 'February', 'March' ],
                datasets: [{ 
                    label: 'Sales',
                    data: [40, 20, 12,10,10,10],
                    backgroundColor: [ // dots
                        'rgba(0, 255, 65, 0.8)',
                        'rgba(0, 255, 65, 0.7)',
                        'rgba(0, 255, 65, 0.6)',
                        'rgba(0, 255, 65, 0.4)',
                        'rgba(0, 255, 65, 0.2)',
                        'rgba(0, 255, 65, 0.1)',
                    ],
                    borderColor: [ //line
                        'rgba(0, 255, 65, 1)',
                        'rgba(0, 255, 65, 1)',
                        'rgba(0, 255, 65, 1)',
                        'rgba(0, 255, 65, 1)',
                        'rgba(0, 255, 65, 1)',
                        'rgba(0, 255, 65, 1)',
                    ],
                    borderWidth: 1, // line thickness
                }]
            },
            chartOptions: {
                responsive: true,
                plugins: {
                    legend: {
                        display: false
                    }
                }
            }
        }
    },
    created(){

    },
    methods: {

    },
    components: {
        Doughnut,
    },
};
</script>

<style scoped>
/* Add any custom styles here if needed */

    .color1 {
        color: rgb(0, 255, 65);
    }

</style>