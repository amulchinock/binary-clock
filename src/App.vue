<script setup>
import { getHours, getMinutes, getSeconds } from 'date-fns';
import indicator from './components/indicator.vue';
</script>

<template>
  <div id="clock">
    <div class="row">
      <div class="col border-right">
        <div class="row">
          <h2 class="col">Hours <small>{{this.hours.tens}} {{this.hours.units}}</small></h2>
        </div>

        <div class="row">
          <div class="col tens">
            <indicator v-for="octet in octets.hours.t" :key="`hours_t_${octet.name}`" :state="octet.active" />
          </div>

          <div class="col units">
            <indicator v-for="octet in octets.hours.u" :key="`hours_u_${octet.name}`" :state="octet.active" />
          </div>
        </div>
      </div>

      <div class="col border-right">
        <div class="row">
          <h2 class="col">Minutes <small>{{this.minutes.tens}} {{this.minutes.units}}</small></h2>
        </div>

        <div class="row">
          <div class="col tens">
            <indicator v-for="octet in octets.minutes.t" :key="`minutes_t_${octet.name}`" :state="octet.active" />
          </div>

          <div class="col units">
            <indicator v-for="octet in octets.minutes.u" :key="`minutes_u_${octet.name}`" :state="octet.active" />
          </div>
        </div>
      </div>

      <div class="col">
        <div class="row">
          <h2 class="col">Seconds <small>{{this.seconds.tens}} {{this.seconds.units}}</small></h2>
        </div>

        <div class="row">
          <div class="col tens">
            <indicator v-for="octet in octets.seconds.t" :key="`seconds_t_${octet.name}`" :state="octet.active" />
          </div>

          <div class="col units">
            <indicator v-for="octet in octets.seconds.u" :key="`seconds_u_${octet.name}`" :state="octet.active" />
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  mounted () {
    setInterval(() => this.dateTime = new Date(), 250);
  },

  methods: {
    splitUnits (number) {
      const numbs = Array.from(String(number), Number);
      const unitNames = ['thousands', 'hundreds', 'tens', 'units'];
      const offset = unitNames.length - numbs.length;

      const split = {};

      numbs.forEach((number, index) => split[unitNames[offset + index]] = number);

      return split;
    },

    octetStatus (value, octetLevel) {
      const ots = {
        8: [ 8, 9, 10, 11, 12, 13, 14, 15 ],
        4: [ 4, 5, 6, 7, 12, 13, 15 ],
        2: [ 2, 3, 6, 7, 10, 11, 14, 15 ],
        1: [ 1, 3, 5, 7, 9, 11, 13, 15 ]
      };

      return ots[octetLevel].includes(value);
    }
  },

  computed: {
    octets () {
      return {
        seconds: {
          t: [
            {
              name: 'octet_4',
              active: this.octetStatus(this.seconds.tens, 4)
            },
            {
              name: 'octet_2',
              active: this.octetStatus(this.seconds.tens, 2)
            },
            {
              name: 'octet_1',
              active: this.octetStatus(this.seconds.tens, 1)
            }
          ],
          u: [
            {
              name: 'octet_8',
              active: this.octetStatus(this.seconds.units, 8)
            },
            {
              name: 'octet_4',
              active: this.octetStatus(this.seconds.units, 4)
            },
            {
              name: 'octet_2',
              active: this.octetStatus(this.seconds.units, 2)
            },
            {
              name: 'octet_1',
              active: this.octetStatus(this.seconds.units, 1)
            }
          ]
        },
        minutes: {
          t: [
            {
              name: 'octet_4',
              active: this.octetStatus(this.minutes.tens, 4)
            },
            {
              name: 'octet_2',
              active: this.octetStatus(this.minutes.tens, 2)
            },
            {
              name: 'octet_1',
              active: this.octetStatus(this.minutes.tens, 1)
            }
          ],
          u: [
            {
              name: 'octet_8',
              active: this.octetStatus(this.minutes.units, 8)
            },
            {
              name: 'octet_4',
              active: this.octetStatus(this.minutes.units, 4)
            },
            {
              name: 'octet_2',
              active: this.octetStatus(this.minutes.units, 2)
            },
            {
              name: 'octet_1',
              active: this.octetStatus(this.minutes.units, 1)
            }
          ]
        },
        hours: {
          t: [
            {
              name: 'octet_2',
              active: this.octetStatus(this.hours.tens, 2)
            },
            {
              name: 'octet_1',
              active: this.octetStatus(this.hours.tens, 1)
            }
          ],
          u: [
            {
              name: 'octet_8',
              active: this.octetStatus(this.hours.units, 8)
            },
            {
              name: 'octet_4',
              active: this.octetStatus(this.hours.units, 4)
            },
            {
              name: 'octet_2',
              active: this.octetStatus(this.hours.units, 2)
            },
            {
              name: 'octet_1',
              active: this.octetStatus(this.hours.units, 1)
            }
          ]
        }
      };
    },

    hours () {
      return this.splitUnits(getHours(this.dateTime));
    },

    minutes () {
      return this.splitUnits(getMinutes(this.dateTime));
    },

    seconds () {
      return this.splitUnits(getSeconds(this.dateTime));
    }
  },

  data () {
    return {
      dateTime: null
    }
  }
}
</script>

<style lang="scss">
$gray: #2c3e50;

#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: $gray;
  margin-top: 60px;
  display: flex;
  align-items: center;
  justify-content: center;

  #clock {
    border: 1px solid $gray;
    width: 1000px;
    border-radius: 5px;
    padding: 15px 0;

    h2 {
      text-align: center;
      margin-top: 0;
    }

    .row {
      display: flex;
      flex-direction: row;
    }

    .col {
      display: flex;
      flex-direction: column;
      flex-grow: 1;
      justify-content: end;

      &.border-right {
        border-right: 1px solid $gray;
      }
    }
  }
}
</style>
