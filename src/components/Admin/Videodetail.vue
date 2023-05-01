<template>
  <div>
    <NavbarAdmin />
    <div>
      <v-breadcrumbs :items="breadcrumbs" large></v-breadcrumbs>
    </div>
    <v-card class="cardShowuser mt-0">

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

        </v-card-title>-->
        <v-card-title>บทที่ {{ lesson_id}} <span class="teal--text">&nbsp;{{ lesson_name }}</span>
          <v-spacer></v-spacer>
                <v-tooltip bottom>
          <template v-slot:activator="{ on, attrs }">
          <v-btn class="mx-2"
          fab
          dark
          small
          color="#099fae"
          @click="OpenDialog()"
                    v-bind="attrs"
          v-on="on"
>
                <v-icon dark>mdi-plus</v-icon>
              </v-btn>
      </template>
        <span>เพิ่มวิดีโอ</span>
      </v-tooltip>
        </v-card-title>
        <v-divider></v-divider>
        <v-container>
          <center>
              <div class="mt-5">
                <v-row>
          <v-col cols="12" xs="12" sm="12"
                md="6" v-for="video in allvideo" v-bind:key="video.video_id">
                <v-card outlined style="border: 1px solid #fcad74;">
            <video autoplay controls class="mt-5" width="90%">
  <source :src="video.video_file" />
</video>

                <!-- <video autoplay controls :src="video.video_file">
  The “video” tag is not supported by your browser.
</video> -->
                <!-- <iframe :src="video.video_file" width="100%" height="600px"></iframe> -->

                <!-- <iframe width="100%" src="https://www.youtube.com/embed/GTcM3qCeup0" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe> -->
                <v-divider class="mt-5" color="#fcad74"></v-divider>

<v-card-actions class="ml-5 mr-5">
  {{video.video_subunit}} {{video.video_name}}
  <v-spacer></v-spacer>

              <v-btn icon>
                <v-icon color="#56a062" @click="editItem(video)">mdi-pencil</v-icon>
              </v-btn>
              <v-btn icon>
                <v-icon color="primary" @click="editVideo(video)">mdi-video</v-icon>
              </v-btn>
              <v-btn icon>
                <v-icon color="red" @click="deleteItem(video)">mdi-delete</v-icon>
              </v-btn>
</v-card-actions>
              </v-card>
                </v-col>
          </v-row>
              </div>
              <div class="ma-10">{{ video.text }}</div>
              </center>
            </v-container>
    </v-card>
    <v-dialog v-model="dialog" max-width="600px" persistent>
                      <v-card>
          <v-card-title> แก้ไขวิดีโอ </v-card-title>
          <v-card-text>
            <v-container>
              <v-row>
                <v-col cols="12">
                  <v-text-field
                    label="บทที่"
                    required
                    v-model="lesson_id"
                    disabled
                  ></v-text-field>
                </v-col>
                <v-col cols="12">
                  <v-text-field
                    label="บทย่อยที่"
                    required
                    v-model="video_subunit"
                  ></v-text-field>
                </v-col>
                <v-col cols="12">
                  <v-text-field
                    label="ชื่อวิดีโอ"
                    v-model="video_name"
                    required
                  ></v-text-field>
                </v-col>
                    <!-- <v-col cols="12">
                    <v-file-input
                    accept="video/*"
                    v-model="videofile"
                    required
                    prepend-icon="mdi-video"
                    label="ไฟล์วิดีโอ"
                    variant="underlined"></v-file-input>
                  </v-col> -->
                <!-- <v-col cols="12">
                  <video autoplay controls class="mt-5" width="100%">
                    <source :src="video_file" />
                  </video>
                </v-col> -->
              </v-row>
            </v-container>
          </v-card-text>
          <v-card-actions>
            <v-spacer></v-spacer>
            <v-btn color="blue darken-1" text @click="dialog = false">
              ปิด
            </v-btn>
            <v-btn color="blue darken-1" text @click="saveUpdate()"> บันทึก </v-btn>
          </v-card-actions>
        </v-card>
                </v-dialog>
                <v-dialog v-model="dialog2" max-width="600px" persistent>
                      <v-card>
          <v-card-title> วิดีโอ </v-card-title>
          <v-card-text>
            <v-container>
              <v-row>
                    <v-col cols="12">
                    <v-file-input
                    accept="video/*"
                    v-model="videofile"
                    required
                    prepend-icon="mdi-video"
                    label="ไฟล์วิดีโอ"
                    variant="underlined"></v-file-input>
                  </v-col>
                  <!-- <v-col>
                    <v-textarea
                    outlined
                              auto-grow disabled
                    label="Base64 value"
                    v-model="base64"/>
                  </v-col> -->
                <!-- <v-col cols="12">
                  <video autoplay controls class="mt-5" width="100%">
                    <source :src="base64" />
                  </video>
                </v-col> -->
              </v-row>
            </v-container>
          </v-card-text>
          <v-card-actions>
            <v-spacer></v-spacer>
            <v-btn color="blue darken-1" text @click="dialog2 = false">
              ปิด
            </v-btn>
            <v-btn color="blue darken-1" text @click="saveUpdateVideo()"> บันทึก </v-btn>
          </v-card-actions>
        </v-card>
                </v-dialog>
                <v-dialog v-model="dialog1" max-width="600px" persistent>
             <v-card>
          <v-card-title> เพิ่มวิดีโอ </v-card-title>
          <v-card-text>
            <v-container>
              <v-form v-model="valid1" ref="form1" enctype='multipart/form-data'>
              <v-row>
                <v-col cols="12">
                  <v-text-field
                    label="บทที่"
                    required
                    v-model="video.lesson_id"
                    disabled
                  ></v-text-field>
                </v-col>
                <v-col cols="12">
                  <v-text-field
                    label="บทย่อยที่"
                    required
                    v-model="video.subunit"
                  ></v-text-field>
                </v-col>
                <v-col cols="12">
                  <v-text-field
                    label="ชื่อวิดีโอ"
                    v-model="video.name"
                    required
                  ></v-text-field>
                </v-col>
                <v-row>
                    <v-col cols="12">
                    <v-file-input
                    accept="video/*"
                    v-model="videofile"
                    required
                    prepend-icon="mdi-video"
                    label="ไฟล์วิดีโอ"
                    variant="underlined"></v-file-input>
                  </v-col>
                    <!-- <v-col  cols="12">
                    <v-img
                      :src="base64"
                      width="100%"
                    ></v-img>{{ videofile ? videofile.name : '' }}
                  </v-col> -->
                  </v-row>
              </v-row>
              </v-form>
            </v-container>
          </v-card-text>
          <v-card-actions>
            <v-spacer></v-spacer>
            <v-btn color="blue darken-1" text @click="dialog1 = false">
              ปิด
            </v-btn>
            <v-btn color="blue darken-1" text @click="insertVideo()"> บันทึก </v-btn>
          </v-card-actions>
        </v-card>
                </v-dialog>

    <!-- <NavbarUser />
    <div>
      <v-breadcrumbs :items="breadcrumbs" large></v-breadcrumbs>
    </div> -->

    <!-- <div>{{item.lesson_unit}}</div>
      <div>{{item.lesson_name}}</div>
    <div>{{item}}</div>-->
    <!-- <v-card class="mt-7 mb-7 pa-5">
      <h1 class="text-center ma-5">
        บทที่ {{ lesson_id}} {{  this.lesson_name }}
        <span class="teal--text">{{ info }}</span>
        {{ video_name }} {{ video_subunit }}
      </h1>
      <div v-for="video in allvideo" v-bind:key="video.video_id">
       {{ video.video_name }} {{ video.video_subunit }}
      </div>
    </v-card> -->
  </div>
</template>

<script>
import axios from 'axios'
import Swal from 'sweetalert2'
import NavbarAdmin from '@/components/NavbarAdmin'
// import { all } from 'q'
export default {
  components: {
    NavbarAdmin
  },
  data () {
    return {
      base64: null,
      videofile: null,
      dialog: false,
      dialog1: false,
      dialog2: false,
      reveal: false,
      alllesson: [],
      allvideo: [],
      valid1: false,
      // lesson_id: localStorage.getItem('vdo_lesson_id'),
      video_id: '',
      video_subunit: '',
      video_name: '',
      video: {
        id: '',
        subunit: '',
        name: '',
        file: ''
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
          text: 'Dashboard',
          disabled: false,
          href: 'admindashboard'
        },
        {
          text: 'จัดการวิดีโอ',
          disabled: false,
          to: { name: 'adminvideo' }
        },
        {
          text: 'วิดีโอ',
          disabled: true
        }
      ],
      item: '',
      lesson: {
        lesson_id: '',
        lesson_unit: '',
        lesson_name: '',
        lesson_description: ''
      },
      lesson_id: '',
      lesson_unit: '',
      // lesson_name: '',
      lesson_description: ''
    }
  },
  created () {
    this.lesson_id = this.$route.params.id
    this.lesson_name = localStorage.getItem('lesson_name')
    console.log('jjjj', this.lesson_name)
    this.getLesson()
    this.getVideo()
  },
  // computed: {
  //   lessonChilren () {
  //     return this.getVideo
  //   }
  // },
  watch: {
    videofile: function (newVal, oldVal) {
      if (newVal) {
        this.createBase64Image(newVal)
      } else {
        this.base64 = null
      }
    }
  },
  methods: {
    createBase64Image: function (FileObject) {
      const reader = new FileReader()
      reader.onload = (event) => {
        this.base64 = event.target.result
      }
      reader.readAsDataURL(FileObject)
    },
    async getLesson () {
      axios.get('http://localhost/vue-backend/editLesson.php').then(res => {
        console.log('data:', res.data)
        if (res.data) {
          this.alllesson = res.data
        }
      })
    },

    async getVideo () {
      const url =
        'http://localhost/vue-backend/videoDetail.php?lesson_id=' + this.lesson_id
      // alert(url)
      // alert('http://localhost/vue-backend/lesson_detail.php?id=', this.lesson_id)

      axios
        .get(url)
        .then(response => {
          console.log(response, 'test')
          this.allvideo = response.data
          console.log(response.data, 'goo')
          if (response.data.length) {
            this.video = response.data[0]
          } else {
            this.video = {
              lesson_id: this.lesson_id,
              subunit: '',
              name: '',
              videofile: '',
              text: 'ไม่มีวิดีโอในบทเรียน'
            }
          }
        //   this.info = response.data[0]
        //   // alert(this.info)
        //   this.lesson_id = this.info.lesson_id
        //   this.video_subunit = this.info.video_subunit
        //   this.video_name = this.info.video_name
        //   this.video_file = this.info.video_file
        })
      /*
        axios.post(url, {
          request: 1

        })
          .then(function (response) {
            alert('work')
            this.data = response.data[0]
            alert(response.data.status)
            // alert(this.data.status)
          })
        */
    },
    OpenDialog () {
      this.dialog1 = true
    },
    insertVideo () {
      if (this.$refs.form1.validate()) {
        const formData = new FormData()
        formData.append('video_subunit', this.video.subunit)
        formData.append('video_name', this.video.name)
        formData.append('video_file', this.base64)
        formData.append('lesson_id', this.lesson_id)
        //   alert(this.file)
        var headers = {
          'Content-Type': 'application/json',
          'Access-Control-Allow-Origin': '*'
        }

        axios.post(
          'http://localhost/vue-backend/insertVideo.php',
          formData,
          headers
        )
          .then(function (response) {
            // handle success
            console.log(response)
            console.log('success')
            Swal.fire({
              icon: 'success',
              title: 'เพิ่มสำเร็จ',
              showConfirmButton: false,
              // text: 'คำอธิบาย',
              customClass: {
                title: 'csss'
              },
              timer: 1500
            })
          })
          .catch(function (response) {
            // handle error
            console.log(response)
            console.log('sorry')
          })
        this.getVideo()
        location.reload()
        this.dialog1 = false
        this.$refs.form1.reset()
      }
    },
    editVideo (data) {
      // console.log('item:', this.items)มันไม่มีค่า มันเอามาจากตัวแปรitems:{}ข้างบน มันว่าง
      // console.log('item:', data)คือฟังก์ชันedit(data)ข้างบนdataที่ส่งมา
      this.dialog2 = true
      this.video_subunit = data.video_subunit
      this.video_file = data.video_file
      // this.create_at = data.create_at
    },
    async saveUpdateVideo () {
      var bodyValue = {
        video_subunit: this.video_subunit,
        video_file: this.base64

      }
      await axios.put('http://localhost/vue-backend/updatefileVideo.php', bodyValue)
      // if (data === 'success') {
      this.dialog2 = false
      this.getVideo()

      Swal.fire({
        icon: 'success',
        title: 'แก้ไขสำเร็จ',
        showConfirmButton: false,
        // text: 'คำอธิบาย',
        customClass: {
          title: 'csss'
        },
        timer: 1500
      })
      this.dialog2 = false
      this.video_file = ''
      location.reload()
      this.getVideo()
      // setTimeout(() => {
      //   this.getData()
      // }, 2000)
      // }
    },
    editItem (data) {
      // console.log('item:', this.items)มันไม่มีค่า มันเอามาจากตัวแปรitems:{}ข้างบน มันว่าง
      // console.log('item:', data)คือฟังก์ชันedit(data)ข้างบนdataที่ส่งมา
      this.dialog = true
      this.lesson_id = data.lesson_id
      this.video_id = data.video_id
      this.video_subunit = data.video_subunit
      this.video_name = data.video_name
      this.create_at = data.create_at
    },
    async saveUpdate () {
      var bodyValue = {
        lesson_id: this.lesson_id,
        video_id: this.video_id,
        video_subunit: this.video_subunit,
        video_name: this.video_name,
        create_at: this.create_at

      }
      var { data } = await axios.put('http://localhost/vue-backend/updateVideo.php', bodyValue)
      console.log(data, 'data here!')
      if (data === 'success') {
        this.dialog = false
        Swal.fire({
          icon: 'success',
          title: 'แก้ไขสำเร็จ',
          showConfirmButton: false,
          // text: 'คำอธิบาย',
          customClass: {
            title: 'csss'
          },
          timer: 1500
        })
        this.dialog = false
        // location.reload()
        this.getVideo()
      // setTimeout(() => {
      //   this.getData()
      // }, 2000)
      }
    },
    async deleteItem (data) {
      Swal.fire({
        title: 'คุณต้องการลบใช่ไหม?',
        // text: "You won't be able to revert this!",
        icon: 'warning',
        showCancelButton: true,
        confirmButtonColor: '#56a062',
        cancelButtonColor: '#ea5859',
        confirmButtonText: 'ตกลง',
        cancelButtonText: 'ยกเลิก'
      }).then((result) => {
        if (result.isConfirmed) {
          axios.post(
            'http://localhost/vue-backend/deleteVideo.php',
            {
              video_id: data.video_id
            }
          )
          Swal.fire({
            icon: 'success',
            title: 'ลบสำเร็จ',
            showConfirmButton: false,
            // text: 'คำอธิบาย',
            customClass: {
              title: 'csss'
            },
            timer: 1500
          })
          setTimeout(() => {
            location.reload()
            this.getVideo()
          }, 1500)
          // this.getVideo()
        }
      })
    }
    // async deleteItem (data) {
    //   var { data: deletes } = await axios.post(
    //     'http://localhost/vue-backend/deleteVideo.php',
    //     {
    //       video_subunit: data.video_subunit
    //     }
    //   )
    //   console.log(deletes, 'delete')
    //   if (deletes === 'success') {
    //     this.dialog = false
    //     Swal.fire({
    //       icon: 'success',
    //       title: 'ลบสำเร็จ',
    //       showConfirmButton: false,
    //       // text: 'คำอธิบาย',
    //       customClass: {
    //         title: 'csss'
    //       },
    //       timer: 1500
    //     })
    //     setTimeout(() => {
    //       this.getVideo()
    //     }, 1500)
    //   }
    // }
  }
}
</script>

  <style scoped>
.v-breadcrumbs >>> a {
  color: #fcad74 !important;
}
</style>
