<script setup lang="ts">
    import AppLayout from '@/layouts/AppLayout.vue';
    import Button from '@/components/ui/button/Button.vue';
    import Spinner from '@/components/ui/spinner/Spinner.vue';
    import Input from '@/components/ui/input/Input.vue';
    import Label from '@/components/ui/label/Label.vue';
    import InputError from '@/components/InputError.vue';

    import { create, index } from '@/routes/roles';
    import { Role, type BreadcrumbItem } from '@/types';
    import { Form, Head } from '@inertiajs/vue3';
    import { store } from '@/actions/App/Http/Controllers/RoleController';

const breadcrumbs: BreadcrumbItem[] = [
    {
        title: 'Roles',
        href: index().url,
    },
    {
        title: 'Create Role',
        href: create().url,
    },
];

const { roles } = defineProps<{ roles: Role[] }>();
</script>

<template>
    <Head title="Create Role" />

    <AppLayout :breadcrumbs="breadcrumbs">
        <div
            class="flex h-full flex-1 flex-col gap-4 overflow-x-auto rounded-xl p-4"
        >
            <div
                class="relative min-h-screen flex-1 overflow-hidden rounded-xl border border-sidebar"
            >

            <Form v-bind="store.form()" :reset-on-success="['password', 'password_confirmation' ]" v-slot="{errors, processing }" class="max-w-xl mx-auto flex flex-col gap-6">
                <div class="grid gap-6">
                    <div class="grid gap-2">
                        <Label for="name">Name</Label>
                        <Input id="name" type="text" autofocus :tabindex="1" autocomplete name="name" placeholder="Role name" />
                        <InputError :message="errors.name" />
                    </div>

                    <div class="grid gap-2">
                        <Label for="guard_name">Select Guard</Label>
                        <Input id="guard_name" type="text" autofocus :tabindex="2" autocomplete name="guard_name" placeholder="Guard name" />
                        <InputError :message="errors.guard_name" />
                    </div>

                    <Button type="submit" class="mt-2 w-full" :tabindex="3" :disabled="processing" data-test="create-role-button">
                        <Spinner v-if="processing"/>
                        Create Role
                    </Button>

                </div>
            </Form>

            </div>
        </div>
    </AppLayout>
</template>
