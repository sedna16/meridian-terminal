<template>

    <div class="d-block py-0 px-5 w-100 h-100 d-flex align-items-center">

        <Pie
            id="my-chart-id"
            :options="chart_options"
            :data="chart_data"
        />

    </div>

</template>

<script>

import { Pie } from 'vue-chartjs'
import { 
    Chart as ChartJS, 
    ArcElement, Tooltip, Legend
} from 'chart.js'

ChartJS.register(
    ArcElement, Tooltip, Legend
)

export default {
    name: "PieChart",
    props: ['chart_data','chart_options'],
    data() {
        return {
            chartData: {
                labels: [ 'January', 'February', 'March' ],
                datasets: [{ 
                    label: 'Sales',
                    data: [40, 20, 12],
                    backgroundColor: [ // dots
                        'rgba(255, 99, 132, 1)',
                        'rgba(54, 162, 235, 1)',
                        'rgba(255, 206, 86, 1)',
                    ],
                    borderColor: [ //line
                        'rgba(255,99,132,1)',
                        'rgba(54, 162, 235, 1)',
                        'rgba(255, 206, 86, 1)',
                    ],
                    borderWidth: 1, // line thickness
                }]
            },
            chartOptions: {
                responsive: true,
                plugins: {
                    legend: {
                        display: true
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
        Pie,
    },
};
</script>

<style scoped>
/* Add any custom styles here if needed */
</style>