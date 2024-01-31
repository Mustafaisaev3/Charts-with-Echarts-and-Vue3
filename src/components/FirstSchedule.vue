<template>
    <div class="first-chart-container" >
        <div class="first-chart-inner">
            <div class="first-chart-header">
                <h2>Общая выручка</h2>
                <div class="first-chart__info">
                    <div class="first-chart__info-item">
                        <h2>ОБЩЕЕ ОПЛАЧЕННО</h2>
                        <div>{{ totalSales }}</div>
                    </div>
                    <div 
                      class="first-chart__info-item"
                      v-for="item in ItemsData"
                    >
                        <h2>
                            <div :style="`background-color: ${item.color};`"></div>
                            {{ item.name }}
                        </h2>
                        <div>{{ currencyFormatter(item.count) }}</div>
                    </div>
                </div>
            </div>
            <div class="first-chart" ref="chartElementRef">
            </div>
        </div>
    </div>
</template>
<script setup lang="ts">
import * as echarts from 'echarts'
import { ref, computed, onMounted } from 'vue';

const ItemsData = [
    {
        name: 'Деньги за мясо',
        color: '#13d6ff ',
        data: [120, 132, 101, 134, 90, 230, 210, 101, 134, 90, 230, 210],
        count: [120000, 132000, 101000, 134000, 90000, 230000, 210000, 101000, 134000, 90000, 230000, 210000].reduce((acc, curr) => acc + curr)
    },
    {
        name: 'Расходы на ЗП',
        color: '#0077f7',
        data: [220, 182, 191, 234, 290, 330, 310, 191, 234, 290, 330, 310],
        count: [220000, 182000, 191000, 234000, 290000, 330000, 310000, 191000, 234000, 290000, 330000, 310000].reduce((acc, curr) => acc + curr),
    },
    {
        name: 'Прочее',
        color: '#9747ff',
        data: [320, 332, 301, 334, 390, 330, 320, 301, 334, 390, 330, 320],
        count: [320000, 332000, 301000, 334000, 390000, 330000, 320000, 301000, 334000, 390000, 330000, 320000].reduce((acc, curr) => acc + curr),
    },
]

const totalSales = computed(() => {
    let total = 0

    ItemsData.map(item => {
        total += item.count
    })

    let totalString = new Intl.NumberFormat('ru-RU', {
        style: 'currency',
        currency: 'RUB',
        minimumFractionDigits: 0, 

    }).format(total);

    return totalString
})

const currencyFormatter = computed(() => {
    return (total: number) => {
        let totalString = new Intl.NumberFormat('ru-RU', {
            style: 'currency',
            currency: 'RUB',
            minimumFractionDigits: 0, 
        }).format(total);
        return totalString
    }
})

const chartElementRef = ref(null)
let chart 

const chartOptions = computed(() => {
    let option = {
        legend: {
            show: false
        },
        grid: {
            left: '3%',
            right: '4%',
            bottom: '3%',
            containLabel: true
        },
        xAxis: [
            {
                type: 'category',
                data: ['Янв', 'Фев', 'Март', 'Апр', 'Май', 'Июнь', 'Июль', 'Авг','Сен','Окт','Ноя','Дек',]
            }
        ],
        yAxis: [
            {
                type: 'value'
            }
        ],
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
                        ${params[1].value}
                        </div>
                    </div>
                    <div>
                        <div style="font-size: 12px; color: gray">
                            ${params[2].seriesName}
                        </div>
                        <div style="font-size: 14px; display: flex; align-items: center; gap: 5px">
                            <div style="width: 5px; height: 5px; border-radius: 100%; background-color: ${params[2].color}"></div>
                        ${params[2].value}
                        </div>
                    </div>
                </div>
                `;
            },
        },
        series: [
            {
                name: 'Прочее',
                type: 'bar',
                stack: 'Ad',
                emphasis: {
                    focus: 'series'
                },
                data: [320, 332, 301, 334, 390, 330, 320, 301, 334, 390, 330, 320]
                },
            {
                name: 'ЗП',
                type: 'bar',
                stack: 'Ad',
                emphasis: {
                    focus: 'series'
                },
                data: [120, 132, 101, 134, 90, 230, 210, 101, 134, 90, 230, 210]
            },
            {
                name: 'Мясо',
                type: 'bar',
                stack: 'Ad',
                emphasis: {
                    focus: 'series'
                },
                data: [220, 182, 191, 234, 290, 330, 310, 191, 234, 290, 330, 310]
            },
            {
                type: 'line',
                symbol: 'none',
                smooth: 0.6,
                lineStyle: {
                    color: '#b3f260',
                    width: 2
                },
                data: [320, 282, 291, 334, 390, 430, 410, 291, 334, 390, 430, 410],
            }
        ],
        backgroundColor: '#1b1b1e',
        color: ['#9747ff', '#0077f7', '#13d6ff',]
    };

    return option
})

onMounted(() => {
    chart = echarts.init(chartElementRef.value, 'dark')
    chart.setOption(chartOptions.value)
})
</script>

<style scoped>
.first-chart-container {
    width: 100%;
    height: auto;
    padding: 50px 0px;
    display: flex;
    justify-content: center;
    align-items: center;
}

.first-chart-inner {
    width: 1000px;
    height: auto;
    border-radius: 10px;
    background: #1b1b1e;
}

.first-chart-header {
    width: 100%;
    height: auto;
    padding: 10px 30px;
}

.first-chart-header h2 {
    color: white;
}

.first-chart__info {
    width: 100%;
    display: grid;
    grid-template-columns: repeat(4, 1fr)
}

.first-chart__info-item {
    width: 100%;
    height: auto;
    border-left: 1px solid #272b33;
    padding-left: 20px;
}

.first-chart__info-item:first-child {
    border-left: none;
    padding-left: none;
}

.first-chart__info-item h2 {
    color: gray;
    font-size: 16px;
    text-transform: uppercase;
    display: flex;
    align-items: center;
    gap: 5px;
}
.first-chart__info-item h2 > div {
    width: 5px;
    height: 5px;
    border-radius: 100%;
}

.first-chart__info-item div {
    color: white;
    font-size: 30px;
}

.first-chart {
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