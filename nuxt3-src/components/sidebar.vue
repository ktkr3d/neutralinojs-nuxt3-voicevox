<template>
  <v-navigation-drawer v-model="drawer" :rail="rail" theme="dark" permanent expand-on-hover @click="rail = false">
    <v-list density="compact" nav>
      <v-list-item v-for="item in menuItems" :prepend-icon="item.icon" :title="item.title" :value="item.value" :to="item.to"></v-list-item>
    </v-list>
    <template v-slot:append>
      <v-btn class="mx-auto" variant="text" icon="mdi-arrow-expand-horizontal" @click.stop="rail = !rail"></v-btn>
    </template>
  </v-navigation-drawer>
</template>

<script setup lang="ts">
const drawer = useState("drawer", () => true);
const rail = useState("rail", () => true);
const menuItems = [
  { title: "Home", value: "home", to: "/", icon: "mdi-home" },
  { title: 'VOICEVOX', value: 'voicevox', to: '/voicevox', icon: 'mdi-account-voice' },
];
</script>
