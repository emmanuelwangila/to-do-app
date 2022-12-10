<template>
    <div
        class="absolute inset-0 z-0 h-64 w-full bg-cover bg-no-repeat"
        :style="{ backgroundImage: `url(${img})` }"
    ></div>
    <div
        class="mx-auto my-0 mt-14 md:mt-20 relative z-10 space-y-12 w-11/12 md:w-3/5 lg:w-2/5"
    >
        <div class="flex items-center justify-between">
            <h1
                class="text-4xl md:text-5xl text-white font-bold tracking-[18px]"
            >
                TODO
            </h1>
            <img
                tabindex="0"
                @click="changeTheme"
                @keypress.enter="changeTheme"
                :src="require(`./assets/images/icon-${themeIcon}.svg`)"
                :alt="themeIcon"
                class="cursor-pointer select-none"
            />
        </div>
        <AddTodo
            @new-todo="addTodo"
            @checkedAllTodos="checkedAllTodos"
        ></AddTodo>
        <TodoList
            :todos="todos"
            @delete-todo="deleteTodo"
            @completed-todo="makeTodoCompleted"
            @clear-completed="clearCompleted"
        ></TodoList>
    </div>
</template>

<script>
import backDesktopLight from "./assets/images/bg-desktop-light.jpg";
import backDesktopDark from "./assets/images/bg-desktop-dark.jpg";
import AddTodo from "./components/AddTodo.vue";
import TodoList from "./components/TodoList.vue";

export default {
    name: "App",
    components: {
        AddTodo,
        TodoList,
    },
    data() {
        return {
            themeIcon: "sun",
            img:
                localStorage.theme === "light"
                    ? backDesktopLight
                    : backDesktopDark,
            todos: [],
            id: 0,
        };
    },
    created() {
        if (localStorage.theme === "light" || !localStorage.getItem("theme")) {
            document.documentElement.classList.remove("dark");
            localStorage.setItem("theme", "light");
            this.themeIcon = "moon";
            this.img = backDesktopLight;
        } else {
            document.documentElement.classList.add("dark");
            localStorage.setItem("theme", "dark");
            this.themeIcon = "sun";
            this.img = backDesktopDark;
        }
        if (localStorage.getItem("todos")) {
            JSON.parse(localStorage.getItem("todos")).forEach((todo) => {
                this.todos.push(todo);
            });
        }
        this.id =
            this.todos.length > 0 ? this.todos[this.todos.length - 1].id : 0;
    },
    methods: {
        changeTheme() {
            if (localStorage.theme === "light") {
                document.documentElement.classList.add("dark");
                localStorage.setItem("theme", "dark");
                this.themeIcon = "sun";
                this.img = backDesktopDark;
            } else {
                document.documentElement.classList.remove("dark");
                localStorage.setItem("theme", "light");
                this.themeIcon = "moon";
                this.img = backDesktopLight;
            }
        },
        addTodo(todo) {
            this.id++;
            todo.id = this.id;
            this.todos.push(todo);
            localStorage.setItem("todos", JSON.stringify(this.todos));
        },
        checkedAllTodos() {
            if (this.todos.every((todo) => todo.completed)) {
                this.todos = this.todos.map((todo) => ({
                    ...todo,
                    completed: false,
                }));
            } else {
                this.todos = this.todos.map((todo) => ({
                    ...todo,
                    completed: true,
                }));
            }
            localStorage.setItem("todos", JSON.stringify(this.todos));
        },
        deleteTodo(todoId) {
            this.todos = this.todos.filter((todo) => todo.id != todoId);
            localStorage.setItem("todos", JSON.stringify(this.todos));
        },
        makeTodoCompleted(todoId) {
            this.todos = this.todos.map((todo) =>
                todo.id == todoId
                    ? { ...todo, completed: !todo.completed }
                    : todo
            );
            localStorage.setItem("todos", JSON.stringify(this.todos));
        },
        clearCompleted() {
            this.todos = this.todos.filter((todo) => !todo.completed);
            localStorage.setItem("todos", JSON.stringify(this.todos));
        },
    },
};
</script>
