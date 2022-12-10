<template>
    <div
        class="group w-full relative flex items-center p-5 gap-x-4 h-14 border-b border-gray-300 dark:border-gray-700 text-[#484b6a] dark:text-[#cacde8] select-text transition-colors"
    >
        <span
            class="w-[18px] h-[18px] relative bg-transparent border border-gray-600 rounded-full inline-block transition-all cursor-pointer"
            :id="isTodoCompleted"
            @click="makeTodoCompleted"
        ></span>
        <p class="transition-all">{{ todo.title }}</p>
        <img
            :src="require(`.././assets/images/icon-${crossIcon}.svg`)"
            alt="cross"
            class="w-3.5 h-3.5 hidden group-hover:inline-block absolute top-2/4 transform -translate-y-2/4 right-4 cursor-pointer"
            @click="deleteTodo"
        />
    </div>
</template>

<script>
import checkIcon from ".././assets/images/icon-check.svg";

export default {
    name: "Todo",
    props: {
        todo: {
            type: Object,
            require: true,
        },
    },
    data() {
        return {
            crossIcon: "cross",
            checkedIcon: `url(${checkIcon})`,
            isTodoCompleted: this.todo.completed ? "checked" : "",
        };
    },
    updated() {
        this.isTodoCompleted = this.todo.completed ? "checked" : "";
    },
    methods: {
        deleteTodo() {
            this.$emit("delete-todo", this.todo.id);
        },
        makeTodoCompleted() {
            this.$emit("completed-todo", this.todo.id);
        },
    },
};
</script>

<style scoped>
#checked::before {
    content: "";
    position: absolute;
    top: 50%;
    left: 50%;
    transform: translate(-50%, -50%);
    display: inline-block;
    width: 11px;
    height: 10px;
    background-image: v-bind(checkedIcon);
    background-size: cover;
    background-repeat: no-repeat;
}
</style>
