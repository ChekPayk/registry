<template>
  <div class="template">
  <header>Регистратура</header>
  <p>Список специалистов</p>
  <div>
    <v-card v-for="doctor in doctors" class="v-card">
      <v-card-title>{{doctor.specialization}}</v-card-title>
      <v-card-text>
        <div>{{doctor.name}}</div>
        <div>Часы приема: {{doctor.workHours}}</div>
        <div>Осталось номерков: {{doctor.tickets}}</div>
      </v-card-text>
    </v-card>
  </div>
  </div>
</template>

<script>

export default {
  name: 'Registry',
  data: () => ({
    doctors: [
      {
        id: 1,
        specialization: 'Врач-терапевт',
        name: 'Круглов Степан Михайлович',
        workHours: '08:00-13:00',
        tickets: '12'
      },
      {
        id: 2,
        specialization: 'Врач-терапевт',
        name: 'Круглова Ионна Дмитриевна',
        workHours: '14:00-18:00',
        tickets: '2'
      },
      {
        id: 3,
        specialization: 'Хирург',
        name: 'Меженков Самуил Васильевич',
        workHours: '07:00-12:00',
        tickets: '3'
      },
      {
        id: 4,
        specialization: 'Окулист',
        name: 'Глазнова Варвара Алексеевна',
        workHours: '11:00-15:00',
        tickets: '14'
      },
      {
        id: 5,
        specialization: 'Невролог',
        name: 'Куйбышев Артур Михайлович',
        workHours: '10:00-15:00',
        tickets: '1'
      },
    ],
  }),
  methods: {
    openDialog() {

    }
  }
}
</script>

<style>
@import url('src/components/Style.css');
</style>
