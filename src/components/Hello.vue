<script setup>
import { ref, reactive } from "vue"
const count = ref(1);
const state = reactive({
    count: 1,
});

const addRefCount = () => {
    count.value++;
};

const addReactiveCount = () => {
    state.count++;
}
</script>

<template>
    <h2>初めてのコンポーネント</h2>
    <p>Ref Count:{{ count }}</p>
    <p>Reactive Count:{{  state.count }}</p>

    <div>
        <button @click="addRefCount">Ref Count+</button>
        <button @click="addReactiveCount">Reactive Count+</button>
    </div>
</template>