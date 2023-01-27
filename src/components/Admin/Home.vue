<template>
  <div class="app">
    <NavbarAdmin/>
    <div>
    <v-breadcrumbs
      :items="breadcrumbs"
      large
    ></v-breadcrumbs>
  </div>
    <h1 class="mt-0 mb-7" align="center">กระทู้</h1>
<v-card class="cardShowuser">
    <v-card-title>
      <!-- <v-icon class="mr-2" color="#fcad74">mdi-book-open-variant</v-icon> -->
      <!-- <v-card class="mx-auto ma-5" width="60%">
     <v-card-title> -->
<v-row
    align="center"
    justify="space-around"
    >
        <v-col cols="9" class="ma-0 col9">
          <center>
            <v-form v-model="valid" ref="form">
          <v-text-field
          v-model="post_detail"
          class="inputpost"
            label="โพสต์ข้อความ"
            color="#099fae"
            filled
            rounded
            clearable
            dense
            @keyup.enter="writePost()"
          ></v-text-field>
          </v-form>
          </center>
        </v-col>
        <v-col cols="3" class="ma-0">
          <!-- <div class="text-center"> -->
            <center>
          <v-btn :disabled='!post_detail'  height="52px" width="100%" class="white--text" rounded color="#099fae" @click="writePost()">โพสต์</v-btn>
          </center>
          <!-- </div> -->
        </v-col>
</v-row>
      <!-- </v-card-title>
    </v-card> -->
    </v-card-title>
    </v-card>

    <v-card class="cardShowuser" v-for="(postuser, index) in allapprove" :key="postuser.post_id">
      <!-- {{postuser}} -->
      <v-card-title>
        Siriwimol&nbsp;<span style="color: #21777f;">{{postuser.post_detail}}</span>
      </v-card-title>

      <v-card-subtitle>โพสต์เมื่อ วันที่ {{postuser.create_at}} น.</v-card-subtitle>

      <v-card-actions>
        <v-spacer></v-spacer>

        <v-btn v-if='panel !== postuser.post_id' icon @click="showtest(index, postuser.post_id)">
          <v-icon color="#099fae">mdi-comment</v-icon>
        </v-btn>
        <v-btn v-else icon @click="showtest2(index)">
          <v-icon color="#099fae">mdi-comment-outline</v-icon>
        </v-btn>
      </v-card-actions>

      <v-expand-transition>
        <div v-show="show === index" >
          <v-container fluid >
            <v-form  v-model="valid2" ref="form2">
              <div class="mx-auto ma-3" width="60%" v-for="comment in allcomment" v-bind:key="comment.comment_id">
              <div v-if="comment.post_id === postuser.post_id">
              <v-divider class="mb-2"></v-divider>
              <div color="#099fae">Siriwimol <span style="color: gray;">แสดงความคิดเห็นเมื่อ {{comment.create_at}} น.</span> </div>
              <div>{{comment.comment_detail}}</div>
              <!-- <v-divider class="mt-3"></v-divider> -->
              </div>
              </div>
            <v-textarea
              v-model="comment_detail"
              filled
              auto-grow
              label="แสดงความคิดเห็น"
              rows="2"
              color="#099fae"
              row-height="20"
              @keyup.enter="writeComment(postuser.post_id)"
            >
            </v-textarea>
            <div class="text-center">
              <v-btn rounded color="#099fae" dark @click="writeComment(postuser.post_id)">
                <!-- <v-icon aria-label="My Account" role="img" aria-hidden="false">
                  mdi-comment
                </v-icon> -->
                แสดงความคิดเห็น
              </v-btn>
            </div>
            </v-form>
          </v-container>
        </div>
      </v-expand-transition>
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
  name: 'show',
  data: () => ({
    allapprove: [],
    allpost: [],
    allcomment: [],
    show: false,
    post_detail: '',
    post_id: '',
    post_status: '',
    user_id: '',
    comment_detail: '',
    comment_id: '',
    postuser: {
      post_detail: '',
      create_at: ''
    },
    comment: {
      comment_detail: '',
      create_at: ''
    },
    valid: false,
    valid2: false,
    panel: '',
    breadcrumbs: [
      {
        text: 'Dashboard',
        disabled: false,
        href: 'admindashboard'
      },
      {
        text: 'หน้าแรก',
        disabled: true,
        href: 'adminhome'
      }
    ]
    // postRules: [
    //   v => !!v || ''
    // ]
  }
  ),
  created () {
    this.getPost()
    this.getComment()
  },
  // mounted () {
  //   this.$emit('test', true)
  // },
  methods: {
    showtest (index, postid) {
      this.show = index
      this.panel = postid
      this.comment_detail = ''
    },
    showtest2 (index) {
      this.show = !index
      this.panel = ''
    },
    async getPost () {
      console.log('rewload')
      axios.get('http://localhost/vue-backend/postApprove.php').then((res) => {
        console.log('data:', res.data)
        if (res.data) {
          this.allpost = res.data
          this.allapprove = res.data
          console.log(this.allpost, 'โพสต์จ้า')
        }
      })
    },
    async getComment () {
      console.log('rewload')
      axios.get('http://localhost/vue-backend/comment.php').then((res) => {
        console.log('data:', res.data)
        if (res.data) {
          this.allcomment = res.data
          console.log(this.allcomment, 'Comment')
        }
      })
    },
    async writePost () {
      // console.log(this.$refs.form.validate(), 'pp')
      if (this.$refs.form.validate()) { // กรอกครบมั้ย
        var { data } = await axios.post('http://localhost/vue-backend/insertPost.php', {
          // post_id: this.post_id,
          post_detail: this.post_detail,
          post_status: 'waiting',
          user_id: 61
        })
        if (data === 'success') {
          Swal.fire({
            icon: 'success',
            title: 'โพสต์สำเร็จ',
            showConfirmButton: false,
            // text: 'คำอธิบาย',
            customClass: {
              title: 'csss'
            },
            timer: 1500
          })
        }
        this.getPost()
        // this.postuser.post_detail = ''
        this.$refs.form.reset()
      }
    },
    async writeComment (postid) {
      console.log(postid, 'post ja')
      console.log(this.$refs.form.validate(), 'pp')
      if (this.comment_detail !== '') { // กรอกครบมั้ย
        var { data } = await axios.post('http://localhost/vue-backend/insertComment.php', {
          comment_detail: this.comment_detail,
          post_id: postid,
          user_id: 61
        })
        if (data === 'success') {
          Swal.fire({
            icon: 'success',
            title: 'คอมเมนต์สำเร็จ',
            showConfirmButton: false,
            // text: 'คำอธิบาย',
            customClass: {
              title: 'csss'
            },
            timer: 1500
          })
        }
        this.getComment()
        // this.postuser.post_detail = ''
        this.comment_detail = ''
      }
    }
  }
}

</script>

<style scoped>
*{
font-family: 'Prompt', sans-serif;
}
.btnpost {
display: flex !important;
  justify-content: center !important;
  align-items: center !important;
  /* height: 50px !important; */
  /* border: 3px solid green !important; */
  text-align: center !important;
  padding: 0px 30px !important;
}
.col9{
  height: 76px;
}
.inputpost{
  display: flex;
  justify-content: center;
  align-items: center;
  text-align: center;
  width: 100%;
}
.csss {
  /* color: black !important; */
  font-family: 'Kanit';
  font-weight: 400;
}
.cardShowuser{
  margin-top: 2%;
}
.v-breadcrumbs >>> a {
    color: #fcad74;
}
/* .app{
  background-color: black;
} */
</style>
