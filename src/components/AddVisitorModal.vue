<template>
  <q-dialog :model-value="isOpen" @update:model-value="updateOpen">
    <q-card style="width: 700px; max-width: 80vw;">
      <q-card-section>
        <div class="text-h6">{{ isEdit ? 'Купить билет' : 'Добавить' }} посетителя</div>
      </q-card-section>
      <q-card-section>
        <q-input color="blue-6" filled v-model="name" label="ФИО" />
        <q-input color="blue-6" filled v-model="company" label="Компания" />
        <q-select color="blue-6" filled v-model="selectedGroup" :options="groups" label="Пакет" />
        <p>Итоговая цена:</p>
        <p>{{ CountSum }}</p>
      </q-card-section>
      <q-card-actions align="right">
        <q-btn flat label="Закрыть" @click="closeDialog" />
        <q-btn color="blue-6" flat label="Купить" @click="handleSubmit" />
      </q-card-actions>
    </q-card>
  </q-dialog>
</template>

<script>
import { ref, computed, watch } from 'vue';

export default {
  name: 'AddVisitorModal',
  props: {
    isOpen: Boolean,
    isEdit: Boolean,
  },
  setup(props, { emit }) {
    const name = ref('');
    const company = ref('');
    const selectedGroup = ref(''); // Устанавливаем по умолчанию 'econom'

    const groups = [
      { label: 'Эконом', value: 'econom', price: 50 },
      { label: 'Стандарт', value: 'standard', price: 100 },
      { label: 'Премиум', value: 'premium', price: 200 },
    ];

    const closeDialog = () => {
      emit('update:modelValue', false);
    };

    const handleSubmit = () => {
      // Можно добавить логику для обработки отправки данных
      closeDialog();
    };

    const updateOpen = (value) => {
      emit('update:modelValue', value);
    };

    const CountSum = computed(() => {
      const selected = groups.find((group) => group.label === selectedGroup.value.label);
      return selected ? `${selected.price} ₽` : '0 ₽';
    });

    return {
      name,
      company,
      selectedGroup,
      groups,
      closeDialog,
      handleSubmit,
      updateOpen,
      CountSum,
    };
  },
};
</script>
