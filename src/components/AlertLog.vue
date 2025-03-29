<template>
    <div class="alert-log">
        <h3>🚨 Alert Log</h3>
        <ul>
            <li v-for="(alert, index) in alerts" :key="index" :class="alert.type">
                [{{ alert.time }}] {{ alert.message }}
            </li>
            <li v-if="alerts.length === 0" class="none">無異常訊息</li>
        </ul>
    </div>
</template>

<script setup>
    const props = defineProps({
        alerts: {
            type: Array,
            default: () => []
        }
    })
</script>

<style scoped>
.alert-log {
    width: 50%;
    margin-top: 16px;
    padding: 12px;  
    background: #fff;
    border: 1px solid #ccc;
    border-radius: 4px;
    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: center;
}

.alert-log h3 {
    margin-bottom: 10px;
}

.alert-log ul {
    list-style: none;
    padding: 0;
    margin: 0;
    font-family: monospace;
}

.alert-log li {
    padding: 4px 0;
}

.alert-log li.warning {
    color: #ff9800;
}

.alert-log li.danger {
    color: #d32f2f;
    font-weight: bold;
}

.alert-log li.none {
    color: #888;
}
</style>