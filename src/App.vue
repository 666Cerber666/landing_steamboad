<template>
  <q-layout view="lHh Lpr lFf">
    <Header
      :active-count="activeCount"
      :inactive-count="inactiveCount"
      @add-visitor="openAddModal"
      @update:searchName="updateSearchName"
      @update:searchDate="updateSearchDate"
    />
    <q-page-container>
      <q-page>
        <VisitorList :visitors="filteredVisitors" @edit-visitor="openEditModal" />
        <AddVisitorModal
          :is-open="isModalOpen"
          :is-edit="isEdit"
          :visitor-data="currentVisitor"
          @update:model-value="isModalOpen = $event"
          @save-visitor="handleSaveVisitor"
        />
        <Footer :filter="filter" @update:filter="filter = $event" />
      </q-page>
    </q-page-container>
  </q-layout>
</template>

<script>
import { ref, computed } from 'vue';
import Header from './components/Header.vue';
import VisitorList from './components/VisitorList.vue';
import AddVisitorModal from './components/AddVisitorModal.vue';
import Footer from './components/Footer.vue';

export default {
  name: 'App',
  components: {
    Header,
    Footer,
    VisitorList,
    AddVisitorModal,
  },
  setup() {
    const visitors = ref([
      { number: 'Москва-Петербург', name: '2024/07/02', company: '2024/07/05', status: 'present', group: { label: 'Олимпик' } },
      { number: 'Ижевск-Воткинск', name: '2024/07/03', company: '2024/07/06', status: 'absent', group: { label: 'Карпатия' } },
      { number: 'Новгород-Краснодар', name: '2024/07/04', company: '2024/07/07', status: 'present', group: { label: 'Лузитания' } },
      { number: 'Воткинск-Ижевск', name: '2024/07/05', company: '2024/07/08', status: 'absent', group: { label: 'Мавритания' } }
    ]);
    const isModalOpen = ref(false);
    const isEdit = ref(false);
    const currentVisitor = ref(null);
    const filter = ref('all');
    const searchName = ref('');
    const searchDate = ref('');

    const filteredVisitors = computed(() => {
      return visitors.value.filter(visitor => {
        const matchesFilter = (filter.value === 'all' ||
          (filter.value === 'present' && visitor.status === 'present') ||
          (filter.value === 'absent' && visitor.status === 'absent'));
        
        const matchesSearch = visitor.number.toLowerCase().includes(searchName.value.toLowerCase()) &&
                              (searchDate.value === '' || visitor.name === searchDate.value); // Сравниваем даты

        return matchesFilter && matchesSearch;
      });
    });

    const activeCount = computed(() => {
      return visitors.value.filter(visitor => visitor.status === 'present').length;
    });

    const inactiveCount = computed(() => {
      return visitors.value.filter(visitor => visitor.status === 'absent').length;
    });

    const openAddModal = () => {
      isEdit.value = false;
      currentVisitor.value = null;
      isModalOpen.value = true;
    };

    const openEditModal = (visitor) => {
      isEdit.value = true;
      currentVisitor.value = { ...visitor }; // Deep copy to avoid direct mutation
      isModalOpen.value = true;
    };

    const handleSaveVisitor = (visitor) => {
      if (isEdit.value) {
        const index = visitors.value.findIndex(v => v.number === visitor.number);
        if (index !== -1) {
          visitors.value[index] = { ...visitor }; // Deep copy to ensure reactivity
        }
      } else {
        visitor.number = visitors.value.length + 1;
        visitors.value.push({ ...visitor }); // Deep copy to ensure reactivity
      }
    };

    const updateSearchName = (name) => {
      searchName.value = name;
    };

    const updateSearchDate = (date) => {
      searchDate.value = date;
      console.log(searchDate.value);
    };

    return {
      isModalOpen,
      isEdit,
      currentVisitor,
      openAddModal,
      openEditModal,
      filteredVisitors,
      handleSaveVisitor,
      filter,
      activeCount,
      inactiveCount,
      updateSearchName,
      updateSearchDate,
    };
  },
};
</script>
