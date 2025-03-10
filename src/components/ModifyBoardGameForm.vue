<script setup lang="ts">
import { ref, watch } from "vue"
import type { Boardgame } from "../scripts/types";

const props = defineProps<{
    name: string
    price: string
    qtyStock: string
    bggScore: string
    nbPlayerMin: string
    nbPlayerMax: string
    playingTimeMin: string
    playingTimeMax: string
    description?: string

    editGame: (boardgame: Boardgame) => void
}>()

const nameError = ref<string>("")
const priceError = ref<string>("")
const qtyStockError = ref<string>("")
const nbPlayerMinError = ref<string>("")
const nbPlayerMaxError = ref<string>("")
const playingTimeMinError = ref<string>("")
const playingTimeMaxError = ref<string>("")
const bggScoreError = ref<string>("")

const editName = ref<string>(props.name)
const editPrice = ref<string>(props.price)
const editQtyStock = ref<string>(props.qtyStock)
const editBggScore = ref<string>(props.bggScore)
const editNbPlayerMin = ref<string>(props.nbPlayerMin)
const editNbPlayerMax = ref<string>(props.nbPlayerMax)
const editPlayingTimeMin = ref<string>(props.playingTimeMin)
const editPlayingTimeMax = ref<string>(props.playingTimeMax)
const editDescription = ref<string>(props.description == null ? "" : props.description)

watch(() => props.name, (val) => {
    editName.value = val
})

watch(() => props.price, (val) => {
    editPrice.value = val
})

watch(() => props.qtyStock, (val) => {
    editQtyStock.value = val
})

watch(() => props.bggScore, (val) => {
    editBggScore.value = val
})

watch(() => props.nbPlayerMin, (val) => {
    editNbPlayerMin.value = val
})

watch(() => props.nbPlayerMax, (val) => {
    editNbPlayerMax.value = val
})

watch(() => props.playingTimeMin, (val) => {
    editPlayingTimeMin.value = val
})

watch(() => props.playingTimeMax, (val) => {
    editPlayingTimeMax.value = val
})

watch(() => props.description, (val) => {
    editDescription.value = val == null ? "" : val
})

function editBoardGame() {
    if (validate()) {
        let newBoardGame: Boardgame = {
            id: Date.now(),
            name: editName.value,
            price: +editPrice.value,
            qtyStock: +editQtyStock.value,
            bggScore: +editBggScore.value,
            nbPlayer: editNbPlayerMin + "-" + editNbPlayerMax,
            playingTime: editPlayingTimeMin + "-" + editPlayingTimeMax,
            description: editDescription.value
        }

        props.editGame(newBoardGame)

        editBggScore.value = ""
        editName.value = ""
        editNbPlayerMax.value = ""
        editNbPlayerMin.value = ""
        editPlayingTimeMax.value = ""
        editPlayingTimeMin.value = ""
        editPrice.value = ""
        editQtyStock.value = ""
        editDescription.value = ""
    }
}

const validate = () => {
    let isValid = true

    if (!editName.value.trim()) {
        nameError.value = 'Le nom du jeu est requis.'
        isValid = false
    } else {
        nameError.value = ""
    }

    if (!editBggScore.value.trim() || isNaN(+editBggScore.value) || 0 > +editBggScore.value || +editBggScore.value > 10) {
        bggScoreError.value = 'Le score du jeu doit être un nombre entre 0 et 10.'
        isValid = false
    } else {
        bggScoreError.value = ""
    }

    if (!editPrice.value.trim() || isNaN(+editPrice.value) || 0 > +editPrice.value) {
        priceError.value = 'Le prix du jeu doit être un nombre avec une valeur minimale de 0.'
        isValid = false
    } else {
        priceError.value = ""
    }

    if (!editQtyStock.value.trim() || isNaN(+editQtyStock.value) || 0 > +editQtyStock.value) {
        qtyStockError.value = 'La quantité du jeu en stock doit être un nombre avec une valeur minimale de 0.'
        isValid = false
    } else {
        qtyStockError.value = ""
    }

    if (!editNbPlayerMin.value.trim() || isNaN(+editNbPlayerMin.value)) {
        nbPlayerMinError.value = 'La quantité minimale de joueurs est requise.'
        isValid = false
    }

    else if (+editNbPlayerMin.value.trim() < 1) {
        nbPlayerMinError.value = 'La quantité minimale de joueurs doit être plus grande que 1.'
        isValid = false
    } else {
        nbPlayerMinError.value = ""
    }

    if (editNbPlayerMax.value.trim()) {
        if (+editNbPlayerMax.value.trim() < +editNbPlayerMin.value || isNaN(+editNbPlayerMax.value)) {
            nbPlayerMaxError.value = 'La quantité maximale de joueurs ne peut pas être plus petite que la quantité minimale de joueurs et doit être un nombre.'
            isValid = false
        }
    } else {
        nbPlayerMaxError.value = ""
    }


    if (!editPlayingTimeMin.value.trim() || isNaN(+editPlayingTimeMin.value)) {
        playingTimeMinError.value = 'La quantité minimale de temps de jeu est requise.'
        isValid = false
    }

    else if (+editPlayingTimeMin.value.trim() < 0 && editPlayingTimeMax.value.trim()) {
        playingTimeMinError.value = 'La quantité minimale de temps de jeu doit être plus grande que 0.'
        isValid = false
    } else {
        playingTimeMinError.value = ""
    }

    if (editPlayingTimeMax.value.trim()) {
        if (+editPlayingTimeMax.value.trim() < +editNbPlayerMin.value || isNaN(+editPlayingTimeMax.value)) {
            playingTimeMaxError.value = 'La quantité maximale de temps de jeu ne peut pas être plus petite que la quantité minimale de temps de jeu et doit être un nombre.'
            isValid = false
        }
    } else {
        playingTimeMaxError.value = ""
    }

    return isValid
}


</script>

<template>
    <!--solution des :class{'is-invalid': Error} trouvé à l'aide de l'ia-->
    <h1>Modification d'un produit</h1>
    <form class="row g-12 needs-validation" id="modify-form" novalidate>
        <div class="row">
            <div class="col-12 col-lg-6">
                <label for="name" class="form-label">Entrez le nom du jeu</label>
                <div class="input-group has-validation">
                    <input type="text" class="form-control" placeholder="Final Girl: The Falconwood Files" name="name"
                        id="name" v-model="editName" :class="{ 'is-invalid': nameError }" required>
                    <div class="invalid-feedback">
                        {{ nameError }}
                    </div>
                </div>
            </div>
            <div class="col-12 col-lg-4">
                <label for="price" class="form-label">Entrez le prix de l'item</label>
                <input type="text" class="form-control" placeholder="29,99" name="price" id="price" v-model="editPrice"
                    :class="{ 'is-invalid': priceError }" required>
                <div class="invalid-feedback">
                    {{ priceError }}
                </div>
            </div>
            <div class="col-12 col-lg-2">
                <label for="bggScore" class="form-label">Note BGG</label>
                <input type="text" class="form-control" placeholder="8.2" name="bggScore" id="bggScore"
                    v-model="editBggScore" :class="{ 'is-invalid': bggScoreError }" required>
                <div class="invalid-feedback">
                    {{ bggScoreError }}
                </div>
            </div>
        </div>
        <div class="row">
            <div class="col-12 col-lg-12">
                <label for="textDescription" class="form-label">Description du jeu</label>
                <textarea class="form-control" name="description" rows="5" id="textDescription"
                    v-model="editDescription"></textarea>
            </div>
        </div>
        <div class="row">
            <div class="col-12 col-lg-6">
                <label for="qtyPlayer">Quantité de joueurs</label>
                <div class="d-flex flex-row justify-content-around">
                    <input type="text" class="form-control qty-values" name="minPlayer" placeholder="min"
                        v-model="editNbPlayerMin" :class="{ 'is-invalid': nbPlayerMinError }">
                    <div class="invalid-feedback">
                        {{ nbPlayerMinError }}
                    </div>
                    <h3> - </h3>
                    <input type="text" class="form-control qty-values" name="maxPlayer" placeholder="max"
                        v-model="editNbPlayerMax" :class="{ 'is-invalid': nbPlayerMaxError }">
                    <div class="invalid-feedback">
                        {{ nbPlayerMaxError }}
                    </div>
                </div>
            </div>
            <div class="col-12 col-lg-6">
                <label for="qtyPlayer">Quantité de temps</label>
                <div class="d-flex flex-row justify-content-around">
                    <input type="text" class="form-control qty-values" name="minTime" placeholder="min"
                        v-model="editPlayingTimeMin" :class="{ 'is-invalid': playingTimeMinError }">
                    <div class="invalid-feedback">
                        {{ playingTimeMinError }}
                    </div>
                    <h3> - </h3>
                    <input type="text" class="form-control qty-values" name="maxTime" placeholder="max"
                        v-model="editPlayingTimeMax" :class="{ 'is-invalid': playingTimeMaxError }">
                    <div class="invalid-feedback">
                        {{ playingTimeMaxError }}
                    </div>
                </div>
            </div>
        </div>
        <div class="row d-flex justify-content-around">
            <div class="col-12 col-lg-5">
                <label for="textStock" class="form-label">Quantité en stock</label>
                <input type="text" class="form-control" placeholder="404" name="quantity" id="textStock"
                    v-model="editQtyStock" :class="{ 'is-invalid': qtyStockError }" required>
                <div class="invalid-feedback">
                    {{ qtyStockError }}
                </div>
            </div>
        </div>
        <div class="col-12 col-lg-12">
            <button type="button" class="btn btn-secondary" @click.prevent="editBoardGame"
                :disabled="!editName.trim() || !editBggScore.trim() || !editNbPlayerMin.trim() || !editPlayingTimeMin.trim() || !editPrice.trim() || !editQtyStock.trim()">Modifier
                le jeu</button>
        </div>
    </form>
</template>