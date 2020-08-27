<template>
  <q-layout view="lHh Lpr lFf">
    <q-header  >
      <q-toolbar style="background: linear-gradient(to right, #cb2d3e, #ef473a);">
        <q-btn
          flat
          dense
          round
          icon="menu"
          aria-label="Menu"
          @click="left_drawer_open = !left_drawer_open"
        />

        <q-toolbar-title>

        </q-toolbar-title>

          <q-btn flat round
                 @click="$q.dark.toggle()"
                 :icon="$q.dark.isActive ? 'nights_stay' : 'wb_sunny'"
          />
      </q-toolbar>
    </q-header>

    <q-drawer
      v-model="left_drawer_open"
      bordered
      @click="left_drawer_open = !left_drawer_open"
      :overlay="true"
      :content-style="{zIndex: '4000'}"
    >
      <q-list padding>
          <q-item  clickable class="GPL__drawer-item" to="/">
            <q-item-section avatar>
              <q-icon name="dashboard" />
            </q-item-section>
            <q-item-section>
              <q-item-label>Covid Tracker</q-item-label>
            </q-item-section>
          </q-item>
      </q-list>
    </q-drawer>
    <q-page-container>
      <router-view/>
    </q-page-container>
  </q-layout>
</template>

<script>

    export default {
        name: 'MainLayout',

        data() {
            return {
                left_drawer_open: false,
                // left_mini_drawer_open: true,
            }
        },
        methods: {
            setDrawer() {
                this.left_drawer_open = !this.left_drawer_open;
            },
        }
    }
</script>
<style>

</style>
