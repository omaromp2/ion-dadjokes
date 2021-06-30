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
        <!-- <h3> {{ joke }} </h3>
        <br>
        <br>
        <ion-button color="primary" @click="getJoke()" >
          <ion-icon name="chat" ></ion-icon> Tell me a Joke
        </ion-button>
        <ion-button color="danger" @click="clear()"  >
          <ion-icon name="trash" ></ion-icon> clear
        </ion-button> -->
        <!-- <ion-icon name="trash" ></ion-icon> -->

        <!-- <ion-button color="primary" @click="getJoke()" >
          <ion-icon name="chat" ></ion-icon> Tell me a Joke
        </ion-button> -->

      <ion-refresher slot="fixed" @ionRefresh="recall($event)">
        <ion-refresher-content></ion-refresher-content>
      </ion-refresher>

        <div v-if="jokes.length == 0">
          No Jokes at the moment. Please pull my finger
        </div>

        <div v-else>
          <!-- {{ jokes[0].joke }} -->

          <ion-card v-for="joke in jokes" :key="joke" >
            <ion-card-header>
              <ion-card-title>
                <ion-icon name="glasses" size="large" ></ion-icon>
              </ion-card-title>
              <ion-card-subtitle>This is your joke</ion-card-subtitle>
            </ion-card-header>

            <ion-card-content>
              {{joke.joke}}

            </ion-card-content>

            <ion-button @click="shareJoke(joke.joke)" >Share <ion-icon name="share" ></ion-icon> </ion-button>

          </ion-card>

        </div>

      </div>

    </ion-content>
    <ion-fab vertical="top" horizontal="end" slot="fixed">
          <ion-fab-button>
            <ion-icon name="add"></ion-icon>
          </ion-fab-button>
          <ion-fab-list side="bottom">
            <ion-fab-button @click="getJoke()" ><ion-icon name="chat"></ion-icon></ion-fab-button>
            <ion-fab-button @click="clear()" ><ion-icon name="trash"></ion-icon></ion-fab-button>
          </ion-fab-list>
        </ion-fab>


  </ion-page>
</template>

<script >
import { IonContent, IonHeader, IonPage, IonTitle, IonToolbar, IonIcon, IonCard, IonCardContent, IonCardSubtitle, IonCardTitle, IonFab, 
  IonFabButton, IonFabList, IonRefresher, IonRefresherContent } from '@ionic/vue';
import { addIcons } from 'ionicons';
import { trashBinOutline, chatbubbleOutline, addCircleOutline, glassesOutline, shareSocialOutline } from 'ionicons/icons';
import { defineComponent } from 'vue';
import axios from 'axios';
import { Plugins } from "@capacitor/core";
const { Share } = Plugins;

export default defineComponent({
  name: 'Home',
  components: {
    IonContent,
    IonHeader,
    IonPage,
    IonTitle,
    IonToolbar, 
    IonIcon, 
    IonCard,
    IonCardContent,
    IonCardSubtitle,
    IonCardTitle, 
    IonFab, 
    IonFabButton, 
    IonFabList, 
    IonRefresher,
    IonRefresherContent,
  }, 

  created(){
    addIcons({
      'trash': trashBinOutline,
      'chat': chatbubbleOutline,
      'add' : addCircleOutline,
      'glasses': glassesOutline,
      'share': shareSocialOutline,
    });
    this.getJoke();
  },

  data() {
    return {
      // joke: ''
      jokes:[]
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

        // this.joke = resp.data.joke;

        // const joke = JSON.parse(resp.data); 
        
        this.jokes.push(resp.data); 

        
      })
      .catch(err => {
        console.log(err);
        
      });
    }, 
    clear(){
      // this.joke = '';
      this.jokes = []; 
    }, 
    
    recall(event){
      console.log("Hago refresh: ", event);

      // llamo la funcion para empujar el chiste 
      this.getJoke(); 

      setTimeout(()=> {
        console.log("acabe");
        event.target.complete();
      },2000);

    }, 

    async shareJoke(joke) {
      await Share.share({
        title: 'Share Joke', 
        text: joke, 
        dialogTitle: "Share with buddies",
      });
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