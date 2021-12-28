<template>
    <div style="display: inline-block">
        <button class="add-buttton" 
            v-on:click="$emit('showModal')">
            Добавить
        </button>
        <table  style="border: 1px solid" class="table">
            <td style="width: 150px; border: 1px solid">Имя</td>
            <td style="width: 250px; border: 1px solid">Телефон</td> 
            <tbody v-for="user in treeUsers" :key='user.id'>
                <tr @click="user.isOpenWorkers = !user.isOpenWorkers">
                    <td>
                        <div style="border-top: 1px solid; border-right: 1px solid">
                            <div class='name-item' v-if="user.workers.length > 0">
                                <span v-if="user.isOpenWorkers">-</span>
                                <span v-else>+</span>
                            </div>
                            <div class="name-item">
                                {{user.name}}
                            </div>
                        </div>
                    </td> 
                    <td>
                        <div style="border-top: 1px solid" >
                            {{user.phone}}
                        </div>
                    </td> 
                </tr>
                <tr v-if="user.workers.length > 0 && user.isOpenWorkers">
                    <td colspan="2">
                        <SubTable :users='user.workers' :level='1' ></SubTable>
                     </td>
                </tr>
            </tbody>
        </table>
    </div>
</template>

<script>
import SubTable from './SubTable'

export default  {
    name: 'UsersTable',
    components: {
        SubTable
    },
    props: ['treeUsers'],
    methods: {

    }
}
</script>

<style>
    table, caption, tbody, tfoot, thead, tr, th, td {
        margin: 0;
        padding: 0;
        border: none;
        outline: 0;
        font-size: 100%;
        vertical-align: baseline;
        background: transparent;
        border-spacing: 0;
        border-collapse: separate;
    }

    .add-buttton {
        background: #bababa;
        margin: 3px 0px 3px 300px;
        width: 100px;
        height: 30px;
        border-radius: 50rem;
    }

   .name-item {
        display: inline-block;
    }
</style>