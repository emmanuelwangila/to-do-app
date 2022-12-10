<template>
    <div
        class="bg-white dark:bg-[#25273c] shadow-xl md:shadow-2xl rounded-md transition-colors overflow-x-hidden"
    >
        <transition-group
            tag="div"
            enter-active-class="animate__animated animate__fadeIn"
            leave-active-class="animate__animated animate__bounceOutLeft"
        >
            <div v-for="todo in filteredTodos" :key="todo.id">
                <Todo
                    :todo="todo"
                    @delete-todo="deleteTodo"
                    @completed-todo="makeTodoCompleted"
                ></Todo>
            </div>
        </transition-group>
        <div
            class="mb-24 md:mb-0 flex justify-between items-center p-3.5 text-[#9394a5] relative"
            v-if="this.todos.length > 0"
        >
            <div class="text-xs">{{ itemsLeft }} items left</div>
            <div
                class="absolute -bottom-20 right-0 w-full rounded-lg justify-center p-4 bg-white dark:bg-[#25273c] shadow-lg md:shadow-none md:static md:bg-transparent md:p-0 md:rounded-none md:w-auto md:justify-start flex space-x-3.5 text-sm font-bold"
            >
                <p
                    tabindex="0"
                    :id="this.filterType === 'all' ? 'active' : ''"
                    class="status-items"
                    @click="addFilter('all')"
                    @keypress.enter="addFilter('all')"
                >
                    All
                </p>
                <p
                    tabindex="0"
                    :id="this.filterType === 'active' ? 'active' : ''"
                    class="status-items"
                    @click="addFilter('active')"
                    @keypress.enter="addFilter('active')"
                >
                    Active
                </p>
                <p
                    tabindex="0"
                    :id="this.filterType === 'completed' ? 'active' : ''"
                    class="status-items"
                    @click="addFilter('completed')"
                    @keypress.enter="addFilter('completed')"
                >
                    Completed
                </p>
            </div>
            <div
                tabindex="0"
                class="text-xs cursor-pointer dark:hover:text-[#d2d3db] hover:text-[#484b6a] transition-colors"
                @click="clearCompleted"
                @keypress.enter="clearCompleted"
            >
                Clear Completed
            </div>
        </div>
    </div>
</template>

<script>
import Todo from "./Todo.vue";

const filters = {
    all(todos) {
        return todos;
    },
    active(todos) {
        return todos.filter((todo) => !todo.completed);
    },
    completed(todos) {
        return todos.filter((todo) => todo.completed);
    },
};

export default {
    name: "TodoList",
    components: {
        Todo,
    },
    data() {
        return {
            itemsLeft: this.todos.filter((todo) => !todo.completed).length,
            filterType: "all",
        };
    },
    props: {
        todos: {
            type: Array,
            require: true,
        },
    },
    created() {
        if (window.location.search.includes("filter")) {
            this.filterType = window.location.search.replace("?filter=", "");
        }
    },
    updated() {
        this.itemsLeft = this.todos.filter((todo) => !todo.completed).length;
    },
    computed: {
        filteredTodos() {
            return filters[this.filterType](this.todos);
        },
    },
    methods: {
        deleteTodo(todoId) {
            this.$emit("delete-todo", todoId);
        },
        makeTodoCompleted(todoId) {
            this.$emit("completed-todo", todoId);
        },
        clearCompleted() {
            this.$emit("clearCompleted");
        },
        addFilter(filterType) {
            this.filterType = filterType;
            window.history.replaceState(
                {},
                null,
                `${window.location.origin}/?filter=${filterType}`
            );
        },
    },
};
</script>
