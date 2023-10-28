<script setup>
import {defineEmits,defineProps} from 'vue'
const props = defineProps({
    list_id: {
        type: Number,
        default: 0
    },
    list_todo: {
        type: String,
        default: ''
    },
    list_date: {
        type: String,
        default: ''
    },
    list_status: {
        type: Number,
        default: 0
    },
    
});

const emit = defineEmits('sendActionTo')

const handleBtnClick = (id, option) => {
  emit('sendActionTo', id, option)
};
</script>

<template>
    <ul>
        <li>
            <span :class="props.list_status === 0 ? 'list' : 'list mark-done'">
                <span>{{ props.list_todo }}</span>
                <span>{{ props.list_date }}</span>
            </span>
            <div class="buttons">
                <button class="donebtn" @click="handleBtnClick(props.list_id, '')">{{ props.list_status === 0 ? 'Done' : 'Undone' }}</button>
                <button class="removebtn" @click="handleBtnClick(props.list_id, 'remove')">Remove</button>
            </div>
        </li>
    </ul>
</template>

<style>
ul {
    width: 100%;
    border: 5px solid #000;
    padding-top: 5px;
    padding-bottom: 5px;
    border-radius: 2%;
    background-color: azure;
    color: black;
}
ul li {
    list-style: none;
    display: flex;
}

.list {
    display: flex;
    flex-direction: column;
    font-weight: 700;
    margin-right: 4em;
    text-transform: capitalize;
}

.list span:nth-child(2) {
    font-size: 12px;
    color: #2e2e2e;
}

.buttons {
    display: flex;
    flex-direction: row;
    flex-wrap: nowrap;
    justify-content: flex-end;
    align-items: center;
    align-content: flex-end;
    gap: 5px;
}
.donebtn{
    padding: 4px 8px;
    border: 2px solid #0aee3b;
    cursor: pointer;
    border-radius: 2px;
    background-color: #0aee3b;
    color: white;
}
.donebtn:hover{
    border: 2px solid #035a16;
    background-color: #035a16;
}
.removebtn{
    padding: 4px 8px;
    cursor: pointer;
    border: 2px solid #f10f0f;
    background-color: #f10f0f;
    border-radius: 2px;
    color: white;
}
.removebtn:hover{
    
    border: 2px solid #6b0808;
    background-color: #6b0808;

}

.mark-done {
    text-decoration: line-through;
    font-style: italic;
    color: #4b4a4a;
}
</style>