<template>
    <div>
        <!-- <img src="/storage/icons/info-circle-black.svg" class="info"> si hay tiempo aplicamos la opción de cambiar entre opciones y tutorial-->
        <div class="d-flex flex-column align-items-center">
            <h2 class="ms-0 settings-font settings">AJUSTES DE PARTIDA</h2>
            <div class="d-flex justify-content-between align-items-center w-50">
                <h3 class="mb-0">NÚMERO DE RONDAS</h3>
                <select class="select-font select-settings" v-model="numberRounds">
                    <option>2</option>
                    <option>3</option>
                    <option>4</option>
                    <option>5</option>
                </select>
            </div>
            <div class="d-flex justify-content-between align-items-center w-50 mt-4">
                <h3 class="mb-0">TIEMPO POR RONDA</h3>
                <select class="select-font select-settings" v-model="roundTime">
                    <option>30</option>
                    <option>45</option>
                    <option>60</option>
                    <option>90</option>
                </select>
            </div>
            <div class="d-flex justify-content-between align-items-center w-50 mt-4">
                <h3 class="mb-0">DIFICULTAD</h3>
                <select class="select-font select-settings-dificulty" v-model="difficulty">
                    <option>Fácil</option>
                    <option>Medio</option>
                    <option>Difícil</option>
                </select>
            </div>
        </div>
    </div>
</template>

<script setup>
import { ref, watch , defineEmits } from 'vue';

// Función para enviar eventos al componente padre
const emits = defineEmits(['updateSettings']);

const numberRounds = ref(2);
const roundTime = ref(30);
const difficulty = ref('Fácil');


// Cada vez que cambia una de las configuraciones se emite al componente padre
watch([numberRounds, roundTime, difficulty], () => {
    emits('updateSettings', {
        numberRounds: numberRounds.value,
        roundTime: roundTime.value,
        difficulty: difficulty.value
    });
});

</script>

<style scoped>

@import './../style/gameSettings.css';

@media(max-width: 520px)
{
    h2
    {
        font-size: 2rem;
    }

    h3
    {
        font-size: 1rem;
    }

    .settings
    {
        margin-top: 40px;
    }
}

</style>