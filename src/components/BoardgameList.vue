<script setup lang="ts">
import { computed, ref } from 'vue';
import type { Boardgame } from '../scripts/types';
import { mkConfig, generateCsv, download } from 'export-to-csv';

import BoardgameItem from './BoardgameItem.vue';
import Searchbar from './Searchbar.vue';
import AddBoardgameForm from './AddBoardgameForm.vue';
import ModifyBoardGame from './ModifyBoardGameForm.vue';

const boardgames = ref<Boardgame[]>([
    {
        id: 1,
        name: "Final Girl: Cauchemar sur Maple Lane",
        price: 56.99,
        description: "Ce nouveau Long Métrage de Final Girl introduit le Docteur Peur, le célèbre docteur des songes, prenant sa revanche sur tous les responsables de sa mort prématurée. Dans Cauchemar sur Maple Lane, incarnez Nancy ou Sheila, deux nouvelles Survivantes, et affrontez le Docteur sur son propre terrain, la terrible Chaufferie dans laquelle il piège ses victimes une fois celles-ci assoupies…",
        qtyStock: 25,
        bggScore: 8.2,
        nbPlayer: "1",
        playingTime: "20 - 60"
    },
    {
        id: 2,
        name: "Slay the Spire: Le jeu de plateau",
        price: 154.99,
        description: "Slay the Spire : Le jeu de plateau est une aventure coopérative de construction de deck. Créez un deck unique, rencontrez des créatures bizarres, découvrez des reliques d'un pouvoir immense et devenez enfin assez fort pour tuer la Spire !",
        qtyStock: 10,
        bggScore: 8.7,
        nbPlayer: "1 - 4",
        playingTime: "30 - 150"
    },
    {
        id: 3, // Jeu que je recommande à toute personnes essayant de trouver un jeu relax à jouer en groupe
        name: "7 Wonders",
        price: 69.99,
        description: "Dans 7 Wonders, prenez la tête de l'une des sept grandes cités du monde antique et laissez votre empreinte dans l'histoire des civilisations. Cette boîte est la 2ème édition de ce jeu devenu un classique.",
        qtyStock: 30,
        bggScore: 7.7,
        nbPlayer: "3 - 7",
        playingTime: "30"
    },
    {
        id: 4, // excellent jeu pour des personnes qui ne jouent pas beaucoup aux jeux de société, il est aussi magnifique
        name: "Canvas",
        price: 49.99,
        description: "Dans Canvas, vous êtes un peintre en compétition dans un prestigieux festival d’art. Au cours du jeu, vous collecterez des cartes d’art transparentes et les superposerez pour créer des peintures. Selon les cartes que vous choisissez et l’ordre dans lequel vous les placez, vous révélerez ou couvrirez les icônes qui déclenchent diverses conditions de notation. Le joueur qui a le plus de points gagne Best in Show!",
        qtyStock: 35,
        bggScore: 7.2,
        nbPlayer: "1 - 5",
        playingTime: "30"
    },
    {
        id: 5,
        name: "Secret Hitler",
        price: 74.99,
        description: "Secret Hitler est un jeu d'intrigue politique et de trahison qui se déroule dans l'Allemagne des années 1930. Chaque joueur est assigné au hasard et secrètement pour être un libéral ou un fasciste et un joueur est Secret Hitler. Les fascistes se coordonnent pour semer la méfiance et installer leur chef; les libéraux doivent trouver et arrêter Hitler.",
        qtyStock: 0,
        bggScore: 7.5,
        nbPlayer: "5 - 10",
        playingTime: "45"
    }
])

const addName = ref<string>("")
const addPrice = ref<string>("")
const addQtyStock = ref<string>("")
const addBggScore = ref<string>("")
const addNbPlayerMin = ref<string>("")
const addNbPlayerMax = ref<string>("")
const addPlayingTimeMin = ref<string>("")
const addPlayingTimeMax = ref<string>("")
const addDescription = ref<string>()

const editName = ref<string>("")
const editPrice = ref<string>("")
const editQtyStock = ref<string>("")
const editBggScore = ref<string>("")
const editNbPlayerMin = ref<string>("")
const editNbPlayerMax = ref<string>("")
const editPlayingTimeMin = ref<string>("")
const editPlayingTimeMax = ref<string>("")
const editDescription = ref<string>()

const selectedBoardGame = ref<Boardgame>()
const searchBarValue = ref<string>("")

const csvConfig = mkConfig({ useKeysAsHeaders: true })

function downloadCsv() {
    download(csvConfig)(generateCsv(csvConfig)(boardgames.value))
}

function deleteBoardGame(id: number) {
    boardgames.value = boardgames.value.filter((boardgame) => boardgame.id !== id) //viens de la vidéo solution de l'exercice 6 https://appweb.progwmj.ca/exercices/exercice-06
}

function startEditBoardGame(idToEdit: number) {
    boardgames.value.forEach((game: Boardgame) => {
        if (game.id == idToEdit) {
            selectedBoardGame.value = game
        }
    })

    editName.value = selectedBoardGame.value!.name
    editBggScore.value = selectedBoardGame.value!.bggScore.toString()
    editDescription.value = selectedBoardGame.value!.description

    if (selectedBoardGame.value!.nbPlayer.indexOf("-") >= 0) {
        editNbPlayerMin.value = selectedBoardGame.value!.nbPlayer.split("-")[0].trim()
        editNbPlayerMax.value = selectedBoardGame.value!.nbPlayer.split("-")[1].trim()
    } else {
        editNbPlayerMin.value = selectedBoardGame.value!.nbPlayer.trim()
    }

    if (selectedBoardGame.value!.playingTime.indexOf("-") >= 0) {
        editPlayingTimeMin.value = selectedBoardGame.value!.playingTime.split("-")[0].trim()
        editPlayingTimeMax.value = selectedBoardGame.value!.playingTime.split("-")[1].trim()
    } else {
        editPlayingTimeMin.value = selectedBoardGame.value!.playingTime.trim()
    }
    editPrice.value = selectedBoardGame.value!.price.toString()
    editQtyStock.value = selectedBoardGame.value!.qtyStock.toString()
}

function editBoardGame(boardgame: Boardgame) {
    selectedBoardGame.value!.name = boardgame.name
    selectedBoardGame.value!.bggScore = boardgame.bggScore
    selectedBoardGame.value!.nbPlayer = boardgame.nbPlayer
    selectedBoardGame.value!.playingTime = boardgame.playingTime
    selectedBoardGame.value!.price = boardgame.price
    selectedBoardGame.value!.qtyStock = boardgame.qtyStock

    if (editDescription.value != null) {
        if (editDescription.value.trim()) {
            selectedBoardGame.value!.description = editDescription.value
        }
    }
}

function addBoardGame(boardgame: Boardgame) {
    boardgames.value.push(boardgame)
}

function duplicateBoardGame(idToDuplicate: number) {
    boardgames.value.forEach((game: Boardgame) => {
        if (game.id == idToDuplicate) {
            selectedBoardGame.value = game
        }
    })

    addName.value = selectedBoardGame.value!.name
    addBggScore.value = String(selectedBoardGame.value!.bggScore)
    addDescription.value = selectedBoardGame.value!.description

    if (selectedBoardGame.value!.nbPlayer.indexOf("-") >= 0) {
        addNbPlayerMin.value = selectedBoardGame.value!.nbPlayer.split("-")[0].trim()
        addNbPlayerMax.value = selectedBoardGame.value!.nbPlayer.split("-")[1].trim()
    } else {
        addNbPlayerMin.value = selectedBoardGame.value!.nbPlayer.trim()
        addNbPlayerMax.value = ""
    }

    if (selectedBoardGame.value!.playingTime.indexOf("-") >= 0) {
        addPlayingTimeMin.value = selectedBoardGame.value!.playingTime.split("-")[0].trim()
        addPlayingTimeMax.value = selectedBoardGame.value!.playingTime.split("-")[1].trim()
    } else {
        addPlayingTimeMin.value = selectedBoardGame.value!.playingTime.trim()
        addPlayingTimeMax.value = ""
    }

    addPrice.value = String(selectedBoardGame.value!.price)
    addQtyStock.value = String(selectedBoardGame.value!.qtyStock)
}

function updateSearchBarValue(query: string) {
    searchBarValue.value = query
}

//méthode faite avec l'aide de l'intelligence artificielle
const searchFilteredBoardgameList = computed(() =>
    boardgames.value.filter(game =>
        game.name.toLowerCase().includes(searchBarValue.value.toLowerCase())
    )
)
</script>

<template>
    <div class="d-flex justify-content-around g-12">
        <button class="btn btn-info mb-3 col-lg-3" @click="downloadCsv">Exporter vers csv</button>
        <div class="col-12 col-lg-6">
            <Searchbar :search="updateSearchBarValue" />
        </div>
    </div>

    <table class="table table-striped" id="list">
        <thead>
            <th scope="col">Nom</th>
            <th scope="col">Score BGG</th>
            <th scope="col">Prix (CA$)</th>
            <th scope="col">Quantité en inventaire</th>
        </thead>
        <tbody>
            <BoardgameItem v-for="game of searchFilteredBoardgameList" :key="game.id" :boardgame="game"
                :onDelete="deleteBoardGame" :onEdit="startEditBoardGame" :onDuplicate="duplicateBoardGame" />
        </tbody>
    </table>

    <AddBoardgameForm :addGame="addBoardGame" :name="addName" :bggScore="addBggScore" :price="addPrice"
        :qtyStock="addQtyStock" :nbPlayerMin="addNbPlayerMin" :nbPlayerMax="addNbPlayerMax"
        :playingTimeMin="addPlayingTimeMin" :playingTimeMax="addPlayingTimeMax" :description="addDescription" />

    <hr>

    <ModifyBoardGame :name="editName" :bggScore="editBggScore" :price="editPrice" :qtyStock="editQtyStock"
        :nbPlayerMin="editNbPlayerMin" :nbPlayerMax="editNbPlayerMax" :playingTimeMin="editPlayingTimeMin"
        :playingTimeMax="editPlayingTimeMax" :description="editDescription" :edit-game="editBoardGame" />

</template>

<style>
#list {
    background-color: #93C48B;
}

.qty-values {
    width: 40%
}

#modify-form {
    margin-top: 10px;
    background: #001B2E;
}
</style>