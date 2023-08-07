<script setup lang="ts">
import type { Task } from '@/types';
import { nanoid } from 'nanoid';

const emit = defineEmits<{
    (e: 'add', payload: Task): void
}>();

const focused = ref(false);
const title = ref("");

function createTask(e: Event): void {
    if (title.value.trim()) {
        e.preventDefault();
        emit("add", {
            id: nanoid(),
            title: title.value.trim(),
            createdAt: new Date(),
        })
    }

    title.value = "";
}

function setFocused(value: boolean): void {
    focused.value = value;
}

const placeholder = computed(() => !focused.value ? '+ Add A Card' : 'Enter a title for this card')
</script>

<template>
    <div>
        <textarea 
            v-model="title"
            @keydown.tab="createTask"
            @keyup.enter="createTask"
            class="focus:bg-white focus:shadow resize-none rounded w-full border-none bg-transparent p-2 cursor-pointer outline-none"
            :class="{
                'h-7': !focused,
                'h-20': focused,
            }"
            @focus="setFocused(true)"
            @blur="setFocused(false)"
            :placeholder="placeholder"
        />
    </div>
</template>