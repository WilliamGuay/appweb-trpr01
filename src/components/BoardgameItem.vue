<script setup lang="ts">
defineProps<{
    name: string
    price: number
    qtyStock: number
    bggScore: number
}>()
</script>

<template>
    <tr>
        <th scope="row">{{ name }}</th>
        <td>{{ bggScore }}</td>
        <td>{{ price }}</td>
        <td>{{ qtyStock }}</td>
        <td>
            <button type="button" class="btn btn-warning">Modifier</button>
            <button type="button" class="btn btn-danger">Retirer</button>
        </td>
    </tr>
</template>

<style></style>