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
                <button @click="handleBtnClick(props.list_id, '')">{{ props.list_status === 0 ? 'Done' : 'Undone' }}</button>
                <button @click="handleBtnClick(props.list_id, 'remove')">Remove</button>
            </div>
        </li>
    </ul>
</template>

<style>
ul {
    width: 100%;
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

.mark-done {
    text-decoration: line-through;
    font-style: italic;
    color: #4b4a4a;
}
</style>