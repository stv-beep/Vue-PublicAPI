<template>
<div class="main">
      <router-link to="/">
        <img src="https://raw.githubusercontent.com/sethvm/kimiquotes/main/kimi.gif" class="kimi-img" />
        <div class="detail">
          <p>#{{ quote.id }}</p>
          <br>
          <h3>"{{ quote.quote }}"</h3>
          <br>
          <p v-if="this.quote.year==undefined">- Kimi Räikkönen</p>
          <p v-else>- Kimi Räikkönen, {{ quote.year }}</p>
        </div>
      </router-link>
    </div>
</template>

<script>
import axios from 'axios'
export default ({
  setup () {
    
  },
  data() {
            return {
                quote: {}
            };    
  },
  created() {
           axios.get('https://kimiquotes.herokuapp.com/quote/'+this.$route.params.id )
                .then(response => {
                  this.quote = response.data;
                  console.log(this.quote)
                });
  }  
})
</script>

<style lang="scss">
.kimi-img { //falta mirar com queda la imatge en la cita en resolucio mes baixa
      display: block;
      width: 100%;
      height: 630px;
      object-fit: cover;
      z-index: 0;
}
.detail {
      position: absolute;
      left: 0;
      right: 0;
      bottom: 0;
      background-color: rgba(0, 0, 0, 0.6);
      padding: 16px;
      z-index: 1;
      h3 {
        color:#FFF;
        font-size: 32px;
        font-family: 'Gill Sans', 'Gill Sans MT', sans-serif;
      }
      p {
        color: #FFF;
        font-size: 28px;
        font-family: 'Gill Sans', 'Gill Sans MT', sans-serif;
      }
}
</style>