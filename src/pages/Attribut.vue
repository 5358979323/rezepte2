<template>
    <div class="pb-2">
        <router-link to="/"><i class="bi bi-house"></i> Home</router-link>&nbsp;
        <router-link to="/produkte"><i class="bi bi-card-list"></i> Attribute</router-link>&nbsp;
    </div>
    <div class="p-2">
        <div class="row gy-2">
            <div class="input-group">
                <input placeholder="Name" :disabled="newAttribut" v-model="name" class="form-control"
                    aria-describedby="nameSaveAddon">
                <button v-if="attribut._id !== name" class="btn btn-success" type="button" id="nameSaveAddon"
                    @click="save"><i class="bi bi-save"></i></button>
            </div>
        </div>
    </div>
    <div class="pb-2">
        <label for="attGroup">Gruppe</label>
        <p>
        <div class="radio-container">
            <div class="radio-option">
                <input type="radio" id="kategorie" name="group" value="Kategorie" v-model="attribut.group">
                <label for="kategorie"> Kategorie</label>
            </div>
            <div class="radio-option">
                <input type="radio" id="herkunft" name="group" value="Herkunft" v-model="attribut.group">
                <label for="herkunft"> Herkunft </label>
            </div>
            <div class="radio-option">
                <input type="radio" id="qualitaet" name="group" value="Qualitaet" v-model="attribut.group">
                <label for="qualitaet"> Qualität </label>
            </div>
            <div class="radio-option">
                <input type="radio" id="verpackung" name="group" value="Verpackung" v-model="attribut.group">
                <label for="verpackung"> Verpackung </label>
            </div>
            <div class="radio-option">
                <input type="radio" id="haltbarmachung" name="group" value="Halbarmachung" v-model="attribut.group">
                <label for="haltbarmachung"> Haltbarmachung </label>
            </div>
            <div class="radio-option">
                <input type="radio" id="verarbeitungsgrad" name="group" value="Verarbeitungsgrad" v-model="attribut.group">
                <label for="verarbeitungsgrad"> Verarbeitungsgrad </label>
            </div>
        </div>
        </p>
    </div>
    <div class="pb-2">
        <label for="attValue">Wert</label>
        <input placeholder="value" class="form-control" type="integer" v-model="value" id="attValue" required>
    </div>

    <div class="d-grid gap-2">
        <button class="btn btn-primary" type="submit" @click="save">Speichern</button>
    </div>
</template>
<script>
import { computed, onMounted, reactive, ref } from "vue";
import { useRoute, useRouter } from "vue-router";
import PouchDB from "pouchdb-browser";
const db = new PouchDB("Attribute");

export default {
    props: {},
    setup(props, { emit }) {
        const $route = useRoute();
        const $router = useRouter();
        const name = ref(null);
        const group = ref("");
        const value = ref("");


        const newAttribut = computed(() => {
            return !($route.params.id === "");
        });

        const attribut = reactive({
            _id: "",
            group: "",
            name: "",
            value: 0,
            _ref: "",
        });

        async function save() {
            try {
                await db.put(attribut);
                console.log("Änderungen gespeichert");
                await $router.push("/attribut/" + attribut._id);
            } catch (error) {
                console.error("Fehler beim Speichern:", error);
            }
        }

        async function load() {
            if ($route.params.id) {
                try {
                    const entry = await db.get($route.params.id);
                    for (const key of Object.keys(entry)) {
                        attribut[key] = entry[key];

                    }
                } catch (e) {
                    console.error(e);
                }
            }
        }

        onMounted(async () => {
            await load();
        });
        return {
            newAttribut,
            attribut,
            group,
            name,
            value,
            save,
        }
    }
};
</script>
  
<style>
.radio-container {
    display: flex;
    gap: 10px;
}

.radio-option input[type="radio"] {
    appearance: none;
    width: 15px;
    height: 15px;
    border: 2px solid #ccc;
    border-radius: 50%;
    margin-right: 8px;
    cursor: pointer;
}

.radio-option input[type="radio"]:checked {
    background-color: #007bff;
    border-color: #007bff;
}
</style>