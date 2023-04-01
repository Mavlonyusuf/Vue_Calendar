<script>
import { defineComponent } from "vue";
import FullCalendar from "@fullcalendar/vue3";
import dayGridPlugin from "@fullcalendar/daygrid";
import timeGridPlugin from "@fullcalendar/timegrid";
import interactionPlugin from "@fullcalendar/interaction";
import { INITIAL_EVENTS, createEventId } from "../Utils/event-utils";
import { ref } from "vue";
export default defineComponent({
  components: {
    FullCalendar,
  },

  setup() {
    let calendarOptions = ref({
      plugins: [dayGridPlugin, timeGridPlugin, interactionPlugin],
      headerToolbar: {
        left: "prev,next today",
        center: "title",
        right: "dayGridMonth,timeGridWeek,timeGridDay",
      },
      initialView: "dayGridMonth",
      initialEvents: INITIAL_EVENTS,
      editable: true,
      selectable: true,
      selectMirror: true,
      dayMaxEvents: true,
      weekends: true,
      select: handleDateSelect,
      eventClick: handleEventClick,
      eventsSet: handleEvents,
    });
    let currentEvents = ref([]);

    function handleDateSelect(selectInfo) {
      let title = prompt("Please enter a new title for your event");
      let calendarApi = selectInfo.view.calendar;
      calendarApi.unselect();
      if (title) {
        calendarApi.addEvent({
          id: createEventId(),
          title,
          start: selectInfo.startStr,
          end: selectInfo.endStr,
          allDay: selectInfo.allDay,
        });
      }
    }
    function handleEventClick(clickInfo) {
      if (
        confirm(
          `Are you sure you want to delete the event '${clickInfo.event.title}'`
        )
      ) {
        clickInfo.event.remove();
      }
    }
    function handleEvents(event) {
      currentEvents.value = event;
    }

    return {
      calendarOptions,
      currentEvents,
      handleDateSelect,
      handleEventClick,
    };
  },
});
</script>

<template>
  <div class="flex max-w-screen-2xl px-20 max-h-screen bg-gray-950">
    <div class="mr-5 p-5 border-r-2 border-fuchsia-600">
      <dl class="max-w-md text-slate-100 divide-y divide-slate-200">
        <div class="flex flex-col pb-3">
          <dd class="text-lg font-semibold">
            Select dates and you will be prompted to create a new event
          </dd>
        </div>
        <div class="flex flex-col py-3">
          <dd class="text-lg font-semibold">Drag, drop, and resize events</dd>
        </div>
        <div class="flex flex-col pt-3">
          <dd class="text-lg font-semibold">Click an event to delete it</dd>
        </div>
      </dl>

      <div class="">
        <p class="text-slate-100">All Events ({{ currentEvents.length }})</p>
        <ul class="space-y-2 mt-2">
          <li
            v-for="event in currentEvents"
            :key="event.id"
            class="border-2 border-fuchsia-600 px-2 rounded-lg text-center"
          >
            <p class="text-slate-100">{{ event.startStr }}</p>
            <p class="text-slate-100">{{ event.title }}</p>
          </li>
        </ul>
      </div>
    </div>
    <div class="w-full my-5 mx-auto">
      <FullCalendar class="demo-app-calendar" :options="calendarOptions">
        <template v-slot:eventContent="arg">
          <p>{{ arg.timeText }}</p>
          <p>{{ arg.event.title }}</p>
        </template>
      </FullCalendar>
    </div>
  </div>
</template>
<style>
@import url(https://fonts.googleapis.com/css2?family=Ubuntu:wght@100;200;300;400;500;600;700&display=swap);

body {
  font-family: "Ubuntu", sans-serif;
}
th,
td,
#fc-dom-1 {
  color: white;
}

:root {
  --fc-today-bg-color: #7ff25f;
  --fc-button-bg-color: #4a044e;
  --fc-button-active-bg-color: #c026d3;
  --fc-button-hover-bg-color: #d946ef;
  --fc-button-border-color: #c026d3;
  --fc-button-text-color: #f8fafc;
}
</style>
