<script setup lang="ts">
import InputError from '@/components/InputError.vue';
import Button from '@/components/ui/button/Button.vue';
import Input from '@/components/ui/input/Input.vue';
import Label from '@/components/ui/label/Label.vue';
import Spinner from '@/components/ui/spinner/Spinner.vue';
import AppLayout from '@/layouts/AppLayout.vue';

import { store } from '@/actions/App/Http/Controllers/PermissionController';
import { create, index } from '@/routes/permissions';
import { Permission, type BreadcrumbItem } from '@/types';
import { Form, Head } from '@inertiajs/vue3';

const breadcrumbs: BreadcrumbItem[] = [
    {
        title: 'Permissions',
        href: index().url,
    },
    {
        title: 'Create Permission',
        href: create().url,
    },
];

const { permissions } = defineProps<{ permissions: Permission[] }>();
</script>

<template>
    <Head title="Create Permission" />

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
                            <Label for="name">Name</Label>
                            <Input
                                id="name"
                                type="text"
                                autofocus
                                :tabindex="1"
                                autocomplete
                                name="name"
                                placeholder="Permission name"
                            />
                            <InputError :message="errors.name" />
                        </div>

                        <div class="grid gap-2">
                            <Label for="guard_name">Guard Name</Label>
                            <Input
                                id="guard_name"
                                type="text"
                                autofocus
                                :tabindex="2"
                                autocomplete
                                name="guard_name"
                                placeholder="Guard name"
                            />
                            <InputError :message="errors.guard_name" />
                        </div>

                        <Button
                            type="submit"
                            class="mt-2 w-full"
                            :tabindex="3"
                            :disabled="processing"
                            data-test="create-permission-button"
                        >
                            <Spinner v-if="processing" />
                            Create Permission
                        </Button>
                    </div>
                </Form>
            </div>
        </div>
    </AppLayout>
</template>
