<script setup lang="ts">
import InputError from '@/components/InputError.vue';
import Button from '@/components/ui/button/Button.vue';
import Input from '@/components/ui/input/Input.vue';
import Label from '@/components/ui/label/Label.vue';
import Spinner from '@/components/ui/spinner/Spinner.vue';
import AppLayout from '@/layouts/AppLayout.vue';

import { store } from '@/actions/App/Http/Controllers/TaskController';
import { create, index } from '@/routes/tasks';
import { Task, type BreadcrumbItem } from '@/types';
import { Form, Head } from '@inertiajs/vue3';

const breadcrumbs: BreadcrumbItem[] = [
    {
        title: 'Tasks',
        href: index().url,
    },
    {
        title: 'Create Task',
        href: create().url,
    },
];

const { tasks } = defineProps<{ tasks: Task[] }>();
</script>

<template>
    <Head title="Create Task" />

    <AppLayout :breadcrumbs="breadcrumbs">
        <div
            class="flex h-full flex-1 flex-col gap-4 overflow-x-auto rounded-xl p-4"
        >
            <div
                class="relative min-h-screen flex-1 overflow-hidden rounded-xl border border-sidebar"
            >
                <Form
                    v-bind="store.form()"
                    v-slot="{ errors, processing }"
                    class="mx-auto flex max-w-xl flex-col gap-6"
                >
                    <div class="grid gap-6">
                        <div class="grid gap-2">
                            <Label for="title">Title</Label>
                            <Input
                                id="title"
                                type="text"
                                autofocus
                                :tabindex="1"
                                autocomplete
                                name="title"
                                placeholder="Task title"
                            />
                            <InputError :message="errors.title" />
                        </div>

                        <div class="grid gap-2">
                            <Label for="description">Description</Label>
                            <Input
                                id="description"
                                type="text"
                                autofocus
                                :tabindex="2"
                                autocomplete
                                name="description"
                                placeholder="description"
                            />
                            <InputError :message="errors.description" />
                        </div>

                        <Button
                            type="submit"
                            class="mt-2 w-full"
                            :tabindex="3"
                            :disabled="processing"
                            data-test="create-task-button"
                        >
                            <Spinner v-if="processing" />
                            Create Task
                        </Button>
                    </div>
                </Form>
            </div>
        </div>
    </AppLayout>
</template>
