<template>
  <v-dialog max-width="400" v-model="dialogVisibility">
    <template v-slot:default="{ isActive }">
      <v-card title="Запись на прием">
        <v-card-text>
          <div>{{ doctorInCard.specialization }}</div>
          <div>{{ doctorInCard.name }}</div>
          <div>
            <v-radio-group inline v-model="selectedTicket">
              <v-radio
                :disabled="calculateDisabled(item)"
                v-for='item in doctorInCard.tickets'
                :label="item"
                :key="item"
                :value="item"
              />
            </v-radio-group>

            <!--            <v-radio-group inline v-model="selectedTickets">-->
            <!--              <v-radio-->
            <!--                v-for='item in timeOfTickets(doctorInCard.startInt, doctorInCard.endInt)'-->
            <!--                :label="item"-->
            <!--                :key="item"-->
            <!--                :value="item"-->
            <!--              />-->
            <!--            </v-radio-group>-->
          </div>

          <ToggleButton v-model="checked" onLabel="On" offLabel="Off"/>
        </v-card-text>

        <v-card-actions>
          <v-spacer></v-spacer>

          <v-btn
            text="Отмена"
            @click="isActive.value = false"
          ></v-btn>
          <v-btn
            text="Записаться"
            :disabled="!selectedTicket"
            @click="addTicket()"
          ></v-btn>
        </v-card-actions>
      </v-card>
    </template>
  </v-dialog>
</template>

<script>

export default {
  name: "Dialog",
  components: {},
  props: {
    // массив записей пользователя к врачу [{id:1,ticket:'08:30'}]
    userBookings: {}
  },
  data() {
    return {
      isButtonDisabled: true,
      doctorInCard: {
        id: null,
        specialization: null,
        name: null,
        workHours: null,
        tickets: null
      },
      dialogVisibility: false,
      selectedTicket: null,
    }
  },
  methods: {
    calculateDisabled(item) {
      if (this.userBookings.some(x => x.ticket === item)) return true;
      return false;
    },
    openDialog(doctor) {
      this.doctorInCard = doctor
      this.dialogVisibility = true
    },
    timeOfTickets(startTime, endTime) {
      console.log(startTime, endTime)
      let tickets = [];
      for (let i = startTime; i <= endTime; i++) { // выведет 0, затем 1, затем 2
        tickets.push(i + ':00')
      }
      return tickets;
    },
    addTicket() {
      const data = {
        id: this.doctorInCard.id,
        specialization: this.doctorInCard.specialization,
        ticket: this.selectedTicket
      }
      this.$emit('save', data)
      // console.log(data)
      this.hideDialog()

    },
    hideDialog() {
      this.dialogVisibility = !this.dialogVisibility;
    }
  },
}
</script>


<style scoped>

</style>
