<script setup>
import AuthenticatedLayout from '@/Layouts/AuthenticatedLayout.vue';
import { Head, useForm, Link } from "@inertiajs/vue3";

// Props
const props = defineProps({
    post: {
        type: Object,
        required: true,
    },
});

// Form with initial data from props
const form = useForm({
    title: props.post.title,
    body: props.post.body,
});

// Submit update request
const submit = () => {
    form.put(`/posts/${props.post.id}`);
};
</script>

<template>
    <Head title="Edit Post" />

    <AuthenticatedLayout>
        <template #header>
            <h2 class="font-semibold text-xl text-gray-800 leading-tight">Edit Post</h2>
        </template>

        <div class="py-12">
            <div class="max-w-3xl mx-auto sm:px-6 lg:px-8">
                <div class="bg-white overflow-hidden shadow-sm sm:rounded-lg">
                    <div class="p-6 text-gray-900">

                        <!-- Back Button -->
                        <Link href="/posts">
                            <button class="bg-gray-500 hover:bg-gray-700 text-white text-sm font-semibold py-1 px-3 rounded inline-flex items-center gap-1 mb-4">
                                🔙 Back
                            </button>
                        </Link>

                        <!-- Edit Form -->
                        <form @submit.prevent="submit">
                            <div class="mb-4">
                                <label for="title" class="block text-gray-700 text-sm font-bold mb-2">Title:</label>
                                <input
                                    id="title"
                                    type="text"
                                    v-model="form.title"
                                    placeholder="Enter Title"
                                    class="shadow appearance-none border rounded w-full py-2 px-3 text-gray-700 leading-tight focus:outline-none focus:shadow-outline"
                                />
                                <div v-if="form.errors.title" class="text-red-500 text-sm mt-1">{{ form.errors.title }}</div>
                            </div>

                            <div class="mb-4">
                                <label for="body" class="block text-gray-700 text-sm font-bold mb-2">Body:</label>
                                <textarea
                                    id="body"
                                    v-model="form.body"
                                    placeholder="Enter Body"
                                    class="shadow appearance-none border rounded w-full py-2 px-3 text-gray-700 leading-tight focus:outline-none focus:shadow-outline"
                                ></textarea>
                                <div v-if="form.errors.body" class="text-red-500 text-sm mt-1">{{ form.errors.body }}</div>
                            </div>

                            <!-- Submit Button -->
                            <button
                                type="submit"
                                class="bg-blue-500 hover:bg-blue-700 text-white text-sm font-semibold py-2 px-4 rounded inline-flex items-center gap-1"
                                :disabled="form.processing"
                            >
                                💾 Update
                            </button>
                        </form>

                    </div>
                </div>
            </div>
        </div>
    </AuthenticatedLayout>
</template>