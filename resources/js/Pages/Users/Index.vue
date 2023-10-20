<script setup>
import AuthenticatedLayout from "@/Layouts/AuthenticatedLayout.vue";
import Chirp from "@/Components/Chirp.vue";
import InputError from "@/Components/InputError.vue";
import PrimaryButton from "@/Components/PrimaryButton.vue";
import { useForm, Head } from "@inertiajs/vue3";
import UserRow from "@/Components/UserRow.vue";
import { ref } from "vue";
import Modal from "@/Components/Modal.vue";

defineProps(["users"]);

const form = useForm({
    name: "",
    email: "",
    password: "",
    password_confirmation: "",
});

const modalshow = ref(false);
</script>

<template>
    <Head title="Users" />

    <AuthenticatedLayout>
        <div class="flex">
            <PrimaryButton class="mx-auto" @click="modalshow = true"
                >CREATE A USER</PrimaryButton
            >
        </div>

        <table class="mx-auto">
            <tr>
                <th>ID</th>
                <th>user</th>
                <th>email</th>
                <th>created</th>
                <th>edit</th>
            </tr>
            <UserRow v-for="user in users" :key="user.id" :user="user" />
        </table>

        <Modal :show="modalshow">
            <div class="m-3 p-3">
                <h2>Create USER</h2>
                <form
                    @submit.prevent="
                        form.post(route('users.store'), {
                            onSuccess: () => form.reset(),
                        });
                        modalshow = false;
                    "
                >
                    Name:
                    <input
                        type="text"
                        id="name"
                        v-model="form.name"
                        required
                        autocomplete="name"
                    /><br />
                    Email:
                    <input
                        type="email"
                        v-model="form.email"
                        required
                        autocomplete="email"
                    /><br />
                    Password:
                    <input
                        type="password"
                        v-model="form.password"
                        autocomplete="new-password"
                    /><br />
                    Confirm Password:
                    <input
                        type="password"
                        v-model="form.password_confirmation"
                        autocomplete="new-password"
                    /><br />
                    <PrimaryButton class="mt-4">Save</PrimaryButton>
                </form>

                <button @click="modalshow = false">cancel</button>
            </div>
        </Modal>
    </AuthenticatedLayout>
</template>
