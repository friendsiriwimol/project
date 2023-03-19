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
      <v-card-title class="justify-center">บทที่ {{ lesson_id}}</v-card-title>
        <v-row>
          <v-col cols="12" sm="12" v-for="video in allvideo" v-bind:key="video.video_id">
                    <v-card class="pa-12">
                      <div>
                        <iframe :src="video.video_file" width="100%" height="600px"></iframe>

                      <!-- <iframe width="100%" src="https://www.youtube.com/embed/GTcM3qCeup0" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe> -->

                      <div class="mt-2 mb-2">{{video.video_subunit}}
                        <router-link v-bind:to="'/videodetail/'">{{video.video_name}}</router-link></div>
                    </div>
                    </v-card>
          </v-col>
        </v-row>
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
    allvideo: [],
    lesson_id: localStorage.getItem('vdo_lession_id'),
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
      var url = 'http://localhost/vue-backend/video.php?lesson_id=' + this.lesson_id
      // alert(url)
      // alert('vdo')
      axios.get(url).then((res1) => {
        console.log('data:', res1.data)
        if (res1.data) {
          this.allvideo = res1.data
          // alert(res1.data.video_id)
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
