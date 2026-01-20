<script setup lang="ts">
import { Badge } from '@/components/ui/badge';
import Table from '@/components/ui/table/Table.vue';
import TableBody from '@/components/ui/table/TableBody.vue';
import TableCaption from '@/components/ui/table/TableCaption.vue';
import TableCell from '@/components/ui/table/TableCell.vue';
import TableHead from '@/components/ui/table/TableHead.vue';
import TableHeader from '@/components/ui/table/TableHeader.vue';
import TableRow from '@/components/ui/table/TableRow.vue';
import AppLayout from '@/layouts/AppLayout.vue';
import { create, destroy, edit, index } from '@/routes/roles';
import type { BreadcrumbItem, Role } from '@/types';
import { Head, Link } from '@inertiajs/vue3';

const breadcrumbs: BreadcrumbItem[] = [
    {
        title: 'Roles',
        href: index().url,
    },
];

const { roles } = defineProps<{ roles: Role[] }>();
</script>

<template>
    <Head title="Roles" />

    <AppLayout :breadcrumbs="breadcrumbs">
        <div class="mt-2 flex justify-end">
            <Link
                :href="create().url"
                class="mx-4 inline-block rounded-md bg-blue-600 px-4 py-2 text-white hover:bg-blue-700"
            >
                Create Role
            </Link>
        </div>

        <div
            class="flex h-full flex-1 flex-col gap-4 overflow-x-auto rounded-xl p-4"
        >
            <div
                class="relative min-h-screen flex-1 overflow-hidden rounded-xl border border-sidebar"
            >
                <Table>
                    <TableCaption>A list of your recent roles.</TableCaption>
                    <TableHeader>
                        <TableRow>
                            <TableHead class="w-25">Name</TableHead>
                            <TableHead>Guard name</TableHead>
                            <TableHead>Permissions</TableHead>
                            <TableHead class="text-right">Actions</TableHead>
                        </TableRow>
                    </TableHeader>
                    <TableBody v-if="roles.length > 0">
                        <TableRow v-for="role in roles" :key="role.id">
                            <TableCell class="font-medium">
                                {{ role.name }}
                            </TableCell>
                            <TableCell>{{ role.guard_name }}</TableCell>
                            <TableCell>
                                <span
                                    v-for="permission in role.permissions"
                                    :key="permission.id"
                                >
                                    <Badge class="bg-green-500 text-white">{{
                                        permission.name
                                    }}</Badge>
                                </span>
                            </TableCell>
                            <TableCell class="gap-x-6 text-right">
                                <Link
                                    :href="edit(role.id)"
                                    class="cursor-pointer text-blue-600 hover:text-blue-700"
                                >
                                    Edit
                                </Link>
                                <Link
                                    :href="destroy(role.id)"
                                    class="cursor-pointer text-red-600 hover:text-red-700"
                                >
                                    Delete
                                </Link>
                            </TableCell>
                        </TableRow>
                    </TableBody>
                    <TableBody v-if="roles.length === 0">
                        <TableRow>
                            <TableCell>No records found yet!</TableCell>
                        </TableRow>
                    </TableBody>
                </Table>
            </div>
        </div>
    </AppLayout>
</template>
