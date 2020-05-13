<template>
    <div id="app">
        <form action="#" method="get">
            <PickOrWriteList
                class="list"
                :field-data="fieldData"
                :select-from-data="fakeData"
                field-name="related_tags"/>
                <button>Сохранить</button>
        </form>
    </div>
</template>

<script>
import PickOrWriteList from './components/PickOrWriteList.vue'

export default {
    name: 'App',
    data(){
        return {
            fieldData: [
                {
                    id: 0,
                    name: '',
                    type: 'custom',
                    relation: -1,
                    link: ''
                },
                {
                    id: 1,
                    name: 'Тест',
                    type: 'product',
                    relation: 0,
                    link: '/url-test-2/'
                }
            ],
            fakeData: [
                {
                    id: 0,
                    name: 'Проверка',
                    type: 'post',
                    link: '/url-test-1/'
                },
                {
                    id: 1,
                    name: 'Тест',
                    type: 'post',
                    link: '/url-test-2/'
                }
            ],
        }
    },
    components: {
        PickOrWriteList
    }
}
</script>

<style lang="sass">
body
    margin: 0

*
    box-sizing: border-box

#app
    height: 100vh
    display: flex
    flex-direction: column
    align-items: center
    justify-content: center

.list
    max-width: 500px
</style>
