<template>
<div>
  <NavbarAdmin/>
  <div>
<v-breadcrumbs
  :items="breadcrumbs"
  large
></v-breadcrumbs>
</div>
  <v-card class="cardShowuser mt-0">
    <v-card-title>
      <v-icon class="mr-2" color="#fcad74">mdi-notebook-edit</v-icon>
     แบบทดสอบ
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
    </v-card-title>
    <!-- <v-card-title>friend</v-card-title> -->
       <v-data-table
       :items="alllesson"
       :headers="headers"
       :items-per-page="5"
       :search="search"
       no-data-text="ไม่พบข้อมูล"
        no-results-text="ไม่พบข้อมูลที่ค้นหา">
       <template slot="data">
        <td>{{lesson_unit}}</td>
        <td>{{lesson_name}}</td>
      </template>
      <!-- <template v-slot:item.edit="{ item }">
        <router-link v-bind:to="'/admininsertquiz/'+ lesson.lesson_unit">
    <v-btn
      text
        color="#56a062"
    >
      เพิ่มแบบทดสอบ
    </v-btn>
    </router-link>

    </template> -->
      <template v-slot:item.edit="{ item }">
        <!-- <v-icon
        small
        class="mr-2"
        @click="editItem(item)"
      >
        mdi-pencil
      </v-icon> -->
    <v-btn
      text
              @click="linkDetail(item)"
        color="#56a062"
    >
      เพิ่มแบบทดสอบ
    </v-btn>
    </template>
    </v-data-table>
  </v-card>
  </div>
</template>

<script>
import axios from 'axios'
import NavbarAdmin from '@/components/NavbarAdmin'

import dedent from 'dedent'
import hljs from 'highlight.js'

// highlight.js style
import 'highlight.js/styles/tomorrow.css'

export default {
  title: 'Theme: snow',
  components: {
    NavbarAdmin
  },
  data () {
    return {
      dialog: false,
      dialog1: false,
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
      breadcrumbs: [
        {
          text: 'Dashboard',
          disabled: false,
          href: 'admindashboard'
        },
        {
          text: 'จัดการแบบทดสอบ',
          disabled: true,
          href: 'adminquiz'
        }
      ],
      valid: false,
      valid2: false,
      search: '',
      headers: [
        {
          text: 'บทที่',
          align: 'start',
          value: 'lesson_unit'
        },
        { text: 'บทเรียนเรื่อง', value: 'lesson_name' },
        // { text: 'เนื้อหาบทเรียน', value: 'lesson_description' },
        { text: '', value: 'edit', sortable: false }
      ],
      data: [],
      editorOption: {
        modules: {
          toolbar: [
            ['bold', 'italic', 'underline', 'strike'],
            ['blockquote', 'code-block'],
            [{ header: 1 }, { header: 2 }],
            [{ list: 'ordered' }, { list: 'bullet' }],
            [{ script: 'sub' }, { script: 'super' }],
            [{ indent: '-1' }, { indent: '+1' }],
            [{ direction: 'rtl' }],
            [{ size: ['small', false, 'large', 'huge'] }],
            [{ header: [1, 2, 3, 4, 5, 6, false] }],
            [{ font: [] }],
            [{ color: [] }, { background: [] }],
            [{ align: [] }],
            ['clean'],
            ['link', 'image', 'video']
          ],
          syntax: {
            highlight: text => hljs.highlightAuto(text).value
          }
        }
      },
      content: dedent``
    }
  },
  created () {
    this.getLesson()
  },
  methods: {
    async getLesson () {
      axios.get('http://localhost/vue-backend/editLesson.php').then((res) => {
        console.log('data:', res.data)
        if (res.data) {
          this.alllesson = res.data
        }
      })
    },
    linkDetail (data) {
      console.log(data, 'id จ้า')
      this.$router.push({ name: 'admininsertquiz', params: { id: data.lesson_unit } })
      // this.$router.push({ name: 'test', params: { id: data } })
    }
  }
}
</script>

<style scoped>
*{
font-family: 'Prompt', sans-serif;
}
.cardShowuser{
  margin-top: 5%;
}
.head{
  background-color: red;
}
.example {
    display: flex;
    flex-direction: column;
}
    .editor {
        width: 100%;
      height: 30rem;
      overflow: hidden;
    }

    .output {
      width: 100%;
      height: 20rem;
      margin: 0;
      border: 1px solid #ccc;
      overflow-y: auto;
      resize: vertical;
    }
      .code {
        padding: 1rem;
        height: 16rem;
      }

      .ql-snow {
        border-top: none;
        height: 24rem;
      }
      .v-breadcrumbs >>> a {
    color: #fcad74;
}

</style>
