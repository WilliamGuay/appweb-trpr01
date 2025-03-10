<script setup lang="ts">
import "bootstrap/dist/css/bootstrap.min.css"
import "bootstrap/dist/js/bootstrap.bundle.min.js"
import "bootstrap"

import BoardgameList from "./components/BoardgameList.vue";
import NavBar from "./components/Header.vue";
</script>

<template>
    <div id="page">
        <NavBar />
        <BoardgameList />
    </div>
</template>

<style>
body {
    background: #B6D369;
}

#page {
    background-color: #D0E562;
    width: 100%;
    margin: 10px;
}
</style>
