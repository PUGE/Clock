<template>
  <div class="clock" :style="getStyle()">
    <div class="date">{{date}}</div>
    <div class="hours">
      <div class="first">
        <div class="number">0</div>
      </div>
      <div class="second">
        <div class="number">0</div>
      </div>
    </div>
    <div class="tick">:</div>
    <div class="minutes">
      <div class="first">
        <div class="number">0</div>
      </div>
      <div class="second">
        <div class="number">0</div>
      </div>
    </div>
    <div class="tick">:</div>
    <div class="seconds">
      <div class="first">
        <div class="number">0</div>
      </div>
      <div class="second infinite">
        <div class="number">0</div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  data () {
    return {
      date: '',
      clock: null
    }
  },
  props: {
    fontSize: {
      type: Number,
      default: 18
    },
    color: {
      type: String,
      default: '#FFFFFF'
    },
    allDate: {
      type: Boolean,
      default: false
    }
  },
  methods: {
    getStyle () {
      return {
        'font-size': this.fontSize + 'px',
        'height': this.fontSize + 2 + 'px',
        'line-height': this.fontSize + 2 + 'px',
        'color': this.color
      }
    }
  },
  mounted () {
    // 获取当前日期
    if (this.allDate) {
      const nowDate = new Date()
      this.date = `${nowDate.getFullYear()}-${nowDate.getMonth()}-${nowDate.getDate()}`
    }
    const hoursContainer = this.$el.childNodes[2]
    const minutesContainer = this.$el.childNodes[6]
    const secondsContainer = this.$el.childNodes[10]
    let last = new Date(0)
    last.setUTCHours(-1)

    function updateTime () {
      const now = new Date()
      const lastHours = last.getHours().toString()
      const nowHours = now.getHours().toString()
      if (lastHours !== nowHours) {
        updateContainer(hoursContainer, nowHours)
      }
      const lastMinutes = last.getMinutes().toString()
      const nowMinutes = now.getMinutes().toString()
      if (lastMinutes !== nowMinutes) {
        updateContainer(minutesContainer, nowMinutes)
      }
      const lastSeconds = last.getSeconds().toString()
      const nowSeconds = now.getSeconds().toString()
      if (lastSeconds !== nowSeconds) {
        updateContainer(secondsContainer, nowSeconds)
      }
      last = now
    }

    function updateContainer (container, newTime) {
      const time = newTime.split('')
      if (time.length === 1) {
        time.unshift('0')
      }
      const first = container.firstElementChild
      if (first.lastElementChild.textContent !== time[0]) {
        updateNumber(first, time[0])
      }
      const last = container.lastElementChild
      if (last.lastElementChild.textContent !== time[1]) {
        updateNumber(last, time[1])
      }
    }

    function updateNumber (element, number) {
      let second = element.lastElementChild.cloneNode(true)
      second.textContent = number
      element.appendChild(second)
      element.classList.add('move')
      setTimeout(function () {
        element.classList.remove('move')
      }, 900)
      setTimeout(function () {
        element.removeChild(element.firstElementChild)
      }, 900)
    }

    this.clock = setInterval(updateTime, 1000)
  },
  beforeDestroy: function () {
    // 清除定时器
    window.clearInterval(this.clock)
  }
}
</script>

<style scoped>
  .clock {
    color: white;
    font-family: sans-serif;
    display: flex;
    position: relative;
    justify-content: center;
    overflow: hidden;
  }
  .date {
    margin-right: 20px;
  }
  .clock > div {
    display: flex;
  }

  .tick-hidden {
    opacity: 0;
  }

  .move {
    animation: move linear 1s infinite;
  }

  @keyframes move {
    from {
      transform: translateY(0);
    }
    to {
      transform: translateY(-100%);
    }
  }
</style>
