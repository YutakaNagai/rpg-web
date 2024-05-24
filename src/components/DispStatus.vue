<script setup>
const props = defineProps({
    // name: String,
    // hp: Number,
    // atk: Number,
    // def: Number,
    // spd: Number,
    character: Object,
})
</script>

<template>
    <div>
        <!-- <div>名前: {{ props.name }}</div>
        <div>HP: {{ props.hp }}</div>
        <div>攻撃: {{ props.atk }}</div>
        <div>防御: {{ props.def }}</div>
        <div>敏捷: {{ props.spd }}</div> -->
        <div>名前: {{ props.character.name }}</div>
        <div>HP: {{ props.character.mhp }}</div>
        <div>攻撃: {{ props.character.atk }}</div>
        <div>防御: {{ props.character.def }}</div>
        <div>敏捷: {{ props.character.spd }}</div>
    </div>
</template>