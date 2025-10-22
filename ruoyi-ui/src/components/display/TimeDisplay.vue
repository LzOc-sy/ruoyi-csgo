<template>
  <div class="time-display" :class="[size, layout]">
    <div class="time-main">
      <i class="el-icon-date">
        <span class="hours">{{ formattedHours }}</span>:<span class="minutes">{{ formattedMinutes }}</span>
        <span v-if="showSeconds" class="seconds">:{{ formattedSeconds }}</span>
        <span v-if="use12Hour" class="ampm">{{ ampm }}</span>
      </i>
    </div>
    <div v-if="showDate" class="date-display">{{ formattedDate }}</div>
    <div v-if="showDay" class="day-display">{{ formattedDay }}</div>
    <div class="control-panel">
      <el-checkbox
        v-model="isChecked"
        @change="handleCheckboxChange"
      >
        {{ checkboxLabel }}
      </el-checkbox>
      <el-date-picker
        v-model="todoTime"
        type="date"
        placeholder="选择日期"
        format="yyyy-MM-dd"
        value-format="yyyy-MM-dd"
        clearable
        @change="handleDateChange"
      >
      </el-date-picker>
    </div>
  </div>
</template>

<script>
export default {
  name: 'TimeDisplay',
  props: {
    showSeconds: {
      type: Boolean,
      default: true
    },
    showDate: {
      type: Boolean,
      default: true
    },
    showDay: {
      type: Boolean,
      default: true
    },
    use12Hour: {
      type: Boolean,
      default: false
    },
    size: {
      type: String,
      default: 'medium',
      validator: value => ['small', 'medium', 'large'].includes(value)
    },
    layout: {
      type: String,
      default: 'standard',
      validator: value => ['standard', 'compact', 'inline'].includes(value)
    },
    color: {
      type: String,
      default: '#409EFF'
    },
    checkboxLabel: {
      type: String,
      default: '启用'
    },
    defaultChecked: {
      type: Boolean,
      default: false
    },
    defaultDate: {
      type: [String, Date],
      default: ''
    }
  },
  data() {
    return {
      now: new Date(),
      isChecked: this.defaultChecked,
      todoTime: this.formatDate(this.defaultDate)
    }
  },
  computed: {
    formattedHours() {
      let hours = this.now.getHours();
      if (this.use12Hour) {
        hours = hours % 12 || 12;
      }
      return hours.toString().padStart(2, '0');
    },
    formattedMinutes() {
      return this.now.getMinutes().toString().padStart(2, '0');
    },
    formattedSeconds() {
      return this.now.getSeconds().toString().padStart(2, '0');
    },
    ampm() {
      return this.now.getHours() >= 12 ? 'PM' : 'AM';
    },
    formattedDate() {
      const year = this.now.getFullYear();
      const month = this.now.getMonth() + 1;
      const day = this.now.getDate();
      return `${year}年${month}月${day}日`;
    },
    formattedDay() {
      const weekdays = ['星期日', '星期一', '星期二', '星期三', '星期四', '星期五', '星期六'];
      return weekdays[this.now.getDay()];
    }
  },
  watch: {
    defaultChecked(newVal) {
      this.isChecked = newVal;
      this.emitAllData();
    },
    defaultDate(newVal) {
      this.todoTime = this.formatDate(newVal);
      this.emitAllData();
    }
  },
  mounted() {
    this.timer = setInterval(() => {
      this.now = new Date();
    }, 1000);
    this.emitAllData();
  },
  beforeDestroy() {
    if (this.timer) {
      clearInterval(this.timer);
    }
  },
  methods: {
    handleCheckboxChange(value) {
      this.$emit('checkbox-change', value);
      this.emitAllData();
    },
    handleDateChange(value) {
      const formattedDate = this.formatDate(value);
      this.$emit('date-change', formattedDate);
      this.emitAllData();
    },
    emitAllData() {
      const formattedDate = this.formatDate(this.todoTime);
      this.$emit('data-change', {
        isChecked: this.isChecked,
        date: formattedDate
      });
    },
    formatDate(date) {
      if (!date) return '';
      if (typeof date === 'string') {
        const parts = date.split('-');
        if (parts.length === 3) {
          const [year, month, day] = parts;
          return `${year.padStart(4, '0')}-${month.padStart(2, '0')}-${day.padStart(2, '0')}`;
        }
        const timestamp = Date.parse(date);
        if (!Number.isNaN(timestamp)) {
          return this.formatDate(new Date(timestamp));
        }
        return '';
      }
      const d = new Date(date);
      if (Number.isNaN(d.getTime())) {
        return '';
      }
      const year = d.getFullYear();
      const month = String(d.getMonth() + 1).padStart(2, '0');
      const day = String(d.getDate()).padStart(2, '0');
      return `${year}-${month}-${day}`;
    }
  }
}
</script>

<style scoped>
.time-display {
  display: flex;

  padding: 20px;
  background: white;
  border-radius: 12px;
  box-shadow: 0 4px 20px rgba(0, 0, 0, 0.08);
  transition: all 0.3s ease;
  min-width: 280px;
}

.control-panel {
  display: flex;
  flex-direction: row;
  align-items: center;
  margin-left: auto;
  gap: 12px;
}

.time-display:hover {
  //transform: translateY(-5px);
  //box-shadow: 0 6px 25px rgba(0, 0, 0, 0.12);
}

.time-main {
  display: flex;
  flex-direction: column;
  align-items: baseline;
  margin-bottom: 8px;
  color: v-bind(color);
}

.hours, .minutes {
  font-weight: 300;
}

.seconds {
  opacity: 0.8;
}

.ampm {
  font-size: 0.8em;
  margin-left: 8px;
  opacity: 0.7;
}

.date-display {
  font-size: 1.1em;
  color: #606266;
  margin-bottom: 5px;
}

.day-display {
  font-size: 1em;
  color: #909399;
}

.time-display.small .time-main {
  font-size: 2rem;
}

.time-display.medium .time-main {
  font-size: 3.2rem;
}

.time-display.large .time-main {
  font-size: 4rem;
}

.time-display.small .date-display {
  font-size: 0.9em;
}

.time-display.small .day-display {
  font-size: 0.8em;
}

.time-display.compact {
  flex-direction: column;
  align-items: center;
  justify-content: center;
  gap: 15px;
  min-width: auto;
  padding: 15px 25px;
}

.time-display.compact .time-main {
  margin-bottom: 0;
  font-size: 1.8rem;
}

.time-display.compact .date-display {
  margin-bottom: 0;
  font-size: 1em;
}

.time-display.compact .day-display {
  display: none;
}

.time-display.inline {
  flex-direction: column;
  align-items: center;
  justify-content: flex-start;
  gap: 10px;
  min-width: auto;
  padding: 10px 15px;
  background: transparent;
  box-shadow: none;
}

.time-display.inline:hover {
  transform: none;
  box-shadow: none;
}

.time-display.inline .time-main {
  margin-bottom: 0;
  font-size: 1.2rem;
}

.time-display.inline .date-display,
.time-display.inline .day-display {
  font-size: 0.9em;
  margin-bottom: 0;
}
</style>
