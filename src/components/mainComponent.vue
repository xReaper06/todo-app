<script setup>
import { computed, ref } from 'vue';
import ListComponent from './listComponents.vue';

const newList = ref('');
const currentdate = new Date();
const searchQuery = ref('');

const todoList = ref([]);

const handleNewList = () => {
    const dateToday = currentdate.getFullYear() + '-' + currentdate.getDate() + '-' + currentdate.getDay();
    const listId = Math.floor(Math.random() * 1000)
    todoList.value.push({list_id: listId, list_todo: newList.value, list_date: dateToday, list_status: 0})
    newList.value = ''
};

const getActionFrom = (id, option) => {
    console.log(id)
    const listIndex = todoList.value.findIndex(item => item.list_id === id)
    if (option === 'remove') {
        if (listIndex !== -1) {
            todoList.value.splice(listIndex, 1);
        }
    } else {
        const listStatus = todoList.value[listIndex].list_status;
        todoList.value[listIndex].list_status = listStatus === 0 ? 1 : 0;
    }
};

const searchList = computed(() => {
    const searchTerm = searchQuery.value.toLowerCase()
    return todoList.value.filter(item => item.list_todo.toLowerCase().includes(searchTerm));
});

const handleClearList = () => {
    todoList.value = [];
};
const clearSearch = ()=>{
    searchQuery.value = ''
}

</script>

<template>
    <div class="main-container">
        <div class="container">
            <div class="top-container">
                <div class="inputs">
                    <label for="input_list">Add new list</label>
                    <div class="button">
                        <input type="text" placeholder="Add new list" v-model="newList" id="input_list" @keyup.enter="handleNewList"/>
                        <input type="button" value="Add" @click="handleNewList"/>
                    </div>
                </div>
            </div>
    <div v-if="todoList.length > 0">
        <div class="search-panels" >
    <div class="search-group">
        <input required="" type="text" name="text" autocomplete="on" v-model="searchQuery" class="input">
        <label class="enter-label">Search</label>
        <div class="btn-box">
            <button class="btn-search">
                <svg xmlns="http://www.w3.org/2000/svg" height="1em" viewBox="0 0 512 512"><path d="M416 208c0 45.9-14.9 88.3-40 122.7L502.6 457.4c12.5 12.5 12.5 32.8 0 45.3s-32.8 12.5-45.3 0L330.7 376c-34.4 25.2-76.8 40-122.7 40C93.1 416 0 322.9 0 208S93.1 0 208 0S416 93.1 416 208zM208 352a144 144 0 1 0 0-288 144 144 0 1 0 0 288z"></path><circle id="svg-circle" cx="208" cy="208" r="144"></circle></svg>
            </button>
        </div>
        <div class="btn-box-x">
            <button class="btn-cleare" @click="clearSearch">
                <svg xmlns="http://www.w3.org/2000/svg" height="1em" viewBox="0 0 384 512"><path d="M342.6 150.6c12.5-12.5 12.5-32.8 0-45.3s-32.8-12.5-45.3 0L192 210.7 86.6 105.4c-12.5-12.5-32.8-12.5-45.3 0s-12.5 32.8 0 45.3L146.7 256 41.4 361.4c-12.5 12.5-12.5 32.8 0 45.3s32.8 12.5 45.3 0L192 301.3 297.4 406.6c12.5 12.5 32.8 12.5 45.3 0s12.5-32.8 0-45.3L237.3 256 342.6 150.6z" id="cleare-line"></path></svg>
            </button>
        </div>
    </div>
    </div>
    <input type="button" class="clearlistbtn" value="Clear List" @click="handleClearList"/>
    </div>
            <div class="bottom-container">
                <ListComponent v-for="index in searchList" :key="index" v-bind="index" @sendActionTo="getActionFrom" />
            </div>
        </div>
    </div>
</template>


<style scoped>
.main-container{
    border: 2px solid #1a73e8;
    border-radius: 5%;
    background-color: #080606e8;
    color: aliceblue;
}
.main-container:hover{
    border: 5px solid color-mix(in srgb, #080606 100%, #1a73e8 100%);
    box-shadow: 10px #080606;
}

.container {
    margin-top: 50px;
    margin-left: 100px;
    margin-right: 100px;
    display: flex;
    flex-direction: column;
}
.container .top-container {
    display: flex;
    align-items: center;
    gap: 5px;
}

.top-container .inputs, .search-panels {
    display: flex;
    flex-direction: column;
    gap: 5px;
}
.search-panels {
  --default-color: #9e9e9e;
  --color-text: #080606;
  --color-active: #1a73e8;
  --color-active-input: #080606;
  --transition: 150ms cubic-bezier(0.4,0,0.2,1);
  --width-input: 200px;
  margin-top: 50px;
}

.search-group {
  position: relative;
}

.search-group input {
  width: var(--width-input);
  border: solid 1.5px var(--default-color);
  border-radius: 80px;
  background: none;
  padding: 1rem;
  padding-right: 3rem;
  font-size: 1rem;
  color: var(--color-active-input);
  transition: border var(--transition);
}

.enter-label {
  position: absolute;
  left: 15px;
  color: var(--default-color);
  pointer-events: none;
  transform: translateY(1rem);
  transition: var(--transition);
}
.input{
    border: 2px solid #1a73e8;
    background-color: aliceblue;
}

.enter-label {
  color: var(--default-color);
  pointer-events: none;
}

.input:focus, input:valid {
  outline: none;
  border: 1.5px solid var(--color-active);
}

.input:focus ~ label, input:valid ~ label {
  transform: translateY(-50%) scale(0.8);
  background-color: var(--color-active);
  border-radius: 20px;
  padding: .2em .6em;
  color: var(--color-text);
}

.btn-box {
  display: flex;
  justify-content: center;
  align-items: center;
  position: absolute;
  top: -1em;
  right: 80%;
  border-radius: 100%;
  transition: 300ms cubic-bezier(0.4,0,0.2,1);
  transition-delay: 100ms;
  opacity: 0;
}

.input:focus ~ .btn-box, input:valid ~ .btn-box {
  right: 10%;
  opacity: 1;
  transition-delay: 0s;
  transition: var(--transition);
}

.input:not(:focus) ~ .btn-box, input:not(:valid) ~ .btn-box {
  transition-property: right, opacity;
  transition-delay: 300ms;
}

.input:focus ~ .btn-box:hover, input:valid ~ .btn-box:hover {
  transform: scale(1.2);
}

.input:focus ~ .btn-box:hover:active, input:valid ~ .btn-box:hover:active {
  transform: scale(1);
}

.btn-search {
  position: relative;
  cursor: pointer;
  background-color: var(--color-active);
  width: 2em;
  height: 2em;
  top: 2.5px;
  border: none;
  border-radius: 100%;
  padding: 0;
  transition: var(--transition);
}

.btn-search:active {
  transform: scale(1);
}

.btn-search svg {
  position: absolute;
  top: 25%;
  left: 25%;
  fill: var(--color-text);
  width: 12px;
  height: 12px;
}

#svg-circle {
  fill: var(--color-text);
  transition: var(--transition);
}

.input:focus ~ .btn-box:hover #svg-circle, input:valid ~ .btn-box:hover #svg-circle {
  fill: transparent;
}

.btn-box-x {
  display: flex;
  justify-content: center;
  align-items: center;
  position: absolute;
  top: 1em;
  right: 7%;
  opacity: 0;
  transform: rotate(-90deg) scale(.1);
  transition: var(--transition);
}

.input:focus ~ .btn-box-x, input:valid ~ .btn-box-x {
  opacity: 1;
  transform: rotate(0) scale(1);
}

.btn-cleare {
  position: relative;
  cursor: pointer;
  background-color: transparent;
  width: 2em;
  height: 2em;
  border: none;
  border-radius: 100%;
  padding: 0;
  transition: var(--transition);
}

.btn-cleare svg {
  width: 15px;
  height: 15px;
}

#cleare-line {
  fill: var(--default-color);
}

.btn-box-x:hover #cleare-line {
  fill: var(--color-active);
}


.button input[type="button"] {
    padding: 7px 15px;
    margin-left: 2px;
    background-color: #10aedf;
    color: white;
}
.button input[type="button"]:hover {
    background-color: #03222c;
    color: rgb(49, 46, 46);
}
.button input[type="text"] {
    padding: 10px 20px;
}
.button input[type="text"]:hover {
    padding: 10px 20px;
    border: 1px solid #1a73e8;
    border-radius: 2%;
}
.button input[type="text"]:active {
    padding: 10px 20px;
    border: 1px solid #1a73e8;
    border-radius: 2%;
}

.bottom-container {
    margin-top: 1em;
    height: calc(100vh - 165px);
}
.clearlistbtn{
    margin-top: 20px;
    align-items: center;
    border: 1px solid #790606;
    background-color: #790606;
    cursor: pointer;
    color: aliceblue;
    text-align: center;
    border-radius: 5%;
    width: 150px;
    height: 30px;
}
.clearlistbtn:hover{
    background-color: #080606;
    color: aliceblue;
    border-radius: 50%;
    transition-duration: 0.3s;
}
</style>