<script setup lang="ts">

</script>

<template>
    <form class="row g-12 needs-validation" novalidate>
        <div class="row">
            <div class="col-12 col-lg-6">
                <label for="name" class="form-label">Entrez le nom du jeu</label>
                <div class="input-group has-validation">
                    <input type="text" class="form-control" placeholder="Final Girl: The Falconwood Files" name="name"
                        id="name" required>
                    <div class="invalid-feedback">
                        Veuillez entrer un nom valide.
                    </div>
                </div>
            </div>
            <div class="col-12 col-lg-6">
                <label for="price" class="form-label">Entrez le prix de l'item</label>
                <input type="text" class="form-control" placeholder="29,99" name="price" id="price" required>
            </div>
        </div>
        <div class="row">
            <div class="col-12 col-lg-12">
                <label for="textDescription" class="form-label">Description du jeu</label>
                <textarea class="form-control" name="description" rows="5" id="textDescription" required></textarea>
            </div>
        </div>
        <div class="row">
            <div class="col-12 col-lg-12">
                <label for="textStock" class="form-label">Quantité en stock</label>
                <input type="textStock" class="form-control" placeholder="404" name="quantity" id="textStock" required>
            </div>
        </div>
        <div class="col-12 col-lg-12">
            <button class="btn btn-primary" type="submit">Envoyer votre demande</button>
        </div>
    </form>
</template>

<style></style>