<template>
  <q-page class="row items-center justify-evenly">
    <example-component
      title="Example component"
      active
      :todos="todos"
      :meta="meta"
    ></example-component>
    <q-btn color="primary" label="Test" @click="$event => openGoogle()"/>
  </q-page>
</template>

<script setup lang="ts">
import { Todo, Meta } from 'components/models';
import ExampleComponent from 'components/ExampleComponent.vue';
import { ref } from 'vue';
import { getName } from "@tauri-apps/api/app"
import { appWindow, WebviewWindow } from "@tauri-apps/api/window"

const todos = ref<Todo[]>([
  {
    id: 1,
    content: 'ct1'
  },
  {
    id: 2,
    content: 'ct2'
  },
  {
    id: 3,
    content: 'ct3'
  },
  {
    id: 4,
    content: 'ct4'
  },
  {
    id: 5,
    content: 'ct5'
  }
]);
const meta = ref<Meta>({
  totalCount: 1200
});

getName().then((name) => {
  console.log("Application name:", name);
})

function centerWindow() {
  appWindow.center();
}

function openGoogle() {
  const webview = new WebviewWindow(Math.random().toString(36).substring(7), {
    url:window.location.href,
    title:"Google"
  });
  webview.once("tauri://created", function() {
    console.log("Webview created!")
  });
}
</script>
