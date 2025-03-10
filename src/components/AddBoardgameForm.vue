<script setup lang="ts">
import { ref, watch } from "vue"
import type { Boardgame } from "../scripts/types"

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

    addGame: (boardgame: Boardgame) => void
}>()

const nameError = ref<string>("")
const priceError = ref<string>("")
const qtyStockError = ref<string>("")
const nbPlayerMinError = ref<string>("")
const nbPlayerMaxError = ref<string>("")
const playingTimeMinError = ref<string>("")
const playingTimeMaxError = ref<string>("")
const bggScoreError = ref<string>("")

const addName = ref<string>("")
const addPrice = ref<string>("")
const addQtyStock = ref<string>("")
const addBggScore = ref<string>("")
const addNbPlayerMin = ref<string>("")
const addNbPlayerMax = ref<string>("")
const addPlayingTimeMin = ref<string>("")
const addPlayingTimeMax = ref<string>("")
const addDescription = ref<string>()

watch(() => props.name, (val) => {
    addName.value = val
})

watch(() => props.price, (val) => {
    addPrice.value = val
})

watch(() => props.qtyStock, (val) => {
    addQtyStock.value = val
})

watch(() => props.bggScore, (val) => {
    addBggScore.value = val
})

watch(() => props.nbPlayerMin, (val) => {
    addNbPlayerMin.value = val
})

watch(() => props.nbPlayerMax, (val) => {
    addNbPlayerMax.value = val
})

watch(() => props.playingTimeMin, (val) => {
    addPlayingTimeMin.value = val
})

watch(() => props.playingTimeMax, (val) => {
    addPlayingTimeMax.value = val
})

function addBoardGame() {
    let addNbPlayer: string = ""
    let addPlayingTime: string = ""

    if (addNbPlayerMax.value) {
        addNbPlayer = addNbPlayerMin.value + "-" + addNbPlayerMax.value
    } else {
        addNbPlayer = addNbPlayerMin.value
    }

    if (addPlayingTimeMax.value) {
        addPlayingTime = addPlayingTimeMin.value + "-" + addPlayingTimeMax.value
    } else {
        addPlayingTime = addPlayingTimeMin.value
    }

    if (validate()) {
        let newBoardgame: Boardgame = {
            id: Date.now(),
            name: addName.value,
            price: +addPrice.value,
            description: addDescription.value,
            qtyStock: +addQtyStock.value,
            bggScore: +addBggScore.value,
            nbPlayer: addNbPlayer,
            playingTime: addPlayingTime
        }

        props.addGame(newBoardgame)

        addBggScore.value = ""
        addDescription.value = ""
        addName.value = ""
        addNbPlayerMax.value = ""
        addNbPlayerMin.value = ""
        addPlayingTimeMax.value = ""
        addPlayingTimeMin.value = ""
        addPrice.value = ""
        addQtyStock.value = ""
    }
}

const validate = () => {
    let isValid = true

    if (!addName.value.trim()) {
        nameError.value = 'Le nom du jeu est requis.'
        isValid = false
    } else {
        nameError.value = ""
    }

    if (!addBggScore.value.trim() || isNaN(+addBggScore.value) || 0 > +addBggScore.value || +addBggScore.value > 10) {
        bggScoreError.value = 'Le score du jeu doit être un nombre entre 0 et 10.'
        isValid = false
    } else {
        bggScoreError.value = ""
    }

    if (!addPrice.value.trim() || isNaN(+addPrice.value) || 0 > +addPrice.value) {
        priceError.value = 'Le prix du jeu doit être un nombre avec une valeur minimale de 0.'
        isValid = false
    } else {
        priceError.value = ""
    }

    if (!addQtyStock.value.trim() || isNaN(+addQtyStock.value) || 0 > +addQtyStock.value) {
        qtyStockError.value = 'La quantité du jeu en stock doit être un nombre avec une valeur minimale de 0.'
        isValid = false
    } else {
        qtyStockError.value = ""
    }

    if (!addNbPlayerMin.value.trim() || isNaN(+addNbPlayerMin.value)) {
        nbPlayerMinError.value = 'La quantité minimale de joueurs est requise.'
        isValid = false
    } else if (+addNbPlayerMin.value.trim() < 1) {
        nbPlayerMinError.value = 'La quantité minimale de joueurs doit être plus grande que 1.'
        isValid = false
    } else {
        nbPlayerMinError.value = ""
    }

    if (addNbPlayerMax.value.trim()) {
        if (+addNbPlayerMax.value.trim() < +addNbPlayerMin.value || isNaN(+addNbPlayerMax.value)) {
            nbPlayerMaxError.value = 'La quantité maximale de joueurs ne peut pas être plus petite que la quantité minimale de joueurs et doit être un nombre.'
            isValid = false
        }
    } else {
        nbPlayerMaxError.value = ""
    }

    if (!addPlayingTimeMin.value.trim() || isNaN(+addPlayingTimeMin.value)) {
        playingTimeMinError.value = 'La quantité minimale de temps de jeu est requise.'
        isValid = false
    } else if (+addPlayingTimeMin.value.trim() < 0 && addPlayingTimeMax.value.trim()) {
        playingTimeMinError.value = 'La quantité minimale de temps de jeu doit être plus grande que 0.'
        isValid = false
    } else {
        playingTimeMinError.value = ""
    }

    if (addPlayingTimeMax.value.trim()) {
        if (+addPlayingTimeMax.value.trim() < +addNbPlayerMin.value || isNaN(+addPlayingTimeMax.value)) {
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
    <h1>Ajout d'un produit</h1>
    <form class="row g-12 needs-validation" id="addBoardgameForm" novalidate>
        <div class="row">
            <div class="col-12 col-lg-6">
                <label for="name" class="form-label">Entrez le nom du jeu</label>
                <div class="input-group has-validation">
                    <input type="text" class="form-control" :class="{ 'is-invalid': nameError }"
                        placeholder="Final Girl: The Falconwood Files" name="name" id="name" v-model="addName" required>
                    <div class="invalid-feedback">
                        {{ nameError }}
                    </div>
                </div>
            </div>
            <div class="col-12 col-lg-4">
                <label for="price" class="form-label">Entrez le prix de l'item</label>
                <input type="text" class="form-control" placeholder="29,99" name="price" id="price" v-model="addPrice"
                    :class="{ 'is-invalid': priceError }" required>
                <div class="invalid-feedback">
                    {{ priceError }}
                </div>
            </div>
            <div class="col-12 col-lg-2">
                <label for="bggScore" class="form-label">Note BGG</label>
                <input type="text" class="form-control" placeholder="8.2" name="bggScore" id="bggScore"
                    v-model="addBggScore" :class="{ 'is-invalid': bggScoreError }" required>
                <div class="invalid-feedback">
                    {{ bggScoreError }}
                </div>
            </div>
        </div>
        <div class="row">
            <div class="col-12 col-lg-12">
                <label for="textDescription" class="form-label">Description du jeu</label>
                <textarea class="form-control" name="description" rows="5" id="textDescription"
                    v-model="addDescription"></textarea>

            </div>
        </div>
        <div class="row">
            <div class="col-12 col-lg-6">
                <label for="qtyPlayer">Quantité de joueurs</label>
                <div class="d-flex flex-row justify-content-around">
                    <input type="text" class="form-control qty-values" name="minPlayer" placeholder="min"
                        v-model="addNbPlayerMin" :class="{ 'is-invalid': nbPlayerMinError }" required>
                    <div class="invalid-feedback">
                        {{ nbPlayerMinError }}
                    </div>
                    <h3> - </h3>
                    <input type="text" class="form-control qty-values" name="maxPlayer" placeholder="max"
                        v-model="addNbPlayerMax" :class="{ 'is-invalid': nbPlayerMaxError }">
                    <div class="invalid-feedback">
                        {{ nbPlayerMaxError }}
                    </div>
                </div>
            </div>
            <div class="col-12 col-lg-6">
                <label for="qtyPlayer">Quantité de temps</label>
                <div class="d-flex flex-row justify-content-around">
                    <input type="text" class="form-control qty-values" name="minTime" placeholder="min"
                        v-model="addPlayingTimeMin" :class="{ 'is-invalid': playingTimeMinError }" required>
                    <div class="invalid-feedback">
                        {{ playingTimeMinError }}
                    </div>
                    <h3> - </h3>
                    <input type="text" class="form-control qty-values" name="maxTime" placeholder="max"
                        v-model="addPlayingTimeMax" :class="{ 'is-invalid': playingTimeMaxError }">
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
                    v-model="addQtyStock" :class="{ 'is-invalid': qtyStockError }" required>
                <div class="invalid-feedback">
                    {{ qtyStockError }}
                </div>
            </div>
        </div>
        <div class="col-12 col-lg-12">
            <button class="btn btn-secondary" @click.prevent="addBoardGame"
                :disabled="!addName.trim() || !addBggScore.trim() || !addNbPlayerMin.trim() || !addPlayingTimeMin.trim() || !addPrice.trim() || !addQtyStock.trim()">Ajouter
                le
                jeu</button>
        </div>
    </form>
</template>