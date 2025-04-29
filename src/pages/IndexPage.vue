<template>
  <q-page class="flex flex-center" v-if="!showListVisible">
    <div class="column items-center q-gutter-md" id="warning-text">
      <div class="text-h5 pd-auto">{{ noTaskHeading }}</div>

      <div class="q-mt-md">
        <q-fab
          v-model="fab2"
          external-label
          vertical-actions-align="left"
          color="purple"
          icon="add"
          direction="down"
        >
          <q-fab-action
            external-label
            class="bg-header"
            @click="showDialog = true"
            icon="add_circle"
            label="Add Meeting"
          />
          <q-fab-action
            external-label
            color="secondary"
            @click="onClick"
            icon="alarm"
            label="Meeting Reminder"
          />
        </q-fab>
      </div>
    </div>

    <q-dialog v-model="showDialog" persistent>
      <AddMeetingForm @added="handleAddMeeting" />
    </q-dialog>
  </q-page>

  <q-page v-if="showListVisible">
    <div class="q-pa-md">
      <div class="q-gutter-md">
        <div class="row q-col-gutter-md" v-for="(meeting, index) in meetingList" :key="index">
          <div class="col-12 q-ma-xs">
            <q-card flat bordered class="bg-grey-1 hover-card q-mx-lg">
              <q-card-section class="q-ma-xs bg-grey">
                <div class="text-subtitle1 text-weight-bold">{{ meeting.title }}</div>
                <div class="text-caption">{{ meeting.date }}</div>
              </q-card-section>
            </q-card>
          </div>
        </div>
      </div>
    </div>

    <q-page-sticky position="bottom-right" :offset="[18, 18]">
      <q-fab icon="add" direction="left" color="accent">
        <q-fab-action @click="showDialog = true" class="bg-header" icon="add" label="Add Meeting" />
        <q-fab-action @click="onClick" class="bg-header" icon="alarm" label="Meeting Reminder" />
      </q-fab>
    </q-page-sticky>

    <q-dialog v-model="showDialog" persistent>
      <AddMeetingForm @added="handleAddMeeting" />
    </q-dialog>
  </q-page>
</template>

<script>
import { defineComponent } from 'vue'
import AddMeetingForm from './AddMeetingForm.vue'

export default defineComponent({
  name: 'IndexPage',
  components: { AddMeetingForm },

  data() {
    return {
      noTaskHeading:
        "You don't have any meetings added. To add a meeting, click on the below '+' button.",
      fab2: false,
      showDialog: false,
      meetingList: [],
      showListVisible: false,
    }
  },

  methods: {
    handleAddMeeting(title) {
      this.meetingList.push({
        title: title.title,
        date: title.dateTime,
      })
      this.showDialog = false

      this.isItemAdded()
    },

    onClick() {
      console.log('Other FAB action clicked')
    },

    isItemAdded() {
      if (this.meetingList.length === 0) {
        this.showListVisible = false
      } else {
        this.showListVisible = true
      }
    },
  },
})
</script>
