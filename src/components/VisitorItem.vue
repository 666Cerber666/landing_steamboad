<template>
  <q-item>
    <q-item-section>{{ visitor.number }}</q-item-section>
    <q-item-section>{{ visitor.name }}</q-item-section>
    <q-item-section>{{ visitor.company }}</q-item-section>
    <q-item-section>{{ visitor.group?.label ?? 'No Group' }}</q-item-section>
    <q-item-section>
      <q-btn @click="handleClick" :class="['circle', visitor.status === 'present' ? 'green-circle' : 'red-circle']" :disable="isRedCircle">{{ textButton }}</q-btn>
      <div><p>{{ randomStatus }}</p></div>
    </q-item-section>
  </q-item>
</template>

<script>
export default {
  name: 'VisitorItem',
  props: {
    visitor: {
      type: Object,
      required: true,
    },
  },
  data() {
    return {
      randomNumber: Math.floor(Math.random() * 100) + 1,
    };
  },
  computed: {
    isRedCircle() {
      return this.visitor.status === 'absent';
    },
    randomStatus() {
      return this.visitor.status === 'present' ? `${this.randomNumber}/100` : '100/100';
    },
    textButton() {
      return this.visitor.status === 'present' ? `Купить` : 'Мест нет';
    },
  },
  methods: {
    handleClick() {
      this.$emit('click');
    },
  },
};
</script>

<style scoped>
.circle {
  border-radius: 5%;
  display: inline-block;
  color:white;
}
.green-circle {
  background-color: #80BB00;
}
.red-circle {
  background-color: #EC5937;
}
q-item-section {
  font-size: 24px;
}
</style>
