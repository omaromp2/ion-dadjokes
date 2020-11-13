<template>
  <ion-page>
    <ion-header :translucent="true">
      <ion-toolbar>
        <ion-title>DadJokes</ion-title>
      </ion-toolbar>
    </ion-header>
    
    <ion-content :fullscreen="true">
      <ion-header collapse="condense">
        <ion-toolbar>
          <ion-title size="large">Dad Jokes</ion-title>
        </ion-toolbar>
      </ion-header>
    
      <div id="container">
        <h3> {{ joke }} </h3>
        <br>
        <br>
        <ion-button color="primary" @click="getJoke()" >
          <ion-icon name="chat" ></ion-icon> Tell me a Joke
        </ion-button>
        <ion-button color="danger" @click="clear()"  >
          <ion-icon name="trash" ></ion-icon> clear
        </ion-button>

      </div>
    </ion-content>
  </ion-page>
</template>

<script lang="ts">
import { IonContent, IonHeader, IonPage, IonTitle, IonToolbar, IonIcon } from '@ionic/vue';
import { addIcons } from 'ionicons';
import { trashBinOutline, chatbubbleOutline } from 'ionicons/icons';
import { defineComponent } from 'vue';
import axios from 'axios';

export default defineComponent({
  name: 'Home',
  components: {
    IonContent,
    IonHeader,
    IonPage,
    IonTitle,
    IonToolbar, 
    IonIcon
  }, 

  created(){
    addIcons({
      'trash': trashBinOutline,
      'chat': chatbubbleOutline
    });
  },

  data() {
    return {
      joke: ''
    }
  }, 
  methods:{
    getJoke(){

      // const header = {'accept': 'application/json'}; 

      axios.get('https://icanhazdadjoke.com/',{
        headers:{
          'accept': 'application/json'
        }
      })
      .then(resp => {
        // console.log(resp);

        this.joke = resp.data.joke;
        
      })
      .catch(err => {
        console.log(err);
        
      });
    }, 
    clear(){
      this.joke = '';
    }
  }

});
</script>

<style scoped>
#container {
  text-align: center;
  
  position: absolute;
  left: 0;
  right: 0;
  top: 50%;
  transform: translateY(-50%);
}

#container strong {
  font-size: 20px;
  line-height: 26px;
}

#container p {
  font-size: 16px;
  line-height: 22px;
  
  color: #8c8c8c;
  
  margin: 0;
}

#container a {
  text-decoration: none;
}
</style>