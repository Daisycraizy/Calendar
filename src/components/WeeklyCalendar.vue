<template>
  <div class="weekly-calendar" @mouseleave="stopDragging">
    <div class="time-column">
      <div v-for="hour in 24" :key="hour" class="time-slot">{{ hour }}:00</div>
    </div>
    <div class="day-columns">
      <div class="day-column" v-for="(intervals, day) in selectedIntervals" :key="day">
        <div class="day-header">{{ day.toUpperCase() }}</div>
        <div
          class="time-slot"
          v-for="hour in 24"
          :key="hour"
          :class="{ selected: isSelected(day, hour) }"
          @mousedown="startDragging(day, hour)"
          @mouseenter="dragging(day, hour)"
          @mouseup="stopDragging"
        ></div>
        <button @click="toggleAllDay(day)">Весь день</button>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  props: {
    calendarData: {
      type: Object,
      required: true
    }
  },
  data() {
    return {
      selectedIntervals: this.initializeIntervals(),
      isDragging: false,
    };
  },
  methods: {
    initializeIntervals() {
      const intervals = {};
      for (const day in this.calendarData) {
        intervals[day] = this.calendarData[day].map(interval => ({
          ...interval,
          selected: false
        }));
      }
      return intervals;
    },
    isSelected(day, hour) {
      return this.selectedIntervals[day].some(interval => 
        interval.bt <= hour * 60 && interval.et > hour * 60 && interval.selected
      );
    },
    startDragging(day, hour) {
      this.isDragging = true;
      this.toggleInterval(day, hour);
    },
    dragging(day, hour) {
      if (this.isDragging) {
        this.toggleInterval(day, hour);
      }
    },
    stopDragging() {
      this.isDragging = false;
    },
    toggleInterval(day, hour) {
      const interval = { bt: hour * 60, et: (hour + 1) * 60 };
      const existing = this.selectedIntervals[day].find(i => i.bt === interval.bt);
      
      if (existing) {
        existing.selected = !existing.selected;
      } else {
        this.selectedIntervals[day].push({ ...interval, selected: true });
      }
    },
    toggleAllDay(day) {
  const allSelected = this.selectedIntervals[day].every(i => i.selected);
  
  // Устанавливаем состояние для всех часов в день
  for (let hour = 0; hour < 25; hour++) {
    const interval = { bt: hour * 60, et: (hour + 1) * 60 };
    const existing = this.selectedIntervals[day].find(i => i.bt === interval.bt);
    
    if (existing) {
      existing.selected = !allSelected; // Меняем состояние на противоположное
    } else {
      this.selectedIntervals[day].push({ ...interval, selected: !allSelected });
    }
  }
}
  }
};
</script>

<style scoped>
.weekly-calendar {
  display: flex;
}
.time-column {
  display: flex;
  flex-direction: column;
}
.time-slot {
  height: 20px;
  border-bottom: 1px solid #eee;
}
.day-columns {
  display: flex;
}
.day-column {
  border-right: 1px solid #ccc;
}
.day-header {
  background-color: #f0f0f0;
}
.time-slot.selected {
  background-color: #007bff; /* Темный цвет для выбранных интервалов */
}
.time-slot:not(.selected) {
  background-color: #e0e0e0; /* Светлый цвет для невыбранных интервалов */
}

.time-column {
  margin-right: 10px; /* Отступ между колонкой времени и таблицей */
}

.time-slot {
  height: 20px;
  border-bottom: 1px solid #eee;
  position: relative;
  top: 5px;
}


.time-column .time-slot {
  position: relative; /* Относительное позиционирование только для времени */
  top: 29px; /* Смещаем текст времени вниз */
}

button {
  margin-top: 10px; /* Отступ сверху для кнопки "Весь день" */
  padding: 5px;
}


</style>