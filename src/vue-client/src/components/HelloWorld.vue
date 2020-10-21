<template>
  <v-container>
    <v-row class="text-center">
      
      <v-col class="mb-12" cols="12">
        <h1 class="display-2 font-weight-bold mb-3">
          Logs Debug
        </h1>
      </v-col>

      <v-col class="mb-12" cols="12">
        <v-card
          elevation="16"
          max-width="400"
          class="mx-auto"
        >
          <!-- ERROR: put several virtual scroll -->
          <v-virtual-scroll
            :items="messages"
            height="300"
            item-height="0"
          >
            
              <v-list-item 
                v-for="message in messages"
                :key="message.lastEventId">

                <v-list-item-content>
                  <v-list-item-title>
                    {{ message.data }}
                  </v-list-item-title>
                </v-list-item-content>
              </v-list-item>
              <v-divider></v-divider>
          </v-virtual-scroll>
        </v-card>
      </v-col>

      <v-col cols="12" class="mb-9">
        <div style="min-height: 4px;">
          <v-progress-linear
          v-model="knowledge"
          height="25"
        >
          <strong>{{ Math.ceil(knowledge) }}%</strong>
        </v-progress-linear>
        </div>
      </v-col>

      <v-col class="mb-12">
        <div style="min-height: 4px;">
          <v-btn block depressed
            color="primary"
            elevation="2"
            @click.native="setupStream()"
          >RUN</v-btn>
        </div>
      </v-col>

      
    </v-row>
  </v-container>
</template>

<script lang="ts">
  export default {
    data () {
      return {
        messages: [],
        knowledge: 20, 
      }
    },

    methods: {
      setupStream() {
        const eventSource = new EventSource('http://localhost:5000/randomNamedEvents');

        eventSource.onmessage = (e) => {
          console.log(e);
        }

        eventSource.addEventListener('meme', (e) => {
          console.log(e);
          this.messages.push(e);
        });

        /** 
         * Other messages of server 
        eventSource.addEventListener('coinToss', (e) => {
          console.log(e);
          this.messages.push(e);
        });
        
        eventSource.addEventListener('dieRoll', (e) => {
          console.log(e);
          this.messages.push(e);
        });

        eventSource.addEventListener('catFact', (e) => {
          console.log(e);
          this.messages.push(e);
        });
        
        */
        eventSource.addEventListener('error', (e) => {
          console.log('got an error');
          console.log(e);
        });
      }
    },
  }
</script>
