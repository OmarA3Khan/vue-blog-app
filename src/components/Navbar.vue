<script lang="ts" setup>
import { useModal } from '../composables/modal';
import { useRouter } from 'vue-router';
import { useUsers } from '../stores/users';

const usersStore = useUsers()
const modal = useModal()
const router = useRouter();

async function logout() {
    await usersStore.logout();
    router.push({path: "/"});
}

</script>

<template>
    <div class="navbar">
        <div class="navbar-end">
            <div v-if="usersStore.currentUserId" class="buttons">
                <RouterLink to="/posts/new" class="button">New Post</RouterLink>
                <button id="logout" class="button" @click="logout()">Log Out</button>
            </div>
            <div v-else class="buttons">
                <button id="sign-up" class="button" @click="modal.showModal('signUp')">Sign Up</button>
                <button data-testid="sign-in" class="button" @click="modal.showModal('signIn')">Sign In</button>
            </div>
        </div>
    </div>

    <Teleport to="#modal">
        <component :is="modal.component.value" />
    </Teleport>
</template>