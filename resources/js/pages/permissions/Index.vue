<script setup lang="ts">
import Table from '@/components/ui/table/Table.vue';
import TableBody from '@/components/ui/table/TableBody.vue';
import TableCaption from '@/components/ui/table/TableCaption.vue';
import TableCell from '@/components/ui/table/TableCell.vue';
import TableHead from '@/components/ui/table/TableHead.vue';
import TableHeader from '@/components/ui/table/TableHeader.vue';
import TableRow from '@/components/ui/table/TableRow.vue';
import AppLayout from '@/layouts/AppLayout.vue';
import { create, destroy, edit, index } from '@/routes/permissions';
import type { BreadcrumbItem, Permission } from '@/types';
import { Head, Link } from '@inertiajs/vue3';

const breadcrumbs: BreadcrumbItem[] = [
    {
        title: 'Permissions',
        href: index().url,
    },
];

const { permissions } = defineProps<{ permissions: Permission[] }>();
</script>

<template>
    <Head title="Permissions" />

    <AppLayout :breadcrumbs="breadcrumbs">
        <div class="mt-2 flex justify-end">
            <Link
                :href="create().url"
                class="mx-4 inline-block rounded-md bg-blue-600 px-4 py-2 text-white hover:bg-blue-700"
            >
                Create Permission
            </Link>
        </div>

        <div
            class="flex h-full flex-1 flex-col gap-4 overflow-x-auto rounded-xl p-4"
        >
            <div
                class="relative min-h-screen flex-1 overflow-hidden rounded-xl border border-sidebar"
            >
                <Table>
                    <TableCaption
                        >A list of your recent permissions.</TableCaption
                    >
                    <TableHeader>
                        <TableRow>
                            <TableHead class="w-25">Name</TableHead>
                            <TableHead>Guard name</TableHead>
                            <TableHead class="text-right">Actions</TableHead>
                        </TableRow>
                    </TableHeader>
                    <TableBody v-if="permissions.length > 0">
                        <TableRow
                            v-for="permission in permissions"
                            :key="permission.id"
                        >
                            <TableCell class="font-medium">
                                {{ permission.name }}
                            </TableCell>
                            <TableCell>{{ permission.guard_name }}</TableCell>
                            <TableCell class="gap-x-6 text-right">
                                <Link
                                    :href="edit(permission.id)"
                                    class="cursor-pointer text-blue-600 hover:text-blue-700"
                                >
                                    Edit
                                </Link>
                                <Link
                                    :href="destroy(permission.id)"
                                    class="cursor-pointer text-red-600 hover:text-red-700"
                                >
                                    Delete
                                </Link>
                            </TableCell>
                        </TableRow>
                    </TableBody>
                    <TableBody v-if="permissions.length === 0">
                        <TableRow>
                            <TableCell>No records found yet!</TableCell>
                        </TableRow>
                    </TableBody>
                </Table>
            </div>
        </div>
    </AppLayout>
</template>
