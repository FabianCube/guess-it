<!-- Componente para Registrar -->
<template>
    <div class="card border-0 shadow-sm popup-login">
        <div class="w-100 p-2">
            <button @click="toggleRegister(), playHovers('/storage/sounds/click-back.mp3')" id="closeLogin">
                <img src="/storage/icons/arrow-left.svg" alt="">
            </button>
        </div>
        <div class="card-body">
            <form @submit.prevent="submitRegister">
                <div class="">
                    <div class="avatar-wrapper">
                        <div class="image-container">
                            <img :src="avatarImage" alt="Avatar" class="avatar-image">
                            <input v-model="registerForm.avatar_id" type="text" id="currentAvatarId" hidden>
                        </div>
                        <a class="change-avatar" @click="changeAvatar(), playHovers('/storage/sounds/pop-change-avatar.mp3')">
                            <img src="/storage/icons/next-avatar.svg" alt="Next-avatar">
                        </a>
                    </div>
                    <!-- Email -->
                    <div class="mb-3">
                        <label for="nickname" class="form-label">Nickname</label>
                        <input v-model="registerForm.nickname" id="nickname" type="text" class="form-control" autofocus>
                        <!-- Validation Errors -->
                        <div class="text-danger mt-1">
                            <div v-for="message in validationErrors?.nickname">
                                {{ message }}
                            </div>
                        </div>
                    </div>
                    <div class="mb-3">
                        <label for="email" class="form-label">{{ $t('email') }}</label>
                        <input v-model="registerForm.email" id="email" type="email" class="form-control"
                            autocomplete="username">
                        <!-- Validation Errors -->
                        <div class="text-danger mt-1">
                            <div v-for="message in validationErrors?.email">
                                {{ message }}
                            </div>
                        </div>
                    </div>
                    <!-- Password -->
                    <div class="mb-4">
                        <label for="password" class="form-label">
                            {{ $t('password') }}
                        </label>
                        <input v-model="registerForm.password" id="password" type="password" class="form-control"
                            autocomplete="current-password">
                        <!-- Validation Errors -->
                        <div class="text-danger-600 mt-1">
                            <div v-for="message in validationErrors?.password">
                                {{ message }}
                            </div>
                        </div>
                    </div>
                    <div class="mb-4">
                        <label for="password_confirmation" class="form-label">
                            {{ $t('confirm_password') }}
                        </label>
                        <input v-model="registerForm.password_confirmation" id="password_confirmation" type="password"
                            class="form-control" autocomplete="current-password">
                        <!-- Validation Errors -->
                        <div class="text-danger-600 mt-1">
                            <div v-for="message in validationErrors?.password_confirmation">
                                {{ message }}
                            </div>
                        </div>
                    </div>

                    <!-- Buttons -->
                    <div class="flex items-center justify-end mt-4">
                        <button 
                            @mouseenter="() => playHovers('/storage/sounds/hover1.mp3')"  
                            class="btn-default btn-register" :class="{ 'opacity-25': processing }" :disabled="processing">
                            REGISTRARSE
                        </button>
                    </div>
                </div>
            </form>
        </div>
    </div>
</template>

<script setup>

import useAuth from '@/composables/auth'
import { defineEmits, ref, onMounted } from 'vue';
import axios from 'axios';
import { useRouter } from 'vue-router';

const emits = defineEmits(['open-register']);
const { registerForm, validationErrors, processing, submitRegister } = useAuth();
const baseAvatar = '/storage/avatars/';
const router = useRouter();
const avatarImage = ref();
let avatarId = ref(1);
const hovers = ref(null);

const playHovers = (soundFile) => {
    hovers.value = new Audio(soundFile);
    hovers.value.volume = 0.25;
    hovers.value.play();
}

// Asigno el avatar_id para usarlo en registerForm (input)

// TODO añadir campos necesarios para el registro.
const loadAvatar = () => {
    axios.get('/api/get-avatar/' + avatarId.value)
    .then(({ data }) => {
        document.getElementById('currentAvatarId').value = avatarId;
        avatarImage.value = baseAvatar + data.image;
        })
        .catch(error => {
            console.error("Error al obtener el avatar: ", error);
        });
};

onMounted(loadAvatar);

// Método para cambiar el avatar
const changeAvatar = () => {
    if (avatarId.value === 4) {
        avatarId.value = 1;
    } else {
        avatarId.value += 1; // Incrementamos el ID
    }
    loadAvatar(); // Cargamos el nuevo avatar
};

function toggleRegister()
{
    emits('open-register');
}

</script>

<style scoped>

@import './../style/register.css';

</style>