<template>
    <div class="data-log">
        <h3>📝 資料紀錄區</h3>
        <ul>
            <li v-for="(log, index) in logs" :key="index">
                [{{ log.time }}] Flow: {{ log.flow.toFixed(1) }} | Pressure: {{ log.pressure.toFixed(1) }}
            </li>
            <li v-if="logs.length === 0" class="none">尚無紀錄</li>
        </ul>
    </div>
</template>

<script setup>
const props = defineProps({
    logs: {
        type: Array,
        default: () => []
    }
})
</script>

<style scoped>
.data-log {
    width: 100%;
    max-width: 600px;
    padding: 12px;
    margin-top: 16px;
    border: 1px solid #ccc;
    border-radius: 4px;
    background: #fff;
    font-family: monospace;
}

ul {
    list-style: none;
    padding: 0;
    margin: 0;
}

li {
    padding: 4px 0;
    border-bottom: 1px dashed #eee;
    font-size: 14px;
    color: #333;
}

li.none {
    color: #888;
}
</style>