<script setup lang="ts">
import { type Recipe } from "../../../types/types";
import { useRoute, useFetch, showError } from 'nuxt/app';

const { id } = useRoute().params;

// Fetching the recipe data
const { data, error } = await useFetch<Recipe>(`https://dummyjson.com/recipes/${id}`);

if (error?.value) {
    showError({
        statusCode: error.value.statusCode || 500,
        statusMessage: error.value.statusMessage || "An error occurred"
    });
}
</script>

<template>
    <div class="flex flex-col max-w-screen-lg container py-20">
        <!-- Recipe Title and Info -->
        <div class="flex flex-col mb-6">
            <h2 class="text-5xl mb-4 font-semibold">{{ data?.name || 'No Recipe Found' }}</h2>
            <div class="flex gap-4 text-xl mb-6">
                <div class="flex items-center gap-1">
                    <Icon name="mdi:clock-time-eight-outline" style="color: #f79f1a" />
                    <span>{{ data?.cookTimeMinutes || 'N/A' }}</span>
                </div>
                <div class="flex items-center gap-1">
                    <Icon name="mdi:fire" style="color: #f79f1a" />
                    <span>{{ data?.caloriesPerserving || 'N/A' }}</span>
                </div>
                <div class="flex items-center gap-1">
                    <Icon name="mdi:star" style="color: #f79f1a" />
                    <span>{{ data?.rating || '0' }} ({{ data?.reviewCount || '0' }})</span>
                </div>
            </div>
            <hr />
        </div>

        <!-- Recipe Image -->
        <NuxtImg
            :src="data?.image"
            densities="x1"
            sizes="xs:100vw sm:100vw md:100vw lg:100vw"
            class="w-full max-h-[500px] object-cover rounded-md shadow-sm mb-12"
        />

        <!-- Ingredients List -->
        <div class="mb-8">
            <h2 class="text-3xl font-semibold mb-4">Ingredients</h2>
            <ul class="grid grid-cols-1 md:grid-cols-2 gap-2 text-lg">
                <li v-for="(ingredient, index) in data?.ingredients" :key="index">
                    <label class="flex gap-2 items-center">
                        <input type="checkbox" class="hidden peer" />
                        <div class="relative w-6 h-6 rounded-full border-2 border-dodgeroll-gold flex items-center justify-center"></div>
                        <span class="peer-checked:line-through">
                            {{ ingredient }}
                        </span>
                    </label>
                </li>
            </ul>
        </div>

        <!-- Instructions Section -->
        <div>
            <h2 class="text-3xl font-medium mb-4">Instructions</h2>
            <ul class="flex flex-col text-lg gap-4">
                <li v-for="(instruction, index) in data?.instructions" :key="index" class="flex gap-2">
                    <span class="flex items-center justify-center bg-dodgeroll-gold w-7 h-7 rounded-full text-white text-sm">
                        {{ index + 1 }}
                    </span>
                    <span class="flex-1">{{ instruction }}</span>
                </li>
            </ul>
        </div>
    </div>
</template>
