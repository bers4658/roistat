<template>
    <div class="modal-mask">
        <div class="modal-wrapper">
            <div class="modal-container">
                <h3>Добавить пользователя</h3>
                <div v-on:click="$emit('closeModal')" 
                    class="close-icon">   
                    <img style="width: 100%" src="@/assets/close.png">
                </div>   
                <div class="form-item">
                    <input v-model="newUser.name" placeholder="Имя">
                </div>
                <div class="form-item">        
                    <input  v-model="newUser.phone" type="number" placeholder="Телефон">
                </div>
                <div class="form-item">
                        <option disabled>Выберите начальника</option>
        
                    <select v-model="boss"> 
                        <option  v-for="boss in bosses" v-bind:value="boss" :key='boss.id'>
                            {{boss.name}}    
                        </option>
                    </select>
                </div>
                <div class="form-item">
                    <button v-on:click="saveForm()"
                        class="saveButton"
                        style="margin: 5px">
                        Сохранить
                    </button>
                </div>
            </div>
        </div> 
    </div>
</template>
<script>

export default  {
    name: 'Profile',
    components: {
        
    },
    props: ['bosses'],
    data() {
        return {
            newUser: {
                id: null,
                name: "",
                phone: "",
                isOpenWorkers: false,
                workers: [],
            },
            boss: null
        }
    },
    methods: {
        saveForm() { 
            if(!this.validation()){
                return
            }
            this.newUser.id = this.getNewId()
            let saveData = {
                user: this.newUser,
                boss: this.boss
            }
            this.$emit('saveUser', saveData)
            this.$emit('closeModal')
        },

        validation() {
            if(this.newUser.name.length > 10) {
                alert('Слишком длинное имя!')
                return false
            }
            if(this.newUser.phone.length > 10) {
                alert('Слишком длинный телефон!')
                return false
            }
            if(!this.newUser.name){
                alert('Введите имя!')
                return false
            }
            if(!this.newUser.phone){
                alert('Введите телефон!')
                return false
            }
            return true
        },

        getNewId(){
            let currentId = localStorage.getItem('currentId')
            if (currentId == null) {
                currentId = 1
                localStorage.setItem('currentId', currentId)
                return currentId;
            }
            currentId++
            localStorage.setItem('currentId', currentId)
            return currentId;
        }  
    }
}
</script>

<style scoped>
.modal-mask {
  position: fixed;
  z-index: 9998;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background-color: rgba(0, 0, 0, 0.5);
  display: table;
  transition: opacity 0.3s ease;
}

.modal-wrapper {
  display: table-cell;
  vertical-align: middle;
}

.modal-container {
  position: relative;
  width: 300px;
  margin: 0px auto;
  padding: 20px 30px;
  background-color: #fff;
  border-radius: 2px;
  box-shadow: 0 2px 8px rgba(0, 0, 0, 0.33);
  transition: all 0.3s ease;
  font-family: Helvetica, Arial, sans-serif;
  text-align: center;
}

.close-icon {
   width:10px;
   position: absolute;
   right: 10px;
   top: 10px;
   cursor: pointer;
}

.form-item{
    margin: 10px;
}

.saveButton{
    background: #bababa;
    margin: 0px 0px 0px 0px;
    width: 100px;
    height: 30px;
    border-radius: 50rem;
    
}
</style>