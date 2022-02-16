<template>
    <div class="todo">
        <h1 class="title">Checklist</h1>
        <ui-tabs :fullwidth="fullwidth">
            <ui-tab title="Pending" @select="selectTabForTask = false">
                <vuescroll :ops="ops">
                    <todo-list :pullTasks="pending" />
                </vuescroll>
            </ui-tab>

            <ui-tab title="Completed" @select="selectTabForTask = true">
                <vuescroll :ops="ops">
                    <todo-list :pullTasks="completed" />
                </vuescroll>
            </ui-tab>
        </ui-tabs>

        <div class="todo__add-item">
            <ui-textbox
                placeholder="Enter your task"
                v-model="newTaskName"
                @keydown-enter="addTask"
                ref="taskInput"
            >
            </ui-textbox>
            <ui-button color="primary" @click="addTask" icon="add">
                Add
            </ui-button>
        </div>
    </div>
</template>

<script>
import vuescroll from "vuescroll";

import todoList from "./todoList.vue";

export default {
    components: {
        vuescroll,
        todoList,
    },
    data() {
        return {
            ops: {
                bar: {
                    background: "#2196F3",
                },
                scrollPanel: {
                    scrollingX: false,
                },
            },
            fullwidth: true,
            newTaskName: "",
            tasks: [],
            selectTabForTask: false,
        };
    },
    watch: {
        tasks: {
            handler() {
                localStorage.setItem(`tasks`, JSON.stringify(this.tasks));
            },
        },
    },
    methods: {
        addTask() {
            this.newTaskName &&
                this.tasks.push({
                    name: this.newTaskName,
                    complete: this.selectTabForTask,
                    id: this.tasks.length,
                }) &&
                this.resetInput();
        },
        resetInput() {
            this.$refs.taskInput.reset();
        },
    },
    mounted() {
        if (localStorage.getItem("tasks"))
            this.tasks = JSON.parse(localStorage.getItem("tasks"));
    },
    computed: {
        pending() {
            return this.tasks.filter((e) => !e.complete);
        },
        completed() {
            return this.tasks.filter((e) => e.complete);
        },
    },
};
</script>

<style lang="scss">
.todo {
    width: 50%;
    height: 65%;
    margin: auto;
    background: #fff;
    padding: 20px;
    border-radius: 5px;
    box-shadow: rgba(0, 0, 0, 0.3) 3px 3px 15px;

    .title {
        margin-top: 0;
    }
}
.todo__add-item {
    display: flex;
    justify-content: space-between;

    .ui-textbox {
        flex-grow: 0.75;
    }
}
.ui-tabs {
    height: 78%;
    .ui-tabs__body {
        height: 85%;
    }
    .ui-tab {
        height: 100%;
    }
}
.__vuescroll {
    .__view {
        min-height: 94% !important;
    }
}
</style>
