<script setup lang="ts">
import InputError from '@/components/InputError.vue';
import Button from '@/components/ui/button/Button.vue';
import Input from '@/components/ui/input/Input.vue';
import Label from '@/components/ui/label/Label.vue';
import Spinner from '@/components/ui/spinner/Spinner.vue';
import AppLayout from '@/layouts/AppLayout.vue';

import { update } from '@/actions/App/Http/Controllers/RoleController';
import { Checkbox } from '@/components/ui/checkbox';
import { edit, index } from '@/routes/roles';
import { Permission, Role, type BreadcrumbItem } from '@/types';
import { Form, Head } from '@inertiajs/vue3';

const { role, permissions } = defineProps<{
    role: Role;
    permissions: Permission[];
}>();

const breadcrumbs: BreadcrumbItem[] = [
    {
        title: 'Role',
        href: index().url,
    },
    {
        title: 'Update Role',
        href: edit(role?.id).url,
    },
];
</script>

<template>
    <Head :title="`Update Role ${role?.name}`" />

    <AppLayout :breadcrumbs="breadcrumbs">
        <div
            class="flex h-full flex-1 flex-col gap-4 overflow-x-auto rounded-xl p-4"
        >
            <div
                class="relative min-h-screen flex-1 overflow-hidden rounded-xl border border-sidebar"
            >
                <Form
                    v-bind="update.form(role)"
                    v-slot="{ errors, processing }"
                    class="mx-auto flex max-w-xl flex-col gap-6"
                >
                    <div class="grid gap-6">
                        <div class="grid gap-2">
                            <Label for="name">Name</Label>
                            <Input
                                id="name"
                                type="text"
                                :default-value="role.name"
                                autofocus
                                :tabindex="4"
                                autocomplete
                                name="name"
                                placeholder="Full name"
                            />
                            <InputError :message="errors.name" />
                        </div>

                        <div class="grid gap-2">
                            <Label for="guard_name">Guard Name</Label>
                            <Input
                                id="guard_name"
                                type="text"
                                :default-value="role.guard_name"
                                autofocus
                                :tabindex="4"
                                autocomplete
                                name="guard_name"
                                placeholder="guard name"
                            />
                            <InputError :message="errors.guard_name" />
                        </div>

                        <div class="grid gap-2">
                            <Label for="permissions">Permissions</Label>
                            <div
                                class="flex flex-row items-center gap-2"
                                v-for="permission in permissions"
                                :key="permission.id"
                            >
                                <Checkbox
                                    :id="permission.name"
                                    :value="permission.name"
                                    :default-value="
                                        role.permissions?.some(
                                            (r) => r.name === permission.name,
                                        )
                                    "
                                    name="permissions[]"
                                />
                                <Label
                                    :for="permission.name"
                                    >{{ permission.name }}</Label
                                >
                            </div>
                            <InputError :message="errors.permissions" />
                        </div>

                        <Button
                            type="submit"
                            class="mt-2 w-full"
                            tabindex="5"
                            :disabled="processing"
                            data-test="register-role-button"
                        >
                            <Spinner v-if="processing" />
                            Update Role
                        </Button>
                    </div>
                </Form>
            </div>
        </div>
    </AppLayout>
</template>
