<template>
  <q-header class="bg-white text-black header">
    <q-toolbar>
      <q-toolbar-title>
        <img src="/src/assets/logo.jpg" style="width:150px;height:100px;">
      </q-toolbar-title>
      <SearchBar
        @update:searchName="updateSearchName"
        @update:searchDate="updateSearchDate"
      />
    </q-toolbar>
  </q-header>
</template>

<script>
import SearchBar from './SearchBar.vue';

export default {
  name: 'Header',
  components: {
    SearchBar,
  },
  props: {
    activeCount: {
      type: Number,
      default: 0,
    },
    inactiveCount: {
      type: Number,
      default: 0,
    },
  },
  methods: {
    openModal() {
      this.$emit('add-visitor');
    },
    updateSearchName(name) {
      this.$emit('update:searchName', name);
    },
    updateSearchDate(date) {
      this.$emit('update:searchDate', date);
    },
  },
};
</script>

<style scoped>
.header {
  padding: 24px 50px;
}
</style>
