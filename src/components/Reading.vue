<template>
    <div>
        <input type="number" v-model="contentId"><button :disabled="contentId < 0" @click="fetchContentById"> Fetch </button>
        <div v-html="content"></div>
    </div>
</template>

<script>
import axios from 'axios';
import Const from '../const';
export default {
    methods: {
        async fetchContentById(){
            try{
                const url = Const.NEWS_URL + '/' + this.contentId
                const result = await axios.get(url);
                console.log(result.data.data);
                this.content = result.data.data.artikel;
                console.log(this.content)
            }catch(err){
                console.log(err);
            }
        }
    },
    data(){
        return{
            content : '',
            contentId : 0
        }
    }
}
</script>