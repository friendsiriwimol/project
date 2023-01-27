<template>
  <div>
    <NavbarUser/>
    <div>
    <v-breadcrumbs
      :items="breadcrumbs"
      large
    ></v-breadcrumbs>
  </div>
    <!-- <div>{{item.lesson_unit}}</div>
    <div>{{item.lesson_name}}</div>
    <div>{{item}}</div> -->
    <v-card class="mt-7 mb-7 pa-5">
      <h1 class="text-center ma-5">บทที่ {{ lesson_unit }} {{lesson_unit}} <span class="teal--text">{{lesson_name}}</span></h1>
    <div class="output ql-snow">
      <div class="ql-editor" v-html="lesson_description"></div>
    </div>
    <v-card  elevation="0" style="border-color:#fcad74; border-width: 2px;" rounded outlined >
      <v-card-title>
        เว็บไซต์ที่เกี่ยวข้อง
      </v-card-title>
      <v-card-subtitle>
        <div  v-for="website in allwebsite" v-bind:key="website.website_id">
      <ul v-if="website.lesson_id === item.lesson_id">
        <a :href="website.website_link" target="_blank"><li>{{ website.website_link }}</li></a>
        <!-- <li>{{ website.website_link }}</li> -->
      </ul>
    </div>
    </v-card-subtitle>

    </v-card>
</v-card>
  </div>
</template>

<script>
import axios from 'axios'
import NavbarUser from '@/components/NavbarUser'
// import { all } from 'q'
export default {
  components: {
    NavbarUser
  },
  name: 'quill-example-custom-toolbar',
  title: 'Custom toolbar',
  data () {
    return {
      alllesson: [],
      allwebsite: [],
      website: {
        website_id: ''
      },
      item: '',
      lesson: {
        lesson_id: '',
        lesson_unit: '',
        lesson_name: '',
        lesson_description: ''
      },
      lesson_id: '',
      lesson_unit: '',
      lesson_name: '',
      lesson_description: '',
      breadcrumbs: [
        {
          text: 'หน้าแรก',
          disabled: false,
          to: { name: 'home' }
        },
        {
          text: 'บทเรียน',
          disabled: false,
          // href: 'lesson'
          to: { name: 'lesson' }
        },
        {
          text: 'รายละเอียดบทเรียน',
          disabled: true
          // href: 'lessondetail'
        }
      ]
    }
  },
  created () {
    this.lesson_unit = this.$route.params.id
    this.getLesson()
    this.getWebsite()
  },
  inject: [
    'getLesson'
  ],
  computed: {
    lessonChilren () {
      return this.getLesson
    }
  },
  methods: {
    async getLesson () {
      axios.get('http://localhost/vue-backend/lesson.php?id=this.lesson_unit').then((res) => {
        console.log('บทเรียน:', res.data)
        if (res.data) {
          this.item = res.data[this.lesson_unit]
          console.log('item', this.item)
        }
      })
    },
    async getWebsite () {
      axios.get('http://localhost/vue-backend/website.php').then((res) => {
        console.log('data:', res.data)
        if (res.data) {
          this.allwebsite = res.data
          console.log(this.allwebsite, 'เว็บไซต์')
        }
      })
    }
  }
}
</script>

<style scoped>

.v-breadcrumbs >>> a {
    color: #fcad74 !important;
}
</style>
