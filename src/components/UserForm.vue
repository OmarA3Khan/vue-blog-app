<script lang="ts" setup>
import { computed, ref } from 'vue';
import { validate, length, required } from '../validation';
import { NewUser } from '../users'
import FormInput from './FormInput.vue';

defineProps<{
    error?: string,
    formType?: 'signIn' | 'signUp',
}>()

const emit = defineEmits<{
    (event: 'submit', payload: NewUser): void
}>()

const username = ref('')
const usernameStatus = computed(() => {
  return validate(username.value, [required, length({ min: 5, max: 10 })])
})

const password = ref('')
const passwordStatus = computed(() => {
  return validate(password.value, [required, length({ min: 10, max: 40 })])
})

const isInvalid = computed(() => {
    return (!usernameStatus.value.valid || !passwordStatus.value.valid)
});

async function handleSubmit () {
    if(isInvalid.value) {
        return
    }

    const newUser: NewUser = {
        username: username.value,
        password: password.value
    }

    try {
        emit('submit', newUser);
    } catch (e) {}

}
</script>

<template>
    <form class="form" @submit.prevent="handleSubmit">
        <FormInput data-testid="username" name="Username" v-model="username" :status="usernameStatus" type="text" />
        <FormInput data-testid="password" name="Password" v-model="password" :status="passwordStatus" type="password" />
        <div class="is-danger help">
            {{ error }}
        </div>
        <button class="button" :disabled="isInvalid">Submit</button>
    </form>
</template>

<style scoped>
.form {
    background: white;
    padding: 30px;
    margin-top: 50px;
}
</style>
