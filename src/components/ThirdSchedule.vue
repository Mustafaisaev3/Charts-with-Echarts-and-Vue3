<template>
    <div class="third-chart-container" >
        <div class="third-chart-inner">
            <div class="third-chart-header">
                <button class="button">Всё</button>
                <button class="button">
                    <div style="background-color: #18a0fb;"></div>
                    План
                </button>
                <button class="button">
                    <div style="background-color: #13d6ff;"></div>
                    Факт
                </button>
            </div>
            <div class="third-chart" ref="chartElementRef">
            </div>
        </div>
    </div>
</template>
<script setup lang="ts">
import * as echarts from 'echarts'
import { ref, computed, onMounted } from 'vue';

const chartElementRef = ref(null)
let chart 

const chartOptions = computed(() => {
    let option = {
        xAxis: [
            {
                type: 'category',
                boundaryGap: false,
                axisLine: {
                    show: false
                },
                data: ['01','02','03','04','05','06','07','08','09','10','11','12','13','14','15','16','17','18','19','21','22','23','24','25','26','27','28','29','30','31']
            }
        ],
        yAxis: {
            type: 'value'
        },
        tooltip: {
            trigger: 'axis',
            position: 'top',
            textStyle: {
                color: '#fff',
            },
            borderColor: '#212226',
            backgroundColor: '#212226',
            formatter: (params) => {
                return `
                <div style="display: flex; gap: 20px">
                    <div>
                        <div style="font-size: 12px; color: gray">
                            ${params[0].seriesName}
                        </div>
                        <div style="font-size: 14px; display: flex; align-items: center; gap: 5px">
                            <div style="width: 5px; height: 5px; border-radius: 100%; background-color: ${params[0].color}"></div>
                         ${params[0].value}
                        </div>
                    </div>
                    <div>
                        <div style="font-size: 12px; color: gray">
                            ${params[1].seriesName}
                        </div>
                        <div style="font-size: 14px; display: flex; align-items: center; gap: 5px">
                            <div style="width: 5px; height: 5px; border-radius: 100%; background-color: ${params[1].color}"></div>
                         ${params[0].value}
                        </div>
                    </div>
                </div>
                `;
            },
        },
        legend: {
            show: false,
            type: 'scroll',
            data: [
                {
                    name: 'План',
                    icon: 'circle',
                    textStyle: {
                        color: 'white'
                    },
                    lineStyle: {
                        color: 'red',
                        width: 200
                    }
                },
                {
                    name: 'Факт',
                    icon: 'circle',
                    textStyle: {
                        color: 'white'
                    },
                    
                },
            ],
            itemGap: 10,
            align: 'left',
            top: 10,
            left: 50,
        },
        series: [
            {
                name: 'План',
                type: 'line',
                areaStyle: {
                    opacity: 0.9
                },
                emphasis: {
                    focus: 'series'
                },
                data: [60, 80, 150, 160, 180, 120, 100, 105, 108, 110, 115, 117, 120, 115, 123, 130, 133, 136, 140, 143, 147, 150, 152, 156, 160, 162, 165, 165, 164, 163, 168,]
            },
            {
                name: 'Факт',
                type: 'line',
                areaStyle: {
                    opacity: 0.3
                },
                emphasis: {
                    focus: 'series'
                },
                data: [90, 100, 120, 130, 140, 150, 165, 180, 200, 210, 215, 223, 240, 235, 245, 255, 270, 300, 330, 350, 365, 380, 390, 400, 410, 420, 430, 440, 450, 460, 480]
            }
        ],
        backgroundColor: '#1b1b1e',
        color: ['#8cbabb', '#13d6ff']
    }

    return option
})

onMounted(() => {
    chart = echarts.init(chartElementRef.value, 'dark')
    chart.setOption(chartOptions.value)
})
</script>

<style scoped>
.third-chart-container {
    width: 100%;
    height: auto;
    padding: 50px 0px;
    display: flex;
    justify-content: center;
    align-items: center;
}

.third-chart-inner {
    width: 1000px;
    height: auto;
    border-radius: 10px;
    background: #1b1b1e;
}

.third-chart-header {
    width: 100%;
    height: auto;
    padding: 30px 30px;
    display: flex;
    gap: 20px;
}

.third-chart-header .button {
    padding: 10px 20px;
    display: flex;
    color: #898994;
    align-items: center;
    gap: 10px;
    border-radius: 5px;
    border: none;
    background-color: #212226;
    cursor: pointer;
}

.third-chart-header .button:hover {
    opacity: 0.8;
}

.third-chart-header .button:first-child{
    color: white;
    background-color: #0077f7;
    color: white;
}

.third-chart-header .button > div{
    width: 5px;
    height: 5px;
    border-radius: 100%;
}

.third-chart {
    width: 1000px;
    height: 500px;
    border-radius: 10px;
    overflow: hidden;
}

.tooltip-custom {
    display: flex;
    gap: 20px;
    background: red;
}
</style>