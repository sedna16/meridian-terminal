<template>

    <div class="d-block w-100 h-100 d-flex align-items-center">

        <Bar
            id="my-chart-id"
            :options="chart_options"
            :data="chart_data"
        />

    </div>

</template>

<script>

import { Bar } from 'vue-chartjs'
import { 
    Chart as ChartJS, 
    Title, Tooltip, 
    Legend, 
    BarElement, 
    CategoryScale, 
    LinearScale 
} from 'chart.js'

ChartJS.register(
    Title, 
    Tooltip, 
    Legend, 
    BarElement, 
    CategoryScale, 
    LinearScale
)

export default {
    name: "BarChart",
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
        Bar,
    },
};
</script>

<style scoped>
/* Add any custom styles here if needed */
</style>