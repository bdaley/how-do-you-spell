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
        <h2 v-if="!words">Click the microphone and say a word.</h2>
        <v-text-field color="success" :loading="loading" disabled v-if="loading" placeholder="Listening..."></v-text-field>
        <template v-if="words && !loading">
            <v-card v-for="(result, index) in words" v-bind:key="index" class="mx-auto mb-8" min-width="50vw" max-width="75vw">
                <v-card-title class="display-1 text--primary py-4">
                    <v-btn class="mr-3" fab depressed dark small color="primary lighten-2" @click="say(result.transcript)"><v-icon>mdi-play</v-icon></v-btn>
                    {{result.transcript}}
                </v-card-title>
            </v-card>
        </template>
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
    words: null,
    synth: window.speechSynthesis,
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
      say: function(word){
          this.synth.speak( new SpeechSynthesisUtterance(word) );
      },
      onResult: function(event){
          console.log(event.results[0]);
          this.words = event.results[0];
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

</style>
