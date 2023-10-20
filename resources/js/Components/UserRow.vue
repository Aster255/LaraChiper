<script setup>
import { ref } from "vue";
import Modal from "./Modal.vue";
import DropdownLink from "@/Components/DropdownLink.vue";
import PrimaryButton from "@/Components/PrimaryButton.vue";
import { useForm } from "@inertiajs/vue3";

import dayjs from "dayjs";
import relativeTime from "dayjs/plugin/relativeTime";
dayjs.extend(relativeTime);

const props = defineProps(["user"]);

const modalshow = ref(false);

const form = useForm({
    name: props.user.name,
    email: props.user.email,
});
</script>

<template>
    <tr>
        <td>{{ user.id }}</td>
        <td>
            {{ user.name }}
        </td>
        <td>
            {{ user.email }}
        </td>
        <td>
            {{ dayjs(user.created_at).fromNow() }}
        </td>
        <td>
            {{ dayjs(user.updated_at).fromNow() }}
        </td>
        <td>
            <PrimaryButton @click="modalshow = true">edit</PrimaryButton>
        </td>
        <td>
            <DropdownLink
                as="button"
                :href="route('users.destroy', user.id)"
                method="delete"
            >
                Delete
            </DropdownLink>
        </td>
    </tr>

    <Modal :show="modalshow">
        <div class="p-3 m-3">
            <h2>EDIT USER</h2>
            <form
                @submit.prevent="
                    form.put(route('users.update', user.id), {});
                    modalshow = false;
                "
            >
                Name:
                <InputLabel for="name" value="Name" />
                <input
                    type="text"
                    id="name"
                    v-model="form.name"
                    required
                    autocomplete="name"
                /><br />
                Email
                <input
                    type="email"
                    v-model="form.email"
                    autocomplete="email"
                    required
                /><br />

                <PrimaryButton class="mt-4">Save</PrimaryButton>
            </form>

            <PrimaryButton @click="modalshow = false">cancel</PrimaryButton>
        </div>
    </Modal>
</template>
