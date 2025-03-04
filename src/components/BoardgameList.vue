<script setup lang="ts">
import GameItem from './BoardgameItem.vue';
import { ref, onMounted, onUnmounted } from 'vue';
import type { Boardgame } from '../scripts/types';
import BoardgameItem from './BoardgameItem.vue';

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
        nbPlayer: "2 - 7",
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
        id: 1,
        name: "Secret Hitler",
        price: 74.99,
        description: "Secret Hitler est un jeu d'intrigue politique et de trahison qui se déroule dans l'Allemagne des années 1930. Chaque joueur est assigné au hasard et secrètement pour être un libéral ou un fasciste et un joueur est Secret Hitler. Les fascistes se coordonnent pour semer la méfiance et installer leur chef; les libéraux doivent trouver et arrêter Hitler.",
        qtyStock: 0,
        bggScore: 7.5,
        nbPlayer: "5 - 10",
        playingTime: "45"
    }
])

</script>

<template>
    <table class="table table-striped">
        <thead>
            <th scope="col">Nom</th>
            <th scope="col">Score BGG</th>
            <th scope="col">Prix (CA$)</th>
            <th scope="col">Quantité en inventaire</th>
        </thead>
        <tbody>
            <BoardgameItem v-for="game of boardgames" :key="game.id" :name="game.name" :price="game.price"
                :description="game.description" :qty-stock="game.qtyStock" :bgg-score="game.bggScore"
                :nb-player="game.nbPlayer" :playing-time="game.playingTime" />
        </tbody>
    </table>
</template>

<style></style>