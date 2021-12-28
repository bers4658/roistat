<template>
    <table  class="table">
        <tbody v-for="user in users" :key='user.id'>
                <tr @click="user.isOpenWorkers = !user.isOpenWorkers">
                    <td style="width: 150px">
                      <div  style="border-top: 1px solid; border-left: 0px solid; border-right: 1px solid; " v-bind:style="'padding-left:' + tdShiftPixel + 'px'">
                            <div class='name-item' v-if="user.workers.length > 0"> 
                                <span v-if="user.isOpenWorkers">-</span>
                                <span v-else>+</span>
                            </div>
                            <div class="name-item cut-text">
                                {{user.name}}
                            </div>
                      </div> 
                    </td> 
                    <td style="width: 250px">
                        <div style="border-top: 1px solid; border-right: 0px solid ">
                            {{user.phone}}
                        </div>
                    </td> 
                </tr>
                <tr v-if="user.workers.length > 0 && user.isOpenWorkers" >
                    <td colspan="2">
                        <SubTable :users='user.workers' :level='level + 1'></SubTable>
                     </td>
                </tr>
            </tbody>
    </table>
</template>
<script>

export default {
    name: 'SubTable',
    props: ['users', 'level'],
    mounted() {
        this.tdShiftPixel = this.level * 20
    },
    data() {
        return {
            tdShiftPixel: 0 
        }
    }
}
</script>

<style scoped>

</style>
