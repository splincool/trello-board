<script setup lang="ts">
import { Column, Task } from '@/types';
import { nanoid } from 'nanoid';
import draggable from 'vuedraggable';

const columns = ref<Column[]>([
    {
        id: nanoid(),
        title: "Backlog",
        tasks: [
            {
                id: nanoid(),
                title: "Create marketing landing page",
                createdAt: new Date(),
            },
            {
                id: nanoid(),
                title: "Develop cool new feature",
                createdAt: new Date(),
            },
            {
                id: nanoid(),
                title: "Develop a weather widget",
                createdAt: new Date(),
            }
        ],
    },
    {
        id: nanoid(),
        title: 'Selected for Dev',
        tasks: [
            {
                id: nanoid(),
                title: 'Create a branch',
                createdAt: new Date(),
            },
            {
                id: nanoid(),
                title: 'Change width of left menu',
                createdAt: new Date(),
            }
        ],
    },
    {
        id: nanoid(),
        title: 'In Progress',
        tasks: [],
    },
    {
        id: nanoid(),
        title: 'QA',
        tasks: [],
    },
    {
        id: nanoid(),
        title: 'Complete',
        tasks: [],
    }
]);

const alt = useKeyModifier("Alt");

function addNewTask(task: Task, column: Column): void {
    column.tasks.push(task);
}
</script>

<template>
    <div>
        <draggable
            v-model="columns"
            group="columns"
            item-key="id"
            animation="150"
            handle=".drag-handle"
            class="flex gap-4 overflow-x-auto items-start"
        >
            <template #item="{ element: column }: { element: Column }">
                <div class="column bg-gray-200 p-5 rounded min-w-[250px]">
                    <header class="font-bold mb-4">
                        <DragHandle />
                        {{ column.title }}
                    </header>

                    <draggable
                        v-model="column.tasks"
                        :group="{ name: 'tasks', pull: alt ? 'clone' : true }"
                        item-key="id"
                        handle=".drag-handle"
                    >
                        <template #item="{ element: task } : {element: Task}">
                            <div>
                                <TrelloBoardTask :task="task" />
                            </div>
                        </template>
                    </draggable>

                    <footer>
                        <!-- <NewTask @add="column.tasks.push($event)"/> -->
                        <NewTask @add="addNewTask($event, column)"/>
                    </footer>
                </div>
            </template>
        </draggable>
    </div>
</template>