<template>
  <q-layout view="hHh lpR fFf">
    <q-header elevated>
      <q-toolbar
        class="q-py-none q-px-sm"
        @mousedown="mouseDownToolbar($event)"
      >
        <q-btn
          size="xs"
          flat
          dense
          round
          icon="menu"
          aria-label="Menu"
          @click="toggleLeftDrawer"
        />

        <q-toolbar-title>
          Quasar App
        </q-toolbar-title>

        <q-btn
          size="xs"
          flat
          dense
          round
          icon="close"
          aria-label="Close"
          @click="exitApplication()"
        />
      </q-toolbar>
    </q-header>

    <q-drawer
      v-model="leftDrawerOpen"
      show-if-above
      bordered
    >
      <q-list>
        <q-item-label
          header
        >
          Essential Links
        </q-item-label>

        <EssentialLink
          v-for="link in essentialLinks"
          :key="link.title"
          v-bind="link"
        />
      </q-list>
    </q-drawer>

    <q-page-container>
      <router-view />
    </q-page-container>
  </q-layout>
</template>

<script setup lang="ts">
import { ref } from 'vue';
import EssentialLink, { EssentialLinkProps } from 'components/EssentialLink.vue';
import { exit } from '@tauri-apps/api/process';
import { appWindow, PhysicalPosition } from '@tauri-apps/api/window';

const essentialLinks: EssentialLinkProps[] = [
  {
    title: 'Docs',
    caption: 'quasar.dev',
    icon: 'school',
    link: 'https://quasar.dev'
  },
  {
    title: 'Github',
    caption: 'github.com/quasarframework',
    icon: 'code',
    link: 'https://github.com/quasarframework'
  },
  {
    title: 'Discord Chat Channel',
    caption: 'chat.quasar.dev',
    icon: 'chat',
    link: 'https://chat.quasar.dev'
  },
  {
    title: 'Forum',
    caption: 'forum.quasar.dev',
    icon: 'record_voice_over',
    link: 'https://forum.quasar.dev'
  },
  {
    title: 'Twitter',
    caption: '@quasarframework',
    icon: 'rss_feed',
    link: 'https://twitter.quasar.dev'
  },
  {
    title: 'Facebook',
    caption: '@QuasarFramework',
    icon: 'public',
    link: 'https://facebook.quasar.dev'
  },
  {
    title: 'Quasar Awesome',
    caption: 'Community Quasar projects',
    icon: 'favorite',
    link: 'https://awesome.quasar.dev'
  }
];

const leftDrawerOpen = ref(false)

var clickOffsetPosition = {x:0, y:0};

function toggleLeftDrawer() {
  leftDrawerOpen.value = !leftDrawerOpen.value
}

function exitApplication() {
  exit(1);
}

function mouseDownToolbar(event: MouseEvent) {
  console.log('Mouse down');
  clickOffsetPosition.x = event.x;
  clickOffsetPosition.y = event.y;
  window.addEventListener("mousemove", moveWindow);
  window.addEventListener("mouseup", () => {
    window.removeEventListener("mousemove", moveWindow);
  });
}

function moveWindow(event: MouseEvent) {
  appWindow.setPosition(new PhysicalPosition(event.screenX-clickOffsetPosition.x, event.screenY-clickOffsetPosition.y));
}
</script>