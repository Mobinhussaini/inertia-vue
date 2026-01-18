<script setup lang="ts">
import InputError from '@/components/InputError.vue';
import Button from '@/components/ui/button/Button.vue';
import Input from '@/components/ui/input/Input.vue';
import Label from '@/components/ui/label/Label.vue';
import Spinner from '@/components/ui/spinner/Spinner.vue';
import AppLayout from '@/layouts/AppLayout.vue';

import { update } from '@/actions/App/Http/Controllers/TaskController';
import { edit, index } from '@/routes/tasks';
import { Task, type BreadcrumbItem } from '@/types';
import { Form, Head } from '@inertiajs/vue3';

const { task } = defineProps<{ task: Task }>();

const breadcrumbs: BreadcrumbItem[] = [
    {
        title: 'Task',
        href: index().url,
    },
    {
        title: 'Update Task',
        href: edit(task?.id).url,
    },
];
</script>

<template>
    <Head :title="`Update Task ${task?.title}`" />

    <AppLayout :breadcrumbs="breadcrumbs">
        <div
            class="flex h-full flex-1 flex-col gap-4 overflow-x-auto rounded-xl p-4"
        >
            <div
                class="relative min-h-screen flex-1 overflow-hidden rounded-xl border border-sidebar"
            >
                <Form
                    v-bind="update.form(task)"
                    v-slot="{ errors, processing }"
                    class="mx-auto flex max-w-xl flex-col gap-6"
                >
                    <div class="grid gap-6">
                        <div class="grid gap-2">
                            <Label for="title">title</Label>
                            <Input
                                id="title"
                                type="text"
                                :default-value="task.title"
                                autofocus
                                :tabindex="4"
                                autocomplete
                                name="title"
                                placeholder="Title"
                            />
                            <InputError :message="errors.title" />
                        </div>

                        <div class="grid gap-2">
                            <Label for="description">Description</Label>
                            <Input
                                id="description"
                                type="text"
                                :default-value="task.description"
                                autofocus
                                :tabindex="4"
                                autocomplete
                                name="description"
                                placeholder="description"
                            />
                            <InputError :message="errors.description" />
                        </div>

                        <Button
                            type="submit"
                            class="mt-2 w-full"
                            tabindex="5"
                            :disabled="processing"
                            data-test="register-task-button"
                        >
                            <Spinner v-if="processing" />
                            Update Task
                        </Button>
                    </div>
                </Form>
            </div>
        </div>
    </AppLayout>
</template>
