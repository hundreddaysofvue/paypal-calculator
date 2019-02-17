<template>
  <div>
    <h2>{{ title }}</h2>
    <article class="flex-container">
      <div>
        <money
          v-model="percentage"
          v-bind="{ suffix: ' %', precision: 2, decimal: '.', masked: false }"
          class="input-field-calc"
          size="6"
        />
      </div>
      <span class="tag-calc">plus</span>
      <div>
        <money
          v-model="fixed"
          v-bind="moneyConf"
          class="input-field-calc"
          size="6"
        />
      </div>
    </article>
    <article class="flex-container flex-columns">
      <div class="input-container">
        <label for="send">{{ messageSend }}</label>
        <money
          v-model="send"
          v-bind="moneyConf"
          class="input-field-pay"
        />
      </div>
      <div
        data-tippy="Click me if you want to change the operation"
        class="icon"
        @click="changeOperation"
      >
        <img
          src="../assets/send-receive-icon.svg"
          alt="send-receive-money"
        >
      </div>
      <div class="input-container">
        <label for="receive">{{ messageRecv }}</label>
        <money
          v-model="recv"
          v-bind="moneyConf"
          class="input-field-pay"
        />
      </div><br>
      <div class="input-container">
        <money
          v-model="fee"
          v-bind="moneyConf"
          class="input-field-pay"
        />
        <label for="fee">PayPal commission</label>
      </div>
    </article>
  </div>
</template>

<script>
import { Money } from 'v-money'
import Big from 'big.js'

Big.DP = 2 // Decimal places for all calculations
Big.RM = 2 // Round method for all calculations

export default {
  name: 'Calculator',
  components: { Money },
  data () {
    return {
      title: 'PayPal Commissions',
      isSending: true,
      send: 0,
      percentage: 5.4,
      fixed: 0.3,
      moneyConf: {
        decimal: '.',
        thousands: ',',
        prefix: '$ ',
        precision: 2,
        masked: false
      }
    }
  },
  computed: {
    messageSend () {
      return this.isSending ? 'If you send' : 'To receive'
    },
    messageRecv () {
      return this.isSending ? 'They will receive' : 'They need to send'
    },
    recv: {
      get () {
        let send = Big(this.send)
        let fee = Big(this.fee)
        return parseFloat(
          this.isSending
            ? send.gt(fee)
              ? send.minus(fee)
              : 0
            : send.plus(fee))
      },
      set (v) {} // Computed property setter necessary
    },
    fee: {
      get () {
        let send = Big(this.send)
        let fixed = Big(this.fixed)
        let percentage = Big(this.isSending ? this.percentage : 5.8)
        let adjust = this.isSending
          ? 0
          : send.lt(5)
            ? Big(-0.02)
            : send.lt(15)
              ? Big(-0.01)
              : Big(-0.01).times([...this.send.toString()][0] - 1)
        let total = this.send
          ? parseFloat(
            send.times(percentage)
              .div(100)
              .plus(fixed)
              .plus(adjust)
              .plus(!this.isSending && send.lt(16) ? 0.01 : 0))
          : 0
        return total
      },
      set (v) {} // Computed property setter necessary
    }
  },
  methods: {
    changeOperation () {
      this.isSending = !this.isSending
      this.send = 0
    }
  }
}
</script>

<style scoped>
label {
  display: block;
  margin: .5rem 0;
}
img {
  width: 128px;
  height: 128px;
}
.flex-container {
  display: flex;
  align-items: center;
  justify-content: center;
  text-align: center;
  margin: 1rem 0;
}
.flex-columns {
  flex-direction: column;
}
.tag-calc {
  display: block;
  margin: 0 1rem;
}
.input-container {
  width: 75%;
}
.input-field-calc, .input-field-pay {
  background-color: #F0F0F0;
  outline: none;
  border: 0;
  border-radius: .5rem;
  text-align: center;
}
.input-field-calc {
  padding: .5rem 0;
}
.input-field-pay {
  width: 100%;
  line-height: 2.5rem;
  font-size: 1.25rem;
}
</style>
