<style>
.center-container {
    display: flex;
    justify-content: center;
}

.selected-button {
    background-color: red;
    color: white;
    font-weight: 800;
    text-transform: uppercase;
    border-radius: 5px;
}

.category-button {
    font-size: 20px;
    margin: 5px;
    padding: 5px;
}

.logo {
    width: 200px;
}

.navbar {
    width: 50%;
    margin: 20px;
}

.code-block {
    text-align: left;
    width: 50%;
    overflow: auto;
}

.code-block pre {
    white-space: pre-wrap;
    font-size: 18px;
}

.text-input {
    margin: 5px;
}
</style>

<template>
    <img class="logo" src="@/assets/logo.png">
    <div class="center-container">
        <div class="navbar">
            <button class="category-button" 
                v-for="(endpoint, label) in endpoints" 
                :key="label"
                :class="{ 'selected-button': selectedEndpoint === endpoint }"
                 @click="onSelectEndpoint(endpoint)">
                {{ label }}
            </button>
        </div>
    </div>
    <input placeholder="search..." ref="searchInput" type="text" class="text-input" @keyup.enter="onSubmitSearch">
    <input placeholder="get info..." ref="getInput" type="text" class="text-input" @keyup.enter="onSubmitGetInfo">
    <div class="code-block">
        <pre>{{ apiOutput }}</pre>
    </div>
</template>
  

<script>
export default {
    methods: {
        request(url) {
            fetch(url)
                .then(response => {
                    if (response.ok) { return response.json() } else { throw new Error('Failed to fetch data') }
                })
                .then(data => {
                    this.apiOutput = data
                    console.log(data)
                })
                .catch(error => {
                    console.error('Error:', error)
                })
        },

        onSelectEndpoint(endpoint) {
            this.selectedEndpoint = endpoint
        },

        onSubmitSearch() {
            const searchInput = this.$refs.searchInput.value;
            this.request(`https://www.dnd5eapi.co${this.selectedEndpoint}/?name=${searchInput}`)
        },

        onSubmitGetInfo() {
            const getInput = this.$refs.getInput.value;
            this.request(`https://www.dnd5eapi.co${this.selectedEndpoint}/${getInput}`)
        },
    },
    data() {
        return {
            apiOutput: '',
            selectedEndpoint: '',
            endpoints: {
                "ability-scores": "/api/ability-scores",
                "alignments": "/api/alignments",
                "backgrounds": "/api/backgrounds",
                "classes": "/api/classes",
                "conditions": "/api/conditions",
                "damage-types": "/api/damage-types",
                "equipment": "/api/equipment",
                "equipment-categories": "/api/equipment-categories",
                "feats": "/api/feats",
                "features": "/api/features",
                "languages": "/api/languages",
                "magic-items": "/api/magic-items",
                "magic-schools": "/api/magic-schools",
                "monsters": "/api/monsters",
                "proficiencies": "/api/proficiencies",
                "races": "/api/races",
                "rule-sections": "/api/rule-sections",
                "rules": "/api/rules",
                "skills": "/api/skills",
                "spells": "/api/spells",
                "subclasses": "/api/subclasses",
                "subraces": "/api/subraces",
                "traits": "/api/traits",
                "weapon-properties": "/api/weapon-properties"
            }
        }
    }
}
</script>