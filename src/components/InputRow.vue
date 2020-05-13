<template>
    <div class="input-row">
        <input 
            class="input-row__field"
            type="text"
            name="link"
            placeholder="URL" 
            v-model="link"
            @input="checkOnInput"
            autocomplete="off"
            v-if="relation == -1">
        <div
            class="input-row__relation relation-box"
            v-else>
            <a
                :href="link"
                target="_blank"
                class="relation-box__value">{{ link }}</a>
            <span
                class="relation-box__remove-button"
                @click="unsetLink">&times;</span>
        </div>
        <input 
            class="input-row__field"
            type="text" 
            name="name" 
            placeholder="Название" 
            v-model="name"
            autocomplete="off"
            @input="checkOnInput">
        <div
            class="input-row__data-list"
            v-if="filteredVariants.length != 0">
            <div
                class="data-list-item"
                v-for="select in filteredVariants"
                :key="select.id"
                @click="setFromSelectable(select)">
                <span class="data-list-item__value">{{ select.name }}</span>
                <span class="data-list-item__type">{{ select.type }}</span>
            </div>
        </div>
    </div>
</template>

<script>
export default {
    props: {
        selectFromData: Array,
        fieldData: Object,
    },
    data(){
        return {
            showHints: this.relation == -1,
            name: this.fieldData.name,
            link: this.fieldData.link,
            type: this.fieldData.type,
            relation: this.fieldData.relation,
        }
    },
    created(){
        this.checkOnInput();
    },
    computed: {
        filteredVariants() {
            let self = this;
            if(this.showHints){
                return this.name.length == 0 && this.link.length == 0 ? [] : this.selectFromData.filter(function(el){
                    let concatSearch = el.name.toLowerCase() + el.link.toLowerCase();
                    return concatSearch.includes(self.link.toLowerCase()) || concatSearch.includes(self.link.toLowerCase())
                })
            } else {
                return [];
            }
        },
        outputFieldData(){
            return {
                id: this.fieldData.id,
                name: this.name,
                link: this.link,
                type: this.type,
                relation: this.relation
            }
        }
    },
    methods: {
        setFromSelectable(source){
            this.name = source.name;
            this.link = source.link;
            this.relation = source.id;
            this.type = source.type;
            this.checkOnInput();
        },
        unsetLink(){
            this.link = '';
            this.relation = -1;
            this.type = 'custom'
            this.showHints = true;
        },
        checkOnInput(){
            if(this.filteredVariants.length != 0) {
                this.showHints = this.relation != this.filteredVariants[0].id;
            } else {
                this.showHints = false;
            }
        },
        saveRow(){
            this.$emit('saveRow', this.outputFieldData)
        }
    },
    watch: {
        outputFieldData(){
            if(this.link == 0 || this.name == 0) this.showHints = true;
            this.saveRow();
        }
    }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style lang="sass" scoped>
.input-row
    display: grid
    grid:
        template-columns: 1fr 1fr
    // Elems
    .input-row__field, 
    .input-row__relation
        flex:
            grow: 1
        height: 40px
    .input-row__data-list
        grid-column: 1 / -1
        margin:
            bottom: 15px
    .input-row__save-button

.data-list-item
    display: flex
    flex-direction: column
    padding: .25em .5em
    background-color: lighten(#f2f2f2, .5)
    & + .data-list-item
        border-top: 1px solid rgba(black, .1)
    // Elems
    .data-list-item__title
    .data-list-item__type
        opacity: .8
        font-size: .8em
    // States
    &:hover
        cursor: pointer



.relation-box
    display: flex
    background-color: #f2f2f2
    align-items: center
    // Elems
    .relation-box__value
        padding: .25em .5em
        flex-grow: 1
        white-space: nowrap
        overflow: hidden
        text-overflow: ellipsis
    .relation-box__remove-button
        display: inline-flex 
        flex:
            shrink: 0
        height: 40px
        width: 40px
        align-items: center
        justify-content: center    
        &:hover
            background-color: #dd0000
            color: white
</style>
