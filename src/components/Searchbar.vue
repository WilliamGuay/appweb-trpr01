<script setup lang="ts">
import { ref } from "vue"

const searchBarValue = ref<string>("")

const props = defineProps<{
    search: (query: string) => void
}>()


function updateSearchBarValue() {
    props.search(searchBarValue.value)
}
</script>

<template>
    <input type="text" v-model="searchBarValue" placeholder="Recherchez un jeu" class="form-control"
        @input="updateSearchBarValue">
</template>