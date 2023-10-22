<template>
    <div v-if="isOpen" class="dialog-overlay">
        <div class="dialog-content">

            <button class="close-button" @click="$emit('closeDialog')">X</button>
            <h4>Attribut hinzufügen</h4>


            <div class="pb-2">
                <div>
                    <label for="attribute">Attributgruppe:</label>
                    <select id="attribute" v-model="attribut.group">
                        <option value="">hier auswählen:</option>
                        <option v-for="group in attributeGroups" :value="group">{{ group }}</option>
                    </select>
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
                            <input type="radio" id="haltbarmachung" name="group" value="Halbarmachung"
                                v-model="attribut.group">
                            <label for="haltbarmachung"> Haltbarmachung </label>
                        </div>
                        <div class="radio-option">
                            <input type="radio" id="verarbeitungsgrad" name="group" value="Verarbeitungsgrad"
                                v-model="attribut.group">
                            <label for="verarbeitungsgrad"> Verarbeitungsgrad </label>
                        </div>
                    </div>
                    </p>
                </div>
                <div class="pb-2">
                    <label for="attName">Name</label>
                    <input placeholder="name" class="form-control" type="text" v-model="name" id="attName" required>
                </div>
                <div class="pb-2">
                    <label for="attValue">Wert</label>
                    <input placeholder="value" class="form-control" type="integer" v-model="value" id="attValue" required>
                </div>
            </div>
            <div class="d-grid gap-2">
                <button class="btn btn-primary" type="submit" @click="addAttribut">Hinzufügen</button>
            </div>
            <div class="d-grid gap-2">
                <button class="btn btn-primary" @click="$emit('closeDialog')">Abbrechen</button>
            </div>
        </div>
    </div>
</template>
<script>
import { defineComponent, onBeforeMount, onMounted, reactive, ref, watch } from "vue";
import PouchDB from "pouchdb-browser";
import { useRoute } from "vue-router";

export default defineComponent({
    emits: ['closeDialog'],
    props: {
        isOpen: {
            type: Boolean,
            required: true
        }
    },
    setup(props, { emit }) {
        const db = new PouchDB("Attribute")
        const name = ref(null)
        const attribut = reactive({
            _id: "",
            group: "",
            name: "",
            value: 0,
            _ref: "",
        });
        watch(
            () => props.isOpen,
            (newVal, oldValue) => {
                if (newVal) {
                    attribut._id = ""
                }
            }
        );
        onBeforeMount(() => {
            attribut._id = ""
        })
        return {
            name,
            attribut,
            attributeGroups: ['Kategorie', 'Herkunft', 'Qualitaet', 'Verpackung', 'Haltbarmachung', 'Verarbeitungsgrad'],
        
            async addAttribut() {
                try {
                    attribut._id = name.value
                    await db.put(attribut)
                } catch (error) {
                    console.error('Error saving attribut:', error)
                }
                //
                emit('closeDialog')
            }
        }
    }
});
</script>

<style scoped>
.dialog-overlay {
    position: fixed;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    background-color: rgba(0, 0, 0, 0.5);
    display: flex;
    justify-content: center;
    align-items: center;
    z-index: 9999;
}

.dialog-content {
    background-color: white;
    padding: 20px;
    border-radius: 4px;
    z-index: 9999;
    width: 60%;
    max-height: 80%;
    overflow-y: auto;
}

.close-button {
    position: absolute;
    top: 50px;
    right: 230px;
    font-size: 1.2rem;
    cursor: pointer;
    background-color: red;
    color: white;
    border: none;
    padding: 8px;
    border-radius: 50%;
}

.radio-container {
    display: flex;
    gap: 10px;
}

.radio-option {
    display: flex;
    align-items: center;

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


