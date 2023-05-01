<template>
  <div>
    <v-container>
      <v-card class="cardShowuser mt-10">
    <v-card-title>
      <v-icon class="mr-2" color="#fcad74">mdi-podium-gold</v-icon>
      จัดลำดับคะเเนนสูงสุด
      <v-spacer></v-spacer>
      <v-text-field
        v-model="search"
        append-icon="mdi-magnify"
        label="ค้นหา"
        filled
          rounded
          dense
        color="#099fae"
        single-line
        hide-details
      ></v-text-field>
    </v-card-title>

    <!-- <v-card-title>friend</v-card-title> -->
       <v-data-table
       :footer-props="{itemsPerPageText: 'แถวต่อหน้า',pageText: '{0}-{1} จาก {2}','items-per-page-all-text': 'ทั้งหมด'}"
       :items="allscore"
       :headers="headers"
       :items-per-page="5"
       :search="search"
       no-data-text="ไม่พบข้อมูล"
        no-results-text="ไม่พบข้อมูลที่ค้นหา">
       <template slot="data">
        <td>{{lesson_id}}</td>
        <!-- <td>{{website_id}}</td> -->
        <td>{{title}}</td>
        <td>{{student}}</td>
      </template>
      <template v-slot:item.second_score="{ item }">
      <v-chip
        :color="getColor(item.second_score)"
        dark
      >
        {{ item.second_score }}
      </v-chip>
    </template>
    </v-data-table>
                </v-card>
              </v-container>
  </div>
</template>

<script>
import axios from 'axios'

export default {
  data: () => ({
    user_firstname: '',
    user_lastname: '',
    alllesson: [],
    allscore: [],
    lesson_id: '',
    lesson_unit: '',
    lesson_name: '',
    lesson_description: '',
    lesson: {
      lesson_unit: '',
      lesson_name: '',
      lesson_description: ''
    },
    search: '',
    items: ['ทั้งหมด', '1', '2', '3'],
    headers: [
      {
        text: 'บทที่',
        align: 'start',
        value: 'lesson_id'
      },
      { text: 'ชื่อบทเรียน', value: 'title' },
      // { text: 'เนื้อหาบทเรียน', value: 'lesson_description' },
      // { text: 'เวลาที่สอบ', value: 'time', sortable: false },
      // { text: 'วันที่ทำข้อสอบ', value: 'date', sortable: false },
      { text: 'ชื่อผู้ใช้งาน', value: 'student', sortable: false },
      { text: 'คะแนน', value: 'second_score', sortable: false }
      // { text: 'posttest', value: 'posttest', sortable: false }

    ]
  }),
  created () {
    this.getScore()
  },
  methods: {
    getColor (score) {
      if (score > 1) return 'orange'
      else if (score > 5) return 'green'
      else return 'red'
    },
    async getScore () {
      console.log('rewload')
      axios.get('http://localhost/vue-backend/score.php').then((res) => {
        console.log('data:', res.data)
        if (res.data) {
          this.allscore = res.data
        }
      })
    }
  }
}
</script>

<style>

</style>
