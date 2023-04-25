<template>
  <div>
    <NavbarAdmin />
    <div>
      <v-breadcrumbs :items="breadcrumbs" large></v-breadcrumbs>
    </div>
    <v-card class="cardShowuser mt-0">
      <v-tabs background-color="transparent" color="#099fae" grow v-model="tab">
        <v-tab @click="getPostWaiting('waiting')" >กระทู้รอการอนุมัติ</v-tab>
        <v-tab @click="getPostApprove('approve')">กระทู้อนุมัติสำเร็จ</v-tab>
        <v-tab @click="getPostAll('all')">กระทู้ทั้งหมด</v-tab>
      </v-tabs>
      <v-card-title v-show="status === 'waiting'">
        <v-icon class="mr-2" color="#fcad74">mdi-post</v-icon>กระทู้
        <v-spacer></v-spacer>
        <v-text-field
          v-model="search"
          filled
          rounded
          dense
          append-icon="mdi-magnify"
          label="ค้นหา"
          color="#099fae"
          single-line
          hide-details
        ></v-text-field>
      </v-card-title>
      <!-- <v-card-title>friend</v-card-title> -->
      <v-data-table
        :footer-props="{
          itemsPerPageText: 'แถวต่อหน้า',
          pageText: '{0}-{1} จาก {2}',
          'items-per-page-all-text': 'ทั้งหมด',
        }"
        :items="allpost"
        :headers="headers"
        :items-per-page="5"
        :search="search"
        v-show="status === 'waiting'"
        no-data-text="ไม่พบข้อมูล"
        no-results-text="ไม่พบข้อมูลที่ค้นหา"
      >
        <template slot="data" slot-scope="props">
          <td>{{ props.data.post_detail }}</td>
        </template>
        <!-- <template v-slot:item.img="{ item }">
          <v-img
      class="white--text align-end ma-2"
      width="20%"
      :src="item.post_img">
    </v-img>
        </template> -->
        <template v-slot:item.check="{ item }">
          <v-icon small @click="editItem(item)" color="#56a062"
            >mdi-pencil</v-icon
          >
        </template>
        <template v-slot:item.delete="{ item }">
          <v-icon small @click="deleteItem(item)" color="#ea5859"
            >mdi-delete</v-icon
          >
        </template>
      </v-data-table>

      <v-card-title v-show="status === 'approve'">
        <v-icon class="mr-2" color="#fcad74">mdi-post</v-icon>กระทู้
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
        :footer-props="{
          itemsPerPageText: 'แถวต่อหน้า',
          pageText: '{0}-{1} จาก {2}',
          'items-per-page-all-text': 'ทั้งหมด',
        }"
        :items="allpost"
        :headers="headers"
        :items-per-page="5"
        :search="search"
        v-show="status === 'approve'"
        no-data-text="ไม่พบข้อมูล"
        no-results-text="ไม่พบข้อมูลที่ค้นหา"
      >
        <template slot="data" slot-scope="props">
          <td>{{ props.data.post_detail }}</td>
        </template>
        <!-- <template v-slot:item.img="{ item }">
          <v-img
      class="white--text align-end ma-2"
      width="20%"
      :src="item.post_img">
    </v-img>
        </template> -->
        <template v-slot:item.check="{ item }">
          <v-icon small @click="editItem(item)" color="#56a062"
            >mdi-pencil</v-icon
          >
        </template>
        <template v-slot:item.delete="{ item }">
          <v-icon small @click="deleteItem(item)" color="#ea5859"
            >mdi-delete</v-icon
          >
        </template>
      </v-data-table>

      <v-card-title v-show="status === 'all'">
        <v-icon class="mr-2" color="#fcad74">mdi-post</v-icon>กระทู้
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
        :footer-props="{
          itemsPerPageText: 'แถวต่อหน้า',
          pageText: '{0}-{1} จาก {2}',
          'items-per-page-all-text': 'ทั้งหมด',
        }"
        :items="allpost"
        :headers="headers"
        :items-per-page="5"
        :search="search"
        v-show="status === 'all'"
        no-data-text="ไม่พบข้อมูล"
        no-results-text="ไม่พบข้อมูลที่ค้นหา"
      >
        <template slot="data" slot-scope="props">
          <td>{{ props.data.post_detail }}</td>
        </template>
        <!-- <template v-slot:item.img="{ item }">
          <v-img
      class="white--text align-end ma-2"
      width="20%"
      :src="item.post_img">
    </v-img>
        </template> -->
        <template v-slot:item.check="{ item }">
          <v-icon small @click="editItem(item)" color="#56a062"
            >mdi-pencil</v-icon
          >
        </template>
        <template v-slot:item.delete="{ item }">
          <v-icon small @click="deleteItem(item)" color="#ea5859"
            >mdi-delete</v-icon
          >
        </template>
      </v-data-table>

      <v-dialog v-model="dialog" max-width="600px" persistent>
        <v-card>
          <v-card-title>จัดการกระทู้</v-card-title>
          <v-card-text>
            <v-container>
              <v-form v-model="valid" ref="form">
                <v-row>
                  <!-- <v-col cols="12">
                    <v-text-field
                      label="บทที่"
                      required
                      v-model="post_id"
                    ></v-text-field>
                  </v-col>-->
                  <v-col cols="12">
                    <v-text-field
                      label="กระทู้"
                      required
                      disabled
                      v-model="post_detail"
                    ></v-text-field>
                  </v-col>
                  <v-col cols="12"  v-if="post_img==='null'" class="pa-0">
                  </v-col>
                  <v-col cols="12"  v-if="post_img!=='null'" class="d-flex align-center justify-center">
                    <v-img
                      class="white--text ma-2"
                      width="70%"
                      :src="post_img">
                    </v-img>
                  </v-col>
                  <v-col
                    cols="12"
                    class="ma-0 pt-0 pb-0"
                    v-if="status === 'waiting'"
                  >
                    <v-checkbox
                      v-model="post_status"
                      label="อนุมัติกระทู้"
                      value="approve"
                    ></v-checkbox>
                  </v-col>
                  <!-- <v-col
                    cols="12"
                    class="ma-0 pt-0 pb-0"
                    v-if="status === 'waiting' "
                  >
                          <v-chip color="#ffe8e8">
                    <span style="color: #ea5859">รอการอนุมัติกระทู้</span>
                  </v-chip>
                  </v-col> -->
                  <v-col
                    cols="12"
                    class="ma-0 pt-0 pb-0"
                    v-if="status === 'approve' "
                  >
                          <v-chip color="#eaffed">
                    <span style="color: #56a062">อนุมัติกระทู้แล้ว</span>
                  </v-chip>
                  </v-col>
                </v-row>
              </v-form>
            </v-container>
          </v-card-text>
          <v-card-actions v-show="status === 'approve' ">
            <v-spacer></v-spacer>
            <v-btn color="blue darken-1" text @click="dialog = false"
              >ปิด</v-btn
            >
          </v-card-actions>
          <v-card-actions v-show="status !== 'approve' ">
            <v-spacer></v-spacer>
            <v-btn color="blue darken-1" text @click="dialog = false"
              >ปิด</v-btn
            >
            <v-btn color="blue darken-1" text @click="saveUpdate()"
              >บันทึก</v-btn
            >
          </v-card-actions>
        </v-card>
      </v-dialog>
    </v-card>
  </div>
</template>

<script>
import axios from 'axios'
import Swal from 'sweetalert2'
import NavbarAdmin from '@/components/NavbarAdmin'

export default {
  components: {
    NavbarAdmin
  },
  data () {
    return {
      show3: false,
      dialog: false,
      allpost: [],
      allwaiting: [],
      allapprove: [],
      search: '',
      post_id: '',
      post_detail: '',
      post_status: '',
      postuser: {
        post_detail: ''
      },
      user: {
        user_id: ''
      },
      valid: false,
      headers: [
        // {
        //   text: 'ชื่อ',
        //   align: 'start',
        //   // sortable: false,
        //   value: 'user_firstname'
        // },
        { text: 'กระทู้', value: 'post_detail' },
        // { text: 'รูป', value: 'img' },
        { text: 'จัดการกระทู้', value: 'check', sortable: false },
        { text: 'ลบ', value: 'delete', sortable: false }
      ],
      data: [],
      breadcrumbs: [
        {
          text: 'Dashboard',
          disabled: false,
          href: 'admindashboard'
        },
        {
          text: 'จัดการกระทู้',
          disabled: true,
          href: 'adminpost'
        }
      ]
    }
  },
  created () {
    this.status = 'waiting'
    // this.statusWaiting = 'waiting'
    // this.statusApprove = 'approve'
    this.getPostWaiting(this.status)
    // this.getReload(this.status)
    // this.getPostWaiting(this.statusWaiting)
    // this.getPostApprove(this.statusApprove)
  },
  methods: {
    async getPostAll (value) {
      this.status = value
      console.log('rewload')
      axios.get('http://localhost/vue-backend/post.php').then((res) => {
        console.log('data:', res.data)
        if (res.data) {
          this.allpost = res.data
        }
      })
    },
    async getPostWaiting (value) {
      this.status = value
      axios.get('http://localhost/vue-backend/postWaiting.php').then((res) => {
        console.log('data:', res.data)
        if (res.data) {
          this.allpost = res.data
          this.allwaiting = res.data
        }
      })
    },
    async getPostApprove (value) {
      this.status = value
      axios.get('http://localhost/vue-backend/postApprove.php').then((res) => {
        console.log('data:', res.data)
        if (res.data) {
          this.allpost = res.data
          this.allapprove = res.data
          // this.allstudent = res.data
        }
      })
    },
    // updateStatus (data) {
    //   this.post_status = data.post_status
    // },
    editItem (data) {
      // console.log('item:', this.items)มันไม่มีค่า มันเอามาจากตัวแปรitems:{}ข้างบน มันว่าง
      // console.log('item:', data)คือฟังก์ชันedit(data)ข้างบนdataที่ส่งมา
      this.dialog = true
      this.post_id = data.post_id
      this.post_detail = data.post_detail
      this.post_img = data.post_img
      this.post_status = data.post_status
      this.create_at = data.create_at
      // console.log('friend data item', data)
      // console.log(this.allshow)
    },
    async saveUpdate (status) {
      if (this.post_status !== null && this.$refs.form.validate()) {
        // กรอกครบมั้ย
        var bodyValue = {
          post_id: this.post_id,
          post_detail: this.post_detail,
          post_img: this.post_img,
          post_status: this.post_status,
          create_at: this.create_at
        }
        var { data } = await axios.put(
          'http://localhost/vue-backend/updateStatusPost.php',
          bodyValue
        )
        console.log(data, 'data here!')
        if (data === 'success') {
          this.dialog = false
          Swal.fire({
            icon: 'success',
            title: 'อนุมัติกระทู้สำเร็จ',
            showConfirmButton: false,
            // text: 'คำอธิบาย',
            customClass: {
              title: 'csss'
            },
            timer: 1500
          })
        }
        this.status = status
        this.getReload(this.status)
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
            'http://localhost/vue-backend/deletePost.php',
            {
              post_id: data.post_id,
              delete_at: data.delete_at
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
            this.status = data
            this.getReload(this.status)
          }, 1500)
        }
      })
    },
    // async deleteItem (data) {
    //   // var idDel = parseInt(data.id)
    //   var { data: deletes } = await axios.post(
    //     'http://localhost/vue-backend/deletePost.php',
    //     {
    //       post_id: data.post_id,
    //       delete_at: data.delete_at
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
    //     this.status = data
    //     this.getReload(this.status)
    //   }
    // },
    getReload (status) {
      if (status === 'all') {
        this.getPostAll('all')
      } else if (status === 'wating') {
        this.getPostWaiting('wating')
      } else {
        this.getPostWaiting('approve')
      }
    }
  }
  // mounted () {
  //   this.getUser()
  // }
}
</script>

<style scoped>
* {
  font-family: "Prompt", sans-serif;
}
.cardShowuser {
  margin-top: 5%;
}
.head {
  background-color: red;
}
.v-breadcrumbs >>> a {
  color: #fcad74;
}
</style>
