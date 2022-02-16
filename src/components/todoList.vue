<template>
    <ul class="tasks">
        <li
            v-for="(task, id) in pullTasks"
            :class="{ complete: task.complete }"
            :key="id"
        >
            <label>
                <ui-checkbox
                    @change="isCompleted(task.id, task)"
                    v-model="task.complete"
                >
                    {{ task.name }}
                </ui-checkbox>
            </label>
        </li>
    </ul>
</template>

<script>
export default {
    props: {
        pullTasks: {
            type: Array,
            default: () => [],
        },
    },
    data() {
        return {};
    },
    methods: {
        isCompleted(i, updateData) {
            let modifiedArray = JSON.parse(localStorage.getItem("tasks")) || [];
            modifiedArray[i] = updateData;
            localStorage.setItem(`tasks`, JSON.stringify(modifiedArray));
        },
    },
};
</script>

<style scoped lang="scss">
.tasks {
    list-style: none;
    padding: 0;

    li {
        max-width: 90%;
    }
    .complete {
        text-decoration: line-through;
        color: rgb(60, 184, 43);
    }
}
</style>
