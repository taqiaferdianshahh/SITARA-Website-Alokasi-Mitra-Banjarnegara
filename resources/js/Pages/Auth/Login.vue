<script setup>
import Checkbox from '@/Components/Checkbox.vue';
import GuestLayout from '@/Layouts/GuestLayout.vue';
import InputError from '@/Components/InputError.vue';
import InputLabel from '@/Components/InputLabel.vue';
import PrimaryButton from '@/Components/PrimaryButton.vue';
import TextInput from '@/Components/TextInput.vue';
import { Head, Link, useForm } from '@inertiajs/vue3';

defineProps({
    canResetPassword: {
        type: Boolean,
    },
    status: {
        type: String,
    },
});

const form = useForm({
    email: '',
    password: '',
    remember: false,
});

const submit = () => {
    form.post(route('login'), {
        onFinish: () => form.reset('password'),
    });
};
</script>

<template>
    <GuestLayout>
        <Head title="Log in" />

        <!-- Menambahkan Logo Responsif dan Diletakkan di Tengah -->
        <div class="flex justify-center mb-6">
            <img src="/images/logo.png" alt="Logo" class="logo" />
        </div>

        <div v-if="status" class="mb-4 text-sm font-medium text-green-600">
            {{ status }}
        </div>

        <form @submit.prevent="submit">
            <div>
                <InputLabel for="email" value="Email" />

                <TextInput
                    id="email"
                    type="email"
                    class="mt-1 block w-full"
                    v-model="form.email"
                    required
                    autofocus
                    autocomplete="username"
                />

                <InputError class="mt-2" :message="form.errors.email" />
            </div>

            <div class="mt-4">
                <InputLabel for="password" value="Password" />

                <TextInput
                    id="password"
                    type="password"
                    class="mt-1 block w-full"
                    v-model="form.password"
                    required
                    autocomplete="current-password"
                />

                <InputError class="mt-2" :message="form.errors.password" />
            </div>

            <div class="mt-4 block">
                <label class="flex items-center">
                    <Checkbox name="remember" v-model:checked="form.remember" />
                    <span class="ms-2 text-sm text-gray-600"
                        >Remember me</span
                    >
                </label>
            </div>

            <div class="mt-4 flex items-center justify-end">
                <Link
                    v-if="canResetPassword"
                    :href="route('password.request')"
                    class="rounded-md text-sm text-gray-600 underline hover:text-gray-900 focus:outline-none focus:ring-2 focus:ring-indigo-500 focus:ring-offset-2"
                >
                    Forgot your password?
                </Link>

                <!-- Menambahkan class untuk tombol login dan menempatkan di pojok kanan -->
                <PrimaryButton
                    class="ms-4 login-button"
                    :class="{ 'opacity-25': form.processing }"
                    :disabled="form.processing"
                >
                    Log in
                </PrimaryButton>
            </div>
        </form>
    </GuestLayout>
</template>

<style scoped>
/* Menambahkan gaya untuk logo agar responsif dan seimbang */
.logo {
    max-width: 100%;  /* Membuat logo menyesuaikan lebar kontainer */
    height: auto;     /* Memastikan aspek rasio logo tetap terjaga */
    width: 180px;     /* Ukuran default logo */
}

/* Membuat logo lebih kecil di perangkat mobile */
@media (max-width: 768px) {
    .logo {
        width: 150px; /* Ukuran logo lebih kecil di layar kecil */
    }
}

/* Gaya untuk tombol login */
.login-button {
    background-color: #17B1CF; /* Warna biru */
    color: white;
    padding: 10px 20px;
    border: none;
    border-radius: 5px;
    cursor: pointer;
    font-size: 16px;
}

/* Membuat tombol lebih responsif dan lebih kecil di perangkat mobile */
@media (max-width: 768px) {
    .login-button {
        width: 100%;  /* Tombol akan mengisi lebar penuh di perangkat kecil */
    }
}

.login-button:hover {
    background-color: #139CB4; /* Biru lebih gelap saat hover */
}

.login-button:disabled {
    background-color: #D1D5DB; /* Warna abu-abu saat disabled */
    cursor: not-allowed;
}
</style>
