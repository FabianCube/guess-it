<template>
    <div class="card border-0 shadow-sm popup-login">
        <div class="w-100">
            <button @click="toggleLogin()" id="closeLogin">
                <img src="/storage/icons/arrow-left.svg" alt="">
            </button>
        </div>
        <div class="card-body" style="width: 50%;">
            <h1 style="text-align: center;">INICIAR SESIÓN</h1>
            <form @submit.prevent="submitLogin">
                <div class="">
                    <!-- Email -->
                    <div class="mb-3">
                        <label for="email" class="form-label">{{ $t('email') }}</label>
                        <input v-model="loginForm.email" id="email" type="email" class="form-control" required autofocus
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
                        <input v-model="loginForm.password" id="password" type="password" class="form-control" required
                            autocomplete="current-password">
                        <!-- Validation Errors -->
                        <div class="text-danger-600 mt-1">
                            <div v-for="message in validationErrors?.password">
                                {{ message }}
                            </div>
                        </div>
                    </div>
                    <!-- Remember me -->
                    <div class="form-check">
                        <input class="form-check-input" type="checkbox" name="remember" v-model="loginForm.remember"
                            id="flexCheckIndeterminate">
                        <label class="form-check-label" for="flexCheckIndeterminate">
                            {{ $t('remember_me') }}
                        </label>
                    </div>

                    <!-- Buttons -->
                    <div class="flex items-center justify-end mt-4">
                        <button class="btn-default btn-login" :class="{ 'opacity-25': processing }" :disabled="processing">
                            ENTRAR!
                        </button>
                    </div>
                </div>
                <router-link :to="{ name: 'auth.forgot-password' }">
                    {{ $t('forgot_password') }}
                </router-link>
            </form>
        </div>
    </div>


</template>

<script setup>
import { defineProps, defineEmits } from 'vue';
import useAuth from '@/composables/auth';

const emits = defineEmits(['close-popup']);
const { loginForm, validationErrors, processing, submitLogin } = useAuth();

function toggleLogin() {
    emits('close-popup');
}
</script>
<style scoped>
#closeLogin
{
    background-color: transparent;
    border: none;
}
.popup-login
{
    width: 800px;
    height: 500px;
    margin-left: auto;
    margin-right: auto;
    margin-top: 100px;
    border-radius: 30px;
    display: flex;
    justify-content: center;
    align-items: center;
    font-family: 'Lilita One', sans-serif;
}

.btn-login
{
    width: 100%;
    height: 65px;
    border: none;
    margin: 0 0 15px 0;
    line-height: 65px;
    font-size: 2rem;
}

.form-control
{
    border-radius: 12px!important;
    border: solid 2px #757575;
}
</style>