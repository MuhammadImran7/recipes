<script setup lang="ts">
import type { NuxtError } from '#app';

// Explicitly defining props for error handling
defineProps<{
    error: NuxtError;
}>();

// Recommended for resetting error state
const handleError = () => clearError({ redirect: '/' });
</script>

<template>
    <section class="flex min-h-screen flex-col items-center justify-center text-center">
        <!-- Error Code Display -->
        <h1 class="mb-2 text-9xl font-bold text-red-500">
            {{ error?.statusCode || 'Error' }}
        </h1>
        
        <!-- Error Message -->
        <p class="mb-10 text-xl sm:text-3xl">
            {{ error?.statusMessage || 'Something went wrong!' }}
        </p>
        
        <!-- Back to Home Button -->
        <NuxtLink to="/" class="px-6 py-3 text-white bg-dodgeroll-gold rounded-md text-lg">
            Go Home plz
        </NuxtLink>
    </section>
</template>
