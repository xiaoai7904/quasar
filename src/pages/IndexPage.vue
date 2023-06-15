<template>
  <q-page class="row items-center justify-evenly">
    <!-- <example-component title="Example component" active :todos="todos" :meta="meta"></example-component> -->
    <div v-for="item in logs" :key="item">
      <p>{{ item }}</p>
    </div>
  </q-page>
</template>

<script setup lang="ts">
// import { Todo, Meta } from 'components/models';
// import ExampleComponent from 'components/ExampleComponent.vue';
import { ref } from 'vue';
// import { CordovaSmsPlugin } from 'boot/capacitorSmsPlugin';

import { Plugins } from '@capacitor/core';

const { SmsPlugin } = Plugins;

const logs = ref<unknown[]>([])

if (process.env.MODE === 'capacitor') {
  SmsPlugin.onSMSArrive().subscribe((result: unknown) => {
    console.log('SMS received successfully:', result);
  });
  SmsPlugin.startReception().then(() => {
    logs.value.push('SmsPlugin started successfully')
    console.log('SmsPlugin started successfully');
  }).catch((error: unknown) => {
    logs.value.push('Failed to start SmsPlugin' + error);
    console.error('Failed to start SmsPlugin:', error);
  });

  SmsPlugin.stopReception().then(() => {
    logs.value.push('SmsPlugin stopped successfully')
    console.log('SmsPlugin stopped successfully');
  }).catch((error: unknown) => {
    logs.value.push('Failed to stop SmsPlugin' + error);
    console.error('Failed to stop SmsPlugin:', error);
  });

  SmsPlugin.onReception().subscribe((result: unknown) => {
    logs.value.push('SMS received successfully' + result);
    console.log('SMS received successfully:', result);
  });
}


// CordovaSmsPlugin.startReception().then(() => {
//   logs.value.push('SmsPlugin started successfully')
//   console.log('SmsPlugin started successfully');
// }).catch((error: any) => {
//   logs.value.push('Failed to start SmsPlugin' + error);
//   console.error('Failed to start SmsPlugin:', error);
// });

// CordovaSmsPlugin.stopReception().then(() => {
//   logs.value.push('SmsPlugin stopped successfully')
//   console.log('SmsPlugin stopped successfully');
// }).catch((error: any) => {
//   logs.value.push('Failed to stop SmsPlugin' + error);
//   console.error('Failed to stop SmsPlugin:', error);
// });

// CordovaSmsPlugin.onReception().subscribe((result: any) => {
//   logs.value.push('SMS received successfully' + result);
//   console.log('SMS received successfully:', result);
// });


// const todos = ref<Todo[]>([
//   {
//     id: 1,
//     content: 'ct1'
//   },
//   {
//     id: 2,
//     content: 'ct2'
//   },
//   {
//     id: 3,
//     content: 'ct3'
//   },
//   {
//     id: 4,
//     content: 'ct4'
//   },
//   {
//     id: 5,
//     content: 'ct5'
//   }
// ]);
// const meta = ref<Meta>({
//   totalCount: 1200
// });


</script>
