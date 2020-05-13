<template>
    <div 
        class="pick-or-write-list"
        v-if="fieldData">
        <div
            class="pick-or-write-list__rows">
            <div 
                class="pick-or-write-list__row-item row"
                v-for="row in fieldData"
                :key="row.id">
                <InputRow
                    class="row__component"
                    :field-data="row"
                    :selectFromData="selectFromData"
                    @saveRow="updateRow"/>
                <button
                    class="row__remove"
                    @click="removeRow(row.id)">&times;</button>
            </div>
        </div>
        <button
            class="pick-or-write-list__create-new-row"
            @click="createRow">+</button>
        
        <hr>
        <div>
            <h4>Данные для вывода</h4>
            {{ fieldData }}
        </div>
        <hr>
        <div>
            <h4>Входящие возможные связи</h4>
            {{ selectFromData }}
        </div>

        <!-- Field -->
        <input 
            type="hidden"
            :name="fieldName"
            :value="JSON.stringify(fieldData)"/>
    </div>
</template>

<script>
import InputRow from './InputRow.vue'

export default {
    name: 'PickOrWriteList',
    props: {
        fieldData: [Array, Boolean],
        fieldName: String,
        selectFromData: [Array, Boolean]
    },
    methods: {
        createRow() {
            let row = {
                id: this.fieldData.length + 1,
                name: '',
                type: 'custom-field',
                relation: -1,
                link: ''
            };
            this.fieldData.push(row);
        },
        removeRow(id) {
            this.fieldData = this.fieldData.filter(el => el.id != id);
        },
        updateRow(data) {
            this.fieldData.map(function(el){
                if(el.id == data.id) {
                    el.name = data.name;
                    el.link = data.link;
                    el.relation = data.relation;
                    el.type = data.type;
                } else return false
            })
        }
    },
    components: {
        InputRow
    }
}
</script>

<style lang="sass" scoped>
.pick-or-write-list
    // Elems
    .pick-or-write-list__rows
    .pick-or-write-list__row-item
        margin: 15px 0
    .pick-or-write-list__create-new-row
        width: 100%

.row
    display: flex
    align-items: flex-start
    // Elems
    .row__component
        flex-grow: 1
</style>