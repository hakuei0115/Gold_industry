
<template>
    <div class="sensor-card">
        <h3 class="card-title">{{ label }}</h3>
        <div class="card-values">
            <p>Flow: <span class="value">{{ flow.toFixed(1) }}</span></p>
            <p>Pressure: <span class="value">{{ pressure.toFixed(1) }}</span></p>
        </div>
    </div>
</template>

<script  setup>
    const props = defineProps({
        label: { type: String, default: 'Sensor' },
        flow: { type: Number, default: 0 },
        pressure: { type: Number, default: 0 },
    });
</script>

<style scoped>
.sensor-card {
    border: 1px solid #ccc;
    border-radius: 4px;
    padding: 12px;
    width: 200px;
    text-align: center;
    background-color: #fff;
    box-shadow: 0 2px 4px rgba(0,0,0,0.1);
    margin-bottom: 10px;
}

.card-title {
    margin-bottom: 8px;
    font-size: 16px;
    font-weight: bold;
    color: #333;
}

.card-values p {
    margin: 4px 0;
    font-size: 14px;
}

.value {
    font-weight: bold;
    color: #007bff;
}
</style>