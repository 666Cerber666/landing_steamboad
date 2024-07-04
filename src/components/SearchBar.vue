<template>
  <div class="search-bar">
    <q-input color="blue-6" filled v-model="searchName" placeholder="Поиск по номеру" @change="updateSearchName" />
    <q-input filled v-model="searchDate" placeholder="Поиск по дате" :rules="['date']">
      <template v-slot:append>
        <q-icon name="event" class="cursor-pointer">
          <q-popup-proxy cover transition-show="scale" transition-hide="scale">
            <q-date v-model="internalSearchDate" mask="YYYY/MM/DD" @update:model-value="updateSearchDate">
              <div class="row items-center justify-end">
                <q-btn v-close-popup label="Закрыть" color="primary" flat />
              </div>
            </q-date>
          </q-popup-proxy>
        </q-icon>
      </template>
    </q-input>
  </div>
</template>

<script>
export default {
  name: 'SearchBar',
  data() {
    return {
      searchName: '',
      internalSearchDate: '',
    };
  },
  computed: {
    searchDate: {
      get() {
        return this.internalSearchDate || ''; // Если internalSearchDate пустое, возвращаем пустую строку
      },
      set(value) {
        this.internalSearchDate = value;
      }
    }
  },
  methods: {
    updateSearchName() {
      this.$emit('update:searchName', this.searchName.toLowerCase().trim());
    },
    updateSearchDate(value) {
      this.internalSearchDate = value || ''; // Устанавливаем пустую строку, если значение пустое
      this.$emit('update:searchDate', value || ''); // Отправляем пустую строку вместо null
    },
  },
};
</script>

<style scoped>
.search-bar {
  display: flex;
  gap: 10px;
}
</style>
