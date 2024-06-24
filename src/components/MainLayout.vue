<template>
    <q-layout view="hHh lpR fFf">
      <q-header elevated class="bg-grey-8 text-white">
        <q-toolbar class="custom-toolbar">
          <q-toolbar-title class="custom-toolbar-title">
            RAKA SANDHEO RAMBANI UAS PBK (223510641)
          </q-toolbar-title>
          <div class="toolbar-tabs-container">
            <q-tabs class="toolbar-tabs">
              <q-route-tab to="/Todo" label="Kegiatan" />
              <q-route-tab to="/Post" label="Postingan" />
              <q-route-tab to="/Album" label="Gallery" />
            </q-tabs>
          </div>
        </q-toolbar>
      </q-header>
  
      <q-page-container>
        <router-view />
      </q-page-container>
    </q-layout>
  </template>
  
  <script>
  import { ref } from "vue";
  
  export default {
    setup() {
      const leftDrawerOpen = ref(false);
  
      return {
        leftDrawerOpen,
        toggleLeftDrawer() {
          leftDrawerOpen.value = !leftDrawerOpen.value;
        },
      };
    },
  };
  </script>
  
  <style scoped>
  .custom-toolbar {
    padding: 20px; 
    display: flex;
    align-items: flex-end; 
  }
  
  .custom-toolbar-title {
    font-size: 2.5rem;
    flex-grow: 1; 
  }
  
  .toolbar-tabs-container {
    display: flex;
    align-items: flex-end; 
  }
  
  .toolbar-tabs {
    margin-left: auto; 
  }
  </style>
  