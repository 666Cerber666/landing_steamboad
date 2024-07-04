<template>
  <q-list>
    <q-item class="b-border">
      <q-item-section>Название рейса</q-item-section>
      <q-item-section>От</q-item-section>
      <q-item-section>До</q-item-section>
      <q-item-section>Название лайнера</q-item-section>
      <q-item-section>Кол-во свободных мест</q-item-section>
    </q-item>
    <VisitorItem v-for="visitor in visitors" :key="visitor.number" :visitor="visitor" @click="editVisitor(visitor)" />
  </q-list>
</template>

<script>
import VisitorItem from './VisitorItem.vue';

export default {
  name: 'VisitorList',
  components: {
    VisitorItem,
  },
  props: {
    visitors: Array,
  },
  emits: ['edit-visitor'],
  methods: {
    editVisitor() {
      this.$emit('edit-visitor');
    },
  },
};
</script>

<style scoped>
.b-border{
  border-bottom:solid 4px rgb(111, 174, 255);
}
</style>
