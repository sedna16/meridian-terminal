<template>

    <div class="d-block" style="opacity: 1.0 !important;">

        <Line
            id="my-chart-id"
            :options="chart_options"
            :data="chart_data"
        />

    </div>

</template>

<script>

import { Line } from 'vue-chartjs'
import { 
    Chart as ChartJS,
    CategoryScale,
    LinearScale,
    PointElement,
    LineElement,
    Title,
    Tooltip,
    Legend
} from 'chart.js'

ChartJS.register(
    CategoryScale,
    LinearScale,
    PointElement,
    LineElement,
    Title,
    Tooltip,
    Legend
)

ChartJS.defaults.font.family = "'Space Grotesk', Helvetica, Arial, sans-serif"; //
ChartJS.defaults.font.size = 12; // Default is 12
ChartJS.defaults.font.weight = 'normal'; //

export default {
    name: "LineChart",
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
        Line,
    },
};
</script>

<style scoped>
/* Add any custom styles here if needed */
</style>