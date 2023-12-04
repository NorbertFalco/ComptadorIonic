<template>
  <ion-page>
    <ion-header :translucent="true">
      <ion-toolbar color="secondary">
        <ion-title>Time Fighter</ion-title>
        <ion-buttons slot="primary">
          <ion-button color="primary" fill="solid" @click="info">
            <ion-icon :icon="infoIcon"></ion-icon>
          </ion-button>
        </ion-buttons>
      </ion-toolbar>
    </ion-header>

    <ion-content :fullscreen="true">
      <ion-header>
        <ion-grid>
          <ion-row>
            <ion-col>
              <div class="ion-text-start" id="score">
                Your Score: {{ score }}
              </div>
            </ion-col>
            <ion-col>
              <div class="ion-text-end">
                Time Left: {{ timeLeft }}
              </div>
            </ion-col>
          </ion-row>
        </ion-grid>
      </ion-header>

      <div id="container" class="background-image">
        <!-- Use an image as the button -->
        <img id="tapMeButton" src="src/assets/images/tap_button2.png" alt="Tap Me" @click="tap" />
      </div>
    </ion-content>
  </ion-page>
</template>

<script>
import {
  alertController,
  IonButton,
  IonButtons, IonCol,
  IonContent, IonGrid,
  IonHeader,
  IonIcon,
  IonPage, IonRow,
  IonTitle,
  IonToolbar, toastController
} from '@ionic/vue';
import { defineComponent } from "vue";
import { informationCircleOutline } from "ionicons/icons";
import { createAnimation } from "@ionic/vue";

const INITIAL_TIME = 5

export default defineComponent({
  name: 'Home',
  components:  {
    IonContent,
    IonHeader,
    IonPage,
    IonTitle,
    IonToolbar,
    IonButtons,
    IonButton,
    IonIcon,
    IonGrid,
    IonRow,
    IonCol
  },
  setup () {
    return {
      infoIcon: informationCircleOutline,
      started: false,
      counterInterval: null
    }
  },
  data () {
    return {
      score: 0,
      timeLeft: INITIAL_TIME
    }
  },
  watch: {
    timeLeft: function(newTimeLeft) {
      if (newTimeLeft <= 0) {
        console.log('FINAL')
        this.started = false
        this.timeLeft = INITIAL_TIME
        clearInterval(this.counterInterval)
        this.showResult()
        this.score = 0
      }
    }
  },
  methods: {
    bounce () {
      const animation = createAnimation()
      animation
          .addElement(document.getElementById('tapMeButton'))
          .duration(2000)
          .fromTo('transform', 'scale(2.0)', 'scale(1.0)')
      animation.play();
    },
    blink () {
      const animation = createAnimation()
      animation.addElement(document.getElementById('score'))
          .duration(500)
          .fromTo('opacity', '0', '1')
      animation.play();
    },
    async info() {
      const alert = await alertController
          .create({
            header: 'Time Fighter 1.0',
            subHeader: 'Creat per Norbert Falcó',
            message: 'Podeu trobar el codi font a: <a href="https://github.com/NorbertFalco/ComptadorIonic">',
            buttons: ['OK'],
          });
      await alert.present();
    },
    tap() {
      this.bounce();
      this.blink()
      this.score++;
      if (!this.started) {
        this.counterInterval = setInterval(() => {
          this.timeLeft--;
        }, 1000);
        this.started = true;
      }
    },
    async showResult() {
      //TOAST
      const toast = await toastController.create({
        color: 'dark',
        duration: 2000,
        message: `Time's up. You score was ${this.score}`,
        showCloseButton: true
      });

      await toast.present();
    }
  }
});
</script>

<style scoped>
.background-image {
  background-image: url('src/assets/images/timeFighter1.jpeg');
  background-size: contain;
  background-position: center;
  background-repeat: no-repeat;
  width: 100vw;
  height: 100vh;
  position: fixed;
  top: 0;
  left: 0;
}

#container {
  text-align: center;
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
}

#tapMeButton {
  margin-top: 40vh;
  cursor: pointer;
  width: 150px;
  height: auto;
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
