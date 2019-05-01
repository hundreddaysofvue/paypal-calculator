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
      </div>
    </article>
  </div>
</template>

<script>
import { Money } from 'v-money'
import Big from 'big.js'

Big.DP = 3 // Decimal places for all calculations
Big.RM = 0 // No rounding, truncate decimals for all calculations

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
    percentageRate () {
      return parseFloat(Big(this.percentage).div(100))
    },
    messageSend () {
      return this.isSending ? 'If you send' : 'To receive'
    },
    messageRecv () {
      return this.isSending ? 'They will receive' : 'They need to send'
    },
    recv: {
      get () {
        let send = Big(this.send)
        let fixed = Big(this.fixed)
        let rate = Big(this.percentageRate)
        return parseFloat(
          this.isSending
            ? send.minus(this.fee > 0 ? this.fee : 0)
            : send.plus(fixed).div(Big(1).minus(rate))
        )
      },
      set (v) {} // Computed property setter necessary
    },
    fee: {
      get () {
        let send = Big(this.send)
        let fixed = Big(this.fixed)
        let rate = Big(this.percentageRate)
        return parseFloat(
          this.send
            ? send.times(rate).plus(fixed)
            : 0
        )
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
  color: #0044aa;
  outline: none;
  border: 0;
  border-radius: .5rem;
  margin: .5rem 0;
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
