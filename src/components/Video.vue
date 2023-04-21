<template>
  <div>
    <h1>Video</h1>
    {{ img }}
    <v-file-input label="File input" :value="current" ></v-file-input>
    <v-container>
            <h2 class="font-weight-light pink--text">File upload multiple</h2>
            <v-file-input accept="image/*"
                label="Select files"
                prepend-icon="photo"
                multiple chips color="pink"
                v-model="files"
                @change="addFiles"></v-file-input>
            <v-row>
                <v-col sm="4" v-for="(file,f) in files" :key="f">
                    {{file.name}}
                    <img :ref="'file'" src="//placehold.it/400/99cc77" class="img-fluid" :title="'file' + f" />
                </v-col>
            </v-row>
        </v-container>

    <v-card height="50vh">
    <v-card-text>
      <v-img
        :src="image ? imagePreview : 'https://picsum.photos/id/11/500/300'"
        height="30vh"
      ></v-img>
      <v-file-input
        v-model="image"
        accept="image/png, image/jpeg, image/bmp"
        placeholder="Pick an image"
        prepend-icon="mdi-camera"
        @change="selectImage"
        @click:clear="clearImagePreview()"
        label="Image"
      ></v-file-input>
    </v-card-text>
  </v-card>
    <v-form v-model="valid">
        <v-file-input
        required
    label="File input"
    v-model="image"
  ></v-file-input>
  {{ filess }}
  <v-btn
      color="error"
      class="mr-4"
      @click="addFormData()"
    >
      Save
    </v-btn>
    </v-form>
    <v-container>
        <v-row>
          <v-col cols="12" sm="12">
            <v-sheet
              class="pt-2"
              min-height="70vh"
              rounded="lg">
              <v-row class="ma-4">
                <v-form v-model="valid">
                <v-col cols="12" sm="12">
                  <v-file-input
                    outlined
                    label="File"
                    v-model="image"/>
                </v-col>
                <v-btn
      color="error"
      class="mr-4"
      @click="addFormData()"
    >
      Save
    </v-btn>
                <v-col cols="12" sm="12">
                  <v-textarea
                    outlined
                              auto-grow disabled
                    label="Base64 value"
                    v-model="base64"/>
                </v-col>
              </v-form>
              </v-row>
            </v-sheet>
          </v-col>
        </v-row>
      </v-container>
  </div>
</template>

<script>
import axios from 'axios'

export default {
  data: () => ({
    current: new File(['foo'], 'frienddddd.jpg'),
    file: '',
    valid: false,
    image: null,
    base64: null,
    img: 'friend.jpg',
    imagePreview: '',
    files: [],
    readers: []
  }),
  watch: {
    image: function (newVal, oldVal) {
      if (newVal) {
        this.createBase64Image(newVal)
      } else {
        this.base64 = null
      }
    }
  },
  methods: {
    addFiles () {
      console.log('files', this.files)
      this.files.forEach((file, f) => {
        this.readers[f] = new FileReader()
        this.readers[f].onloadend = (e) => {
          const fileData = this.readers[f].result
          const imgRef = this.$refs.file[f]
          imgRef.src = fileData
          console.log(fileData)
          // send to server here...
        }

        this.readers[f].readAsDataURL(this.files[f])
      })
    },
    async selectImage (e) {
      const file = e
      if (!file) return
      const readData = (f) =>
        new Promise((resolve) => {
          const reader = new FileReader()
          reader.onloadend = () => resolve(reader.result)
          reader.readAsDataURL(f)
        })
      const data = await readData(file)
      this.imagePreview = data
    },
    async clearImagePreview () {
      this.imagePreview = ''
    },
    createBase64Image: function (FileObject) {
      const reader = new FileReader()
      reader.onload = (event) => {
        this.base64 = event.target.result
      }
      reader.readAsDataURL(FileObject)
    },
    addFormData () {
      const formData = new FormData()
      formData.append('file', this.base64)
      //   alert(this.file)
      var headers = {
        'Content-Type': 'application/json',
        'Access-Control-Allow-Origin': '*'
      }

      axios.post('http://localhost/vue-backend/video.php', formData, headers)
        .then(function (response) {
          // handle success
          console.log(response)
          console.log('success')
        })
        .catch(function (response) {
          // handle error
          console.log(response)
          console.log('sorry')
        })
    }
  }
}
</script>

<style>
.img-fluid {
    width: 100%;
}
</style>
