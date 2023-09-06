<script setup lang="ts">
import { ref, onMounted } from 'vue';
import { api } from 'boot/axios';

interface User {
  '.id'?: string;
  'attributes': string;
  'caller-id': string;
  'comment': string;
  'disabled': string;
  'group': string;
  'name': string;
  'otp-secret': string;
  'password': string;
  'shared-users': string;
}

const users = ref<User[]>([]);
const user = ref<User>({
  'group': 'default',
  'caller-id': 'bind',
  'disabled': 'false',
  'attributes': '',
  'otp-secret': '',
  'comment': '',
  'name': '',
  'password': '',
  'shared-users': '1'
})

// hooks
onMounted(async () => {
  try {
    const ret = await api.get('');
    console.log(ret);
    users.value = ret.data;
  } catch (err) {
    console.log(err);
  }
})

// methods
const requiredRule = [(val: string) => val && val.length > 0 || 'Please type something'];
const onSubmit = async () => console.log('submit');
</script>

<template>
  <q-page padding>
    <q-card>
      <q-card-section>
        <q-form @submit="onSubmit">
          <div class="row q-col-gutter-sm">
            <div class="col-2">
              <q-input v-model="user.name" label="usuário" lazy-rules :rules="requiredRule" outlined dense />
            </div>
            <div class="col-2">
              <q-input v-model="user.password" label="senha" lazy-rules :rules="requiredRule" outlined dense />
            </div>
            <div class="col-2">
              <q-btn label="Adicionar" type="submit" color="primary" />
            </div>
          </div>
        </q-form>
      </q-card-section>
    </q-card>
    <div class="q-mt-md">
      <span>usuários: {{ users.length }}</span>
      <pre>{{ users }}</pre>
    </div>
  </q-page>
</template>
