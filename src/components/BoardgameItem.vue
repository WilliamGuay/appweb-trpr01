<script setup lang="ts">
import type { Boardgame } from '../scripts/types';
import { ref, watch } from "vue"

const props = defineProps<{
    boardgame: Boardgame
    onEdit: (id: number) => void;
    onDelete: (id: number) => void;
    onDuplicate: (id: number) => void;
}>()

let quantityStatus = ref<string>("low")

updateQuantityStatus()

watch(props.boardgame, () => {
    updateQuantityStatus()
})

function updateQuantityStatus() {
    quantityStatus.value = "low"
    if (props.boardgame.qtyStock < 20 && props.boardgame.qtyStock > 5) {
        quantityStatus.value = "medium"
    }
    if (props.boardgame.qtyStock >= 20) {
        quantityStatus.value = "high"
    }
}
</script>

<template>
    <tr>
        <th scope="row">{{ boardgame.name }}</th>
        <td>{{ boardgame.bggScore }}</td>
        <td>{{ boardgame.price }}</td>
        <td>
            <p :class="'qtyBackground-' + quantityStatus"
                style="display: inline-block; padding-left: 5px; padding-right: 5px;">{{
                    boardgame.qtyStock }}</p>
        </td>
        <td>
            <button type="button" class="btn btn-outline-info" data-bs-toggle="modal"
                v-bind:data-bs-target="'#gameDetails' + boardgame.id">
                Détails
            </button>
            <button @click="onEdit(boardgame.id)" type="button" class="btn btn-outline-warning">Modifier</button>
            <button @click="onDuplicate(boardgame.id)" type="button" class="btn btn-outline-primary">Dupliquer</button>
            <button @click="onDelete(boardgame.id)" type="button" class="btn btn-outline-danger">Retirer</button>
        </td>
    </tr>

    <div class="modal fade" v-bind:id="'gameDetails' + boardgame.id" tabindex="-1" aria-labelledby="gameDetailsLabel"
        aria-hidden="true">
        <div class="modal-dialog">
            <div class="modal-content">
                <div class="modal-header">
                    <h5 class="modal-title" id="gameDetailsLabel">Modal title</h5>
                    <button type="button" class="btn-close" data-bs-dismiss="modal" aria-label="Close"></button>
                </div>
                <div class="modal-body">
                    <h3>{{ boardgame.name }}</h3>
                    <h5>Score BGG: {{ boardgame.bggScore }}</h5>
                    <h5>Prix: {{ boardgame.price }} $CAD</h5>
                    <h5>Quantité en inventaire: {{ boardgame.qtyStock }}</h5>
                    <p>Description: {{ boardgame.description }}</p>
                    <p>Nombre de joueurs: {{ boardgame.nbPlayer }}</p>
                    <p>Temps de jeu: {{ boardgame.playingTime }}</p>
                </div>
                <div class="modal-footer">
                    <button type="button" class="btn btn-secondary" data-bs-dismiss="modal">Close</button>
                </div>
            </div>
        </div>
    </div>
</template>

<style>
.qtyBackground-high {
    background: #4fd600;
    border-radius: 50%;
}

.qtyBackground-medium {
    background: #e3a600;
    border-radius: 50%;
}

.qtyBackground-low {
    background: #d62000;
    border-radius: 50%;
}
</style>