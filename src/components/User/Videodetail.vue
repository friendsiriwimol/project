<template>
  <div>
    <NavbarUser />
    <div>
      <v-breadcrumbs :items="breadcrumbs" large></v-breadcrumbs>
    </div>
    <v-card class="cardShowuser mt-0 ">
        <center>
      <!-- <v-card-title>
        <v-icon class="mr-2" color="#fcad74">mdi-video</v-icon>รายละเอียดวิดีโอ
        <v-spacer></v-spacer>
        <v-text-field
          v-model="search"
          append-icon="mdi-magnify"
          label="ค้นหา"
          dense
          color="#099fae"
          single-line
          hide-details
        ></v-text-field>

      </v-card-title> -->
      <v-card-title class="justify-center">บทที่ 1</v-card-title>
      <v-card-text>1.1</v-card-text>
        <iframe width="560" height="315" src="https://www.youtube.com/embed/gQlMMD8auMs" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" allowfullscreen></iframe>
    </center>
    </v-card>
  </div>
</template>
<script>
import axios from 'axios'
import NavbarUser from '@/components/NavbarUser'
export default {
  components: {
    NavbarUser
  },
  data: () => ({
    reveal: false,
    alllesson: [],
    lesson_id: '',
    lesson_unit: '',
    lesson_name: '',
    lesson_description: '',
    lesson: {
      lesson_unit: '',
      lesson_name: '',
      lesson_description: ''
    },
    headers: [
      {
        text: 'บทที่',
        align: 'start',
        value: 'lesson_id'
      },
      { text: 'บทเรียนเรื่อง', value: 'lesson_name' },
      // { text: 'เนื้อหาบทเรียน', value: 'lesson_description' },
      { text: 'ดูวิดีโอ', value: 'edit', sortable: false }
    ],
    breadcrumbs: [
      {
        text: 'หน้าแรก',
        disabled: false,
        href: 'home'
      },
      {
        text: 'วิดีโอ',
        disabled: true,
        href: 'video'
      }
    ],
    dialog: false
  }),
  created () {
    this.getLesson()
    this.getVideo()
  },
  methods: {
    async getVideo () {
      console.log('rewload')
      axios.get('http://localhost/vue-backend/video.php').then(res => {
        console.log('data:', res.data)
        if (res.data) {
          this.allvideo = res.data
        }
      })
    },
    async getLesson () {
      axios.get('http://localhost/vue-backend/editLesson.php').then(res => {
        console.log('data:', res.data)
        if (res.data) {
          this.alllesson = res.data
        }
      })
    },
    openVideo (item) {
      this.dialog = true
    },
    deleteItem (item) {
      this.dialog = false
    }
  }
  // mounted () {
  //   this.$emit('test', true)
  // }
}
</script>

  <style scoped>
@import url("https://fonts.googleapis.com/css2?family=Prompt&display=swap");
* {
  font-family: "Prompt", sans-serif;
}

.text-h4 {
  text-align: center;
  font-size: 20px;
}
h1 {
  color: #000;
}
.img1 {
  width: 500px;
  height: 100px;
  text-align: center;
}
.mx {
  height: 400px;
  padding: 20px;
  margin-left: auto;
  margin-right: auto;
}
.text-center {
  font-size: 25px;
  font-style: bold;
  padding: 15px;
  color: #56a062;
  text-align: center;
}
.text-title {
  text-align: center;
  padding: 15px;
  font-size: 1.3em !important;
  color: rgb(75, 75, 75);
}
a {
  text-decoration: none;
  text-align: center;
  color: #56a062;
}
.lesson {
  text-align: center;
  margin-top: 30px;
}

.v-card {
  transition: opacity 0.5s ease-in-out rgb(255, 255, 107);
  box-shadow: 1px 1px 9px 9px lightblue;
}

.show-btns {
  color: #feedb0 !important;
  box-shadow: 10px 10px 5px 12px lightblue;
}
.v-breadcrumbs >>> a {
  color: #fcad74;
}
</style>
