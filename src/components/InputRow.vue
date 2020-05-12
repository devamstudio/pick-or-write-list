<template>
    <div class="input-row">
        <input 
            type="text" 
            name="link" 
            placeholder="URL" 
            v-model="link"
            @input="checkOnInput"
            autocomplete="off"
            v-if="id == -1">
        <span
            v-else
            @click="unsetLink">{{ link }}</span>
        <input 
            type="text" 
            name="name" 
            placeholder="Название" 
            v-model="name"
            @input="checkOnInput">
        <div>
            <div
                v-for="select in filteredVariants"
                :key="select.id"
                @click="setFromSelectable(select)">
                {{ select.name }}
            </div>
        </div>
    </div>
</template>

<script>
export default {
    data(){
        return {
            showHints: true,
            name: '',
            link: '',
            type: 'custom-link',
            id: -1,
            existing_links: [
                {
                    id: 0,
                    name: 'Цифровая печать',
                    model: 'post',
                    slug: '/cifrovaya-pechati/'
                },
                {
                    id: 1,
                    name: 'Листовая полиграфия',
                    model: 'post',
                    slug: '/listovaya-poly/'
                }
            ]
        }
    },
    computed: {
        filteredVariants() {
            let self = this;
            if(this.showHints){
                return this.link.length == 0 ? [] : this.existing_links.filter(function(el){
                    return el.name.toLowerCase().includes(self.link.toLowerCase()) || el.slug.toLowerCase().includes(self.link.toLowerCase())
                })
            } else {
                return [];
            }
        }
    },
    methods: {
        setFromSelectable(source){
            this.name = source.name;
            this.link = source.slug;
            this.id = source.id;
            this.checkOnInput();
        },
        unsetLink(){
            this.link = '';
            this.id = -1;
            this.showHints = true;
            this.showInfo();
        },
        checkOnInput(){
            if(this.filteredVariants.length != 0) {
                console.log(this.id != this.filteredVariants[0].id);
                this.showHints = this.id != this.filteredVariants[0].id;
            } else {
                this.showHints = false;
            }
        },
        showInfo(){
            console.log(this.id);
            console.log(this.showHints);
        }
    },
    watch: {
        link(){
            if(this.link == 0) this.showHints = true;
        },
        name(){
            if(this.name == 0) this.showHints = true;
        }
    }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
</style>
