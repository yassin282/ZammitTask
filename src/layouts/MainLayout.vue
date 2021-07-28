<template>
  <q-layout view="lHh Lpr lFf bg-grey">
    <q-header class="bg-white text-grey-10" elevated>
      <q-toolbar>
        <q-btn
          flat
          dense
          round
          icon="menu"
          aria-label="Menu"
          @click="toggleLeftDrawer"
          class="mobile"
        ></q-btn>

        <q-toolbar-title class="desktop-only text-black text-bold text-grey-10 q-pa-md">All Products</q-toolbar-title>
        <q-space></q-space>
        <div class="q-pa-md">
          <q-img src="../assets/avatar.png" style="min-width: 50px"></q-img>
        </div>
      </q-toolbar>
    </q-header>

    <q-drawer
      :width="250"
      v-model="leftDrawerOpen"
      show-if-above
      bordered
      class="bg-grey-10 text-white"
    >
      <q-list>
        <div class="row">
          <div class="col-4 q-pa-md">
            <q-img src="../assets/avatar.png" style="max-width:50px; border-radius:25px"></q-img>
          </div>
          <div class="col-8 q-pa-md" style="margin:auto">
            <q-item-label class="text-subtitle2 text-white">Dots</q-item-label>
            <div class="text-caption">Free trial plan</div>
          </div>
        </div>
        <EssentialLink
          v-for="link in essentialLinks"
          :key="link.title"
          v-bind="link"
          @click="check(link)"
          :class="selected===link.title?'text-black':''"
        />
      </q-list>
    </q-drawer>

    <q-page-container>
      <router-view/>
    </q-page-container>
  </q-layout>
</template>

<script>
import EssentialLink from "components/EssentialLink.vue";

const linksList = [
  {
    title: "Home",
    icon: "home"
  },
  {
    title: "Orders",
    icon: "receipt"
  },
  {
    title: "Products",
    icon: "inventory_2"
  },
  {
    title: "All products",
    icon: "none"
  },
  {
    title: "Inventory",
    icon: "none"
  },
  {
    title: "Collections",
    icon: "none"
  },
  {
    title: "Customers",
    icon: "groups"
  },
  {
    title: "Analytics",
    icon: "signal_cellular_alt"
  },
  {
    title: "Discounts",
    icon: "local_offer"
  },
  {
    title: "Website builder",
    icon: "laptop"
  },
  {
    title: "Marketing",
    icon: "campaign"
  }
];

import { defineComponent, ref } from "vue";

export default defineComponent({
  name: "MainLayout",

  components: {
    EssentialLink
  },

  setup() {
    const leftDrawerOpen = ref(false);
    return {
      selected: "",
      desktop: window.innerWidth > 767,
      essentialLinks: linksList,
      leftDrawerOpen,
      toggleLeftDrawer() {
        leftDrawerOpen.value = !leftDrawerOpen.value;
      }
    };
  }
});
</script>

<style lang="scss" scoped>
@media screen and (min-width: 767px) {
  .mobile {
    display: none;
  }
}
.menu-list .q-item {
  border-radius: 0 32px 32px 0;
}
</style>
