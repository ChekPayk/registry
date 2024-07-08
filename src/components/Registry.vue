<template>
  <div class="ml-5">
    <h1 class="h1">Регистратура</h1>
    <div class="bookings">
      <h3>Ваши записи</h3>
      <div
        style="padding-top: 5px;
        display:flex;
        flex-direction: row;
        flex-wrap: nowrap"
        v-for="item in bookings"
      >
        <div>
          <h4>{{ item.specialization }}</h4>
          <p>{{ getDoctorById(item.id).name }}</p>
          <p>{{ getDate(0).name }} {{ item.ticket }}</p>
        </div>
        <div style="padding-top: 25px; padding-left: 10px">
          <v-icon icon="mdi-delete" color="red" @click="deleteStudent(index)"/>
        </div>
      </div>
    </div>

    <p>Список специалистов</p>
    <div>
      <v-radio-group inline v-model="selectedDate">
        <v-radio :label="getDate(0).name" :value="getDate(0).date"></v-radio>
        <v-radio :label="getDate(1).name" :value="getDate(1).date"></v-radio>
        <v-radio :label="getDate(2).name" :value="getDate(2).date"></v-radio>
        <v-radio :label="getDate(3).name" :value="getDate(3).date"></v-radio>
      </v-radio-group>
    </div>

    <div>
      <v-card @click="openDoctorCard(doctor)" v-for="doctor in doctors" class="doctor-card ">
        <v-card-title>{{ doctor.specialization }}</v-card-title>
        <v-card-text>
          <div>{{ doctor.name }}</div>
          <div>Часы приема: {{ doctor.startTime + '-' + doctor.endTime }}</div>
          <div>Осталось номерков: {{ doctor.tickets.length }}</div>
        </v-card-text>
      </v-card>
    </div>
    <BookingDialog ref="doctorsCardRef" @save='onSave' :userBookings="bookings"
    />
  </div>
</template>

<script>

import BookingDialog from "@/components/BookingDialog.vue";
import moment from "moment";

export default {
  name: 'Registry',
  components: {BookingDialog},
  data: () => ({
    doctors: [
      {
        id: 1,
        specialization: 'Врач-терапевт',
        name: 'Круглов Степан Михайлович',
        startTime: '08:00',
        endTime: '13:00',
        startInt: 8,
        endInt: 13,
        tickets: [
          '08:00',
          '09:00',
          '12:00',
        ]
      },
      {
        id: 2,
        specialization: 'Врач-терапевт',
        name: 'Круглова Ионна Дмитриевна',
        startTime: '14:00',
        endTime: '18:00',
        startInt: 14,
        endInt: 18,
        tickets: [
          '14:00',
          '18:00'
        ]
      },
      {
        id: 3,
        specialization: 'Хирург',
        name: 'Меженков Самуил Васильевич',
        startTime: '07:00',
        endTime: '12:00',
        startInt: 7,
        endInt: 12,
        tickets: [
          '08:00',
          '10:00',
          '11:00'
        ]
      },
      {
        id: 4,
        specialization: 'Окулист',
        name: 'Глазнова Варвара Алексеевна',
        startTime: '11:00',
        endTime: '15:00',
        startInt: 11,
        endInt: 15,
        tickets: [
          '11:00',
          '12:00',
          '13:00',
          '14:00'
        ]
      },
      {
        id: 5,
        specialization: 'Невролог',
        name: 'Куйбышев Артур Михайлович',
        startTime: '10:00',
        endTime: '15:00',
        startInt: 10,
        endInt: 15,
        tickets: [
          '10:00',
          '13:00',
          '15:00'
        ]
      },
    ],
    selectedDate: null,
    selectedTickets: null,
    bookings: []
  }),
  methods: {
    //TODO: добавить метод
    openDoctorCard(doctor) {
      this.$refs.doctorsCardRef.openDialog({...doctor})
    },
    // она будет возвращать объект с текущей датой плюс количество дней
    getDate(amount) {
      return {
        name: moment(new Date()).add(amount, 'day').format('DD.MM'),
        date: moment(new Date()).add(amount, 'day').format('DD.MM.YYYY'),
      }
    },
    timeOfTickets(startTime, endTime) {
      console.log(startTime, endTime)
      let tickets = [];
      for (let i = startTime; i <= endTime; i++) { // выведет 0, затем 1, затем 2
        tickets.push(i + ':00')
      }
      return tickets;
    },
    onSave(data) {
      this.bookings.push(data)
    },
    getDoctorById(doctorId) {
      return this.doctors.find(item => item.id === doctorId)
    },
    deleteStudent(index) {
      this.bookings.splice(index, 1)
    },
  },
  mounted() {
    this.selectedDate = this.getDate(0).date
    console.log(this.timeOfTickets(10, 13))
  },
}
</script>

<style>
.h1 {
  text-align: center;
}

.bookings {
  width: 500px;
  background-color: antiquewhite;
  padding: 10px;
}

.doctor-card {
  margin-top: 10px;
  width: 250px;
  cursor: pointer; /* Делаем курсор указателем, чтобы указать, что это интерактивный элемент */
}
</style>
