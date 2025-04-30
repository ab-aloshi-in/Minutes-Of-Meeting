<template>
  <q-card class="q-pa-md" id="meeting-container" style="min-width: 500px">
    <q-card-section>
      <div class="text-h6">Add New Meeting</div>
    </q-card-section>

    <q-card-section class="q-gutter-md">
      <q-input v-model="meetingTitle" label="Meeting Title" />

      <q-input v-model="formattedDate" label="Meeting Date" readonly @click="showDate = true" />
      <q-dialog v-model="showDate">
        <q-date v-model="date" @update:model-value="updateDate" :options="dateOptions" />
      </q-dialog>

      <q-input
        v-model="formattedTime"
        id="myDate"
        label="Meeting Time"
        readonly
        @click="showTime = true"
      />
      <q-dialog v-model="showTime">
        <q-time v-model="time" format24h @update:model-value="updateTime" />
      </q-dialog>
    </q-card-section>

    <q-card-actions align="right">
      <q-btn flat label="Cancel" v-close-popup />
      <q-btn flat label="Add" @click="addMeeting" />
    </q-card-actions>
  </q-card>
</template>

<script>
import { date,useQuasar } from 'quasar'

export default {
  name: 'AddMeetingForm',
  data() {
    return {
      meetingTitle: '',
      date: '',
      time: '',
      showDate: false,
      showTime: false,
      $q: useQuasar(),
    }
  },
  computed: {
    formattedDate() {
      return this.date ? date.formatDate(this.date, 'YYYY-MM-DD') : ''
    },
    formattedTime() {
      return this.time ? this.time : ''
    },
  },
  methods: {
    updateDate(val) {
      this.date = val
      this.showDate = false
    },
    updateTime(val) {
      this.time = val
      this.showTime = false
    },
    addMeeting() {
      if (!this.meetingTitle || !this.formattedDate || !this.formattedTime) {
        this.$q.notify({
          message: 'Please fill all the details before adding the meeting.',
          color: 'negative',
          position: 'top',
        })
        return
      }

      const fullDateTime = `${this.formattedDate} ${this.formattedTime}`
      this.$emit('added', {
        title: this.meetingTitle,
        dateTime: fullDateTime,
      })
      this.meetingTitle = ''
      this.date = ''
      this.time = ''
    },
    dateOptions(dateStr) {
      const today = new Date()
      const date = new Date(dateStr)
      return date >= new Date(today.getFullYear(), today.getMonth(), today.getDate())
    },
  },
  mounted() {
    this.$q = useQuasar()
  },
}
</script>

<style>
#meeting-container {
  border: 2px solid #e8b415;
}
</style>
