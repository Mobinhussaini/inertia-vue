<script setup lang="ts">
import Badge from '@/components/ui/badge/Badge.vue';
import Table from '@/components/ui/table/Table.vue';
import TableBody from '@/components/ui/table/TableBody.vue';
import TableCaption from '@/components/ui/table/TableCaption.vue';
import TableCell from '@/components/ui/table/TableCell.vue';
import TableHead from '@/components/ui/table/TableHead.vue';
import TableHeader from '@/components/ui/table/TableHeader.vue';
import TableRow from '@/components/ui/table/TableRow.vue';
import AppLayout from '@/layouts/AppLayout.vue';
import { create, destroy, edit, index } from '@/routes/users';
import { User, type BreadcrumbItem } from '@/types';
import { Head, Link } from '@inertiajs/vue3';

const breadcrumbs: BreadcrumbItem[] = [
    {
        title: 'Users',
        href: index().url,
    },
];

const { users } = defineProps<{ users: User[] }>();
</script>

<template>
    <Head title="Users" />

    <AppLayout :breadcrumbs="breadcrumbs">
        <div class="mt-2 flex justify-end">
            <Link
                :href="create().url"
                class="mx-4 inline-block rounded-md bg-blue-600 px-4 py-2 text-white hover:bg-blue-700"
            >
                Create User
            </Link>
        </div>

        <div
            class="flex h-full flex-1 flex-col gap-4 overflow-x-auto rounded-xl p-4"
        >
            <div
                class="relative min-h-screen flex-1 overflow-hidden rounded-xl border border-sidebar"
            >
                <Table>
                    <TableCaption>A list of your recent users.</TableCaption>
                    <TableHeader>
                        <TableRow>
                            <TableHead class="w-25">Name</TableHead>
                            <TableHead>Email</TableHead>
                            <TableHead>Roles</TableHead>
                            <TableHead class="text-right">Actions</TableHead>
                        </TableRow>
                    </TableHeader>
                    <TableBody>
                        <TableRow v-for="user in users" :key="user.id">
                            <TableCell class="font-medium">
                                {{ user.name }}
                            </TableCell>
                            <TableCell>{{ user.email }}</TableCell>
                            <TableCell>
                                <div class="flex items-center gap-2">
                                    <div v-for="role in user.roles" :key="role.id">
                                        <Badge class="bg-blue-500 text-white">
                                            {{ role.name }}
                                        </Badge>
                                    </div>
                                </div>
                            </TableCell>
                            <TableCell class="gap-x-6 text-right">
                                <Link
                                    :href="edit(user.id)"
                                    class="text-blue-600 hover:text-blue-700"
                                >
                                    Edit
                                </Link>
                                <Link
                                    :href="destroy(user.id)"
                                    class="text-red-600 hover:text-red-700"
                                >
                                    Delete
                                </Link>
                            </TableCell>
                        </TableRow>
                    </TableBody>
                </Table>
            </div>
        </div>
    </AppLayout>
</template>
