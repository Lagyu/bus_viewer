<template>
  <v-layout
    column
    justify-center
    align-center
  >
    <v-flex
      xs12
      sm8
      md6
      lg6
      xl6
    >
      <v-card>
        <v-card-title class="headline">
          バス待機列人数カウント
        </v-card-title>
        <v-card-actions>
          <v-spacer />
          <v-layout row wrap justify-center="">
            <v-flex v-for="bus_count in bus_counts" style="margin: 0.25em">
              <v-btn
                v-bind:color="bus_count.unique_rider_count_for_the_next_bus.color"
                nuxt
                to=""
                height="auto"
                width="100%"
              >
                <v-layout column>
                  <span class="display-1">
                    {{ bus_count.from_office_brunch.name }}
                    →
                    {{ bus_count.to_office_brunch.name }}
                    :
                    {{ bus_count.unique_rider_count_for_the_next_bus.count }}/9人
                  </span>
                  <span class="title">
                    発車時刻：{{ bus_count.next_close_count_datetime_for_today ? ('0' + new Date(bus_count.next_close_count_datetime_for_today).getHours()).slice(-2) + ':' + ('0' + new Date(bus_count.next_close_count_datetime_for_today).getMinutes()).slice(-2) : 'データなし' }}<br>
                  </span>
                </v-layout>
              </v-btn>
            </v-flex>
          </v-layout>
        </v-card-actions>
      </v-card>
    </v-flex>
  </v-layout>
</template>

<script>

// import axios from '../.nuxt/axios';

import axios from 'axios'

export default {
  data () {
    return {
      bus_counts: [
        {
          from_office_brunch: {
            id: 1,
            name: 'サンプル'
          },
          to_office_brunch: {
            id: 2,
            name: 'です'
          },
          'next_close_count_datetime_for_today': '2019-11-27T10:00:00+09:00',
          unique_rider_count_for_the_next_bus: {
            count: 2,
            color: '#ACE163'
          }
        },
        {
          from_office_brunch: {
            id: 2,
            name: 'さん'
          },
          to_office_brunch: {
            id: 1,
            name: 'ぷる'
          },
          'next_close_count_datetime_for_today': '2019-11-27T10:00:00+09:00',
          unique_rider_count_for_the_next_bus: {
            count: 8,
            color: '#E4540E'
          }
        }
      ]
    }
  },
  created () {
    const updateInfo = () => {
      axios.get('/api/bus_route/?format=json').then((res) => {
        this.bus_counts = res.data
      })
      setTimeout(updateInfo, 4000)
    }
    updateInfo()
  }
}

</script>
