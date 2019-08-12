<template>
  <v-layout
    column
    justify-center
    align-center
  >
    <v-flex
      xs12
      sm8
      md6
    >
        <h1>{{word}}</h1>
        <h2 v-if="!word">Click the microphone and say a word.</h2>
        <v-text-field color="success" :loading="loading" disabled v-if="loading" placeholder="Listening..."></v-text-field>

        <v-btn
            absolute
            dark
            fab
            bottom
            right
            color="pink"
            @click="listen"
        >
            <v-icon>mdi-microphone</v-icon>
        </v-btn>
    </v-flex>
  </v-layout>
</template>

<script>

export default {
  data: () => ({
    word: null,
    loading: false,
    recognition: new (window.SpeechRecognition || window.webkitSpeechRecognition || window.mozSpeechRecognition || window.msSpeechRecognition)()
  }),
  components: {
  },
  methods: {
      listen: function(){
          this.recognition.stop();
          this.recognition.start();
      },
      onResult: function(event){
        this.word = event.results[0][0].transcript;
      },
      onStart: function(event){
          this.loading = true;
      },
      onEnd: function(event){
          this.loading = false;
      }
  },
  mounted: function () {
    this.recognition.lang = 'en-US';
    this.recognition.interimResults = false;
    this.recognition.maxAlternatives = 5;
    this.recognition.onresult = this.onResult;
    this.recognition.onstart = this.onStart;
    this.recognition.onend = this.onEnd;
  }
}
</script>


<style scoped>
h1 {
    font-size: 15vmin;
}
</style>
