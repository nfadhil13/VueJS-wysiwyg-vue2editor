<template>
  <div id="app">
    <vue-editor
      id="editor"
      useCustomImageHandler
      @image-added="handleImageAdded"
      @image-removed="handleImageDeleted"
      v-model="htmlForEditor"
    ></vue-editor>
    <button @click="post">Post Data</button>
  </div>
</template>

<script>
import { VueEditor } from "vue2-editor";
import axios from "axios";
import Const from "../const";
export default {
  components: {
    VueEditor,
  },

  data() {
    return {
      htmlForEditor: "",
      addedImagesURL : []
    };
  },

  methods: {
    async post(){
        let result;
        try{
        const formData = new FormData();
        formData.append('judul' , 'test_judul1');
        formData.append('artikel' , this.htmlForEditor);
        formData.append('kategori_berita' , 'software');
        formData.append('jurnalis', 'dodit');
        formData.append('deskripsi_jurnalis' , 'ini deskripsi');
        const url = Const.NEWS_URL + "/new-news";
        result = await axios.post(url , formData)
        alert(result.data.message);
        }catch(err){
            console.log(err.response);
        }
    },
    async handleImageAdded(file, Editor, cursorLocation, resetUploader) {
      // An example of using FormData
      // NOTE: Your key could be different such as:
      // formData.append('file', file)
      try {
        const formData = new FormData();
        formData.append("url_gambar", file);
        const url = Const.NEWS_URL + "/new-image";
        const result = await axios.post(url, formData);
        const imageUrl = Const.BASE_URL + "/" + result.data.data.url; // Get url from response
        Editor.insertEmbed(cursorLocation, "image", imageUrl);
        resetUploader();
      } catch (err) {
        console.log(err);
      }
    },
    /*
        @params fullUrl is image fullURL with the domain , example :
        'http://localhost:3000/public/images/image.jpg'
        
    */
    async handleImageDeleted(fullUrl) {
      //URL is image without domain as prefix , exmample : /public/images/image.jpg
      const imageUrl = fullUrl.substring(Const.BASE_URL.length + 1, fullUrl.length);
      try {
        const apiUrl = Const.NEWS_URL + "/delete-image";
        console.log(imageUrl);
        const result = await axios.post(apiUrl , {
            filepath : imageUrl
        })
        console.log(result.data.message);
      } catch (err) {
        console.log(err);
      }
    }
  },
};
</script>