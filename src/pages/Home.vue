<template>
  <div>
    <h1><i class="bi bi-house"></i> Home</h1>
    <div class="container">
      <div class="d-grid gap-2">
        <button class="btn btn-primary" type="button" @click="navigateTo('/rezepte')"><i class="bi bi-card-list"></i> Rezepte</button>
        <button class="btn btn-primary" type="button" @click="navigateTo('/rezept')"><i class="bi bi-file-earmark-plus-fill"></i> Neues Rezept</button>
        <button class="btn btn-primary" type="button" @click="navigateTo('/produkte')"><i class="bi bi-card-list"></i> Produkte</button>
        <!--<button class="btn btn-primary" type="button" @click="navigateTo('/produkt')"><i class="bi bi-cart-plus"></i> Neues Produkt</button>-->
        <button class="btn btn-primary" type="button" @click="openNewProduktDialog"><i class="bi bi-cart-plus"></i> Neues Produkt</button>
        <button class="btn btn-primary" type="button" @click="navigateTo('/materialien')"><i class="bi bi-card-list"></i> Materialien</button>
        <button class="btn btn-primary" type="button" @click="navigateTo('/material')"><i class="bi bi-card-list"></i> Neues Material</button>
        <button class="btn btn-primary" type="button" @click="navigateTo('/attribute')"><i class="bi bi-card-list"></i> Attribute</button>
        <button class="btn btn-primary" type="button" @click="openNewAttributDialog"><i class="bi bi-cart-plus"></i> Neues Attribut</button>

      </div>
    </div>
    <NewProduktDialog :is-open="newProduktDialogOpen" @close-dialog="closeNewProduktDialog"/>
    <NewAttributDialog :is-open="newAttributDialogOpen" @close-dialog="closeNewAttributDialog"/>
    <!-- Weitere Komponenten und Inhalt hier -->
  </div>
</template>

<script>

import NewProduktDialog from "@/components/NewProduktDialog.vue";
import NewAttributDialog from "@/components/NewAttributDialog.vue";
import {ref} from "vue";
import {useRouter} from "vue-router";

export default {
  components: {NewProduktDialog, NewAttributDialog},
  setup(props, { emit }) {
    const $router = useRouter()
    const newProduktDialogOpen = ref(false); // Zustand zur Steuerung des Dialogs
    const openNewProduktDialog = () => {
      newProduktDialogOpen.value = true; // Öffnet den Dialog
    };
    const closeNewProduktDialog = async () => {
      newProduktDialogOpen.value = false; // Schließt den Dialog
      //await loadProducts()
      //emit('refresh')
    };
    const newAttributDialogOpen = ref(false); 
    const openNewAttributDialog = () => {
      newAttributDialogOpen.value = true; 
    };
    const closeNewAttributDialog = async () => {
      newAttributDialogOpen.value = false; 
    };
    return {
      newProduktDialogOpen,
      openNewProduktDialog,
      closeNewProduktDialog,
      newAttributDialogOpen,
      openNewAttributDialog,
      closeNewAttributDialog,
      navigateTo(route) {
        $router.push(route)
      }
    }
  }
};
</script>

<style>
</style>