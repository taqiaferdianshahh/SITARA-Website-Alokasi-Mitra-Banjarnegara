<script setup>
    import AuthenticatedLayout from '@/Layouts/AuthenticatedLayout.vue';
    import { Head, Link, useForm } from '@inertiajs/vue3';
    import { ref } from 'vue'

    defineProps({
        posts: {
            type: Array,
            default: () => [],
        },
    });

    const form = useForm({});

    const deletePost = (id) => {
        form.delete(`posts/${id}`);
    };

    const showDeleteModal = ref(false)
    const postIdToDelete = ref(null)

    function openDeleteModal(id) {
        postIdToDelete.value = id
        showDeleteModal.value = true
    }

    function cancelDelete() {
        postIdToDelete.value = null
        showDeleteModal.value = false
    }

    function confirmDelete() {
        // Call your existing method
        deletePost(postIdToDelete.value)

        // Close modal
        showDeleteModal.value = false
    }
</script>

<template>
    <Head title="Manage Posts" />
    <AuthenticatedLayout>
        <template #header>
            <h2 class="font-semibold text-xl text-gray-800 leading-tight">Manage Posts</h2>
        </template>
        <div class="py-12">
            <div class="max-w-7xl mx-auto sm:px-6 lg:px-8">
                <div class="bg-white overflow-hidden shadow-sm sm:rounded-lg">
                    <div class="p-6 text-gray-900">
                        <Link href="posts/create">
                            <button class="bg-blue-500 hover:bg-blue-700 text-white text-sm font-semibold py-1 px-3 rounded my-3 flex items-center gap-1">
                                ➕ Create
                            </button>
                        </Link>
                        <table class="table-auto w-full">
                            <thead>
                                <tr class="text-left bg-gray-200">
                                    <th class="border px-4 py-2">ID</th>
                                    <th class="border px-4 py-2">Title</th>
                                    <th class="border px-4 py-2">Content</th>
                                    <th class="border px-4 py-2 text-center" width="180px">Action</th>
                                </tr>
                            </thead>
                            <tbody>
                            <tr v-for="(post, index) in posts" :key="post.id" class="text-left hover:bg-gray-100">
                                <td class="border px-4 py-2">{{ index + 1 }}</td>
                                <td class="border px-4 py-2">{{ post.title }}</td>
                                <td class="border px-4 py-2">{{ post.body }}</td>
                                <td class="border px-4 py-2 text-center">
                                <Link :href="`posts/${post.id}/edit`">
                                    <button class="bg-blue-500 hover:bg-blue-700 text-white text-sm font-semibold py-1 px-2 rounded inline-flex items-center gap-1">
                                    ✏️ Edit
                                    </button>
                                </Link>
                                <button
                                    class="bg-red-500 hover:bg-red-700 text-white text-sm font-semibold py-1 px-2 rounded inline-flex items-center gap-1 ml-2"
                                    @click="openDeleteModal(post.id)">
                                    🗑️ Delete
                                </button>
                                </td>
                            </tr>
                            </tbody>
                        </table>
                    </div>
                </div>
            </div>
        </div> 
    </AuthenticatedLayout>
    <!-- Delete Confirmation Modal -->
    <div v-if="showDeleteModal" class="fixed inset-0 flex items-center justify-center z-50 bg-black bg-opacity-50">
        <div class="bg-white rounded-lg shadow-md w-full max-w-sm p-6">
            <h2 class="text-lg font-semibold text-gray-800 mb-4">Confirm Deletion</h2>
            <p class="text-gray-600 mb-6">Are you sure you want to delete this post?</p>
            <div class="flex justify-end space-x-2">
            <button @click="cancelDelete"
                class="bg-gray-300 hover:bg-gray-400 text-gray-800 font-semibold py-1 px-4 rounded">Cancel</button>
            <button @click="confirmDelete"
                class="bg-red-500 hover:bg-red-700 text-white font-semibold py-1 px-4 rounded">Yes, Delete</button>
            </div>
        </div>
    </div>
</template>
