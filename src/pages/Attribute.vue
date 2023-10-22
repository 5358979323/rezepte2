<template>
    <div class="pb-2">
        <router-link to="/"><i class="bi bi-house"></i> Home</router-link>&nbsp;
        <span><i class="bi bi-card-list"></i> Gesammelte Attribute</span>&nbsp;
    </div>
    <div>
        <div class="accordion" id="attributeAccordion">
            <div v-for="(attribut, idx) in daten.attribute" :key="idx" class="accordion-item">
                <h2 class="accordion-header" :id="'heading-' + idx">
                    <button :class="'accordion-button' + (idx === 0 ? '' : ' collapsed')" type="button"
                        data-bs-toggle="collapse" :data-bs-target="'#collapse-' + idx"
                        :aria-expanded="idx === 0 ? 'true' : 'false'" :aria-controls="'collapse-' + idx">
                        {{ attribut._id }}
                    </button>
                </h2>
                <div :id="'collapse-' + idx" :class="'accordion-collapse collapse' + (idx === 0 ? ' show' : '')"
                    :aria-labelledby="'heading-' + idx" data-bs-parent="#attributeAccordion">
                    <div class="accordion-body">
                        <p>Gruppe: {{ attribut.group }}</p>
                        <p>Name: {{ attribut.name }}</p>
                        <p>Wert: {{ attribut.value }}</p>

                        <button class="btn btn-primary" type="button" @click="navigateTo('/attribut/' + attribut._id)">
                            <i class="bi bi-pen"></i> Bearbeiten
                        </button>
                        <button class="btn btn-warning" type="button" @click="remove(attribut._id)" disabled>
                            <i class="bi bi-trash3"></i> Entfernen
                        </button>
                    </div>
                </div>
            </div>
        </div>

    </div>
</template>
<script>
import { onMounted, reactive, computed, ref } from "vue";
import PouchDB from 'pouchdb-browser';
import { useRoute, useRouter } from "vue-router";

const db = new PouchDB("Attribute");
const validSelector = ref("");
export default {
    props: {},
    setup(props, ctx) {
        const $router = useRouter()
        const validSelector = (id) => {
            return id.replace(/[^a-zA-Z0-9_-]/g, "");
        };

        const attribut = reactive({
            _id: "",
            group: "",
            name: "",
            value: 0,
            _ref: "",
        })


        const daten = reactive({
            attribute: []
        });
        async function load() {
            try {
                const result = await db.allDocs({ include_docs: true });
                daten.attribute = result.rows.map(row => row.doc);
            } catch (error) {
                console.error("Fehler beim Abrufen der Attribute:", error);
            }
        }
        function remove(id) {
            const index = daten.attribute.findIndex(p => p._id === id);
            if (index !== -1) {
                daten.attribute.splice(index, 1);
            }
            console.log("Attribut entfernen:", id);
        }
        onMounted(async () => {
            await load()
        })

        return {
            daten,
            remove,
            navigateTo(route) {
                $router.push(route)
            },
            validSelector
        }
    },
};
</script>
  
<style></style>
  