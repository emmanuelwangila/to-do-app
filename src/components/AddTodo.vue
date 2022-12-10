<template>
    <div
        class="bg-white dark:bg-[#25273c] p-3 flex items-center rounded-md h-14 transition-colors relative"
        ref="addTodoContainer"
    >
        <span
            class="w-[18px] h-[18px] bg-transparent border border-gray-600 rounded-full inline-block cursor-pointer"
            @click="checkedAll"
        ></span>
        <form @submit.prevent="AddTodo" class="w-full h-full ml-5">
            <input
                type="text"
                placeholder="Create a new todo..."
                class="focus-visible:outline-none focus:underline focus:decoration-dotted bg-transparent w-full h-full inline-block text-[#4d5066] dark:text-[#cacde8]"
                v-model="todoTitle"
            />
        </form>
    </div>
</template>

<script>
export default {
    name: "AddTodo",
    data() {
        return {
            todoTitle: "",
        };
    },
    methods: {
        AddTodo() {
            if (!this.todoTitle.trim()) {
                return this.$refs.addTodoContainer.classList.add(
                    "border-2",
                    "border-red-400"
                );
            }
            this.$refs.addTodoContainer.classList.remove(
                "border-2",
                "border-red-400"
            );
            const newTodo = { title: this.todoTitle, completed: false };
            this.$emit("new-todo", newTodo);
            this.todoTitle = "";
        },
        checkedAll() {
            this.$emit("checkedAllTodos");
        },
    },
};
</script>
