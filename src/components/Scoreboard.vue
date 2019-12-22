<template>
  <div class="container">
    <div class="inningBox"><h1>Current Inning: {{ currentInning }}</h1></div>
    <div class="scoreboard">
      
      <table class="table">
        <thead>
          <th style="background-color: black"></th>
          <th v-for="item in 9" :key="item">{{ item  }}</th>
        </thead>
        <tbody>
          <td>Away</td>
          <td v-bind:class="{ active: this.inning === 0 }">{{ score[0] }}</td>
          <td v-bind:class="{ active: this.inning === 2 }">{{ score[2]}}</td>
          <td v-bind:class="{ active: this.inning === 4 }">{{ score[4] }}</td>
          <td v-bind:class="{ active: this.inning === 6 }">{{ score[6] }}</td>
          <td v-bind:class="{ active: this.inning === 8 }">{{ score[8] }}</td>
          <td v-bind:class="{ active: this.inning === 10 }">{{ score[10] }}</td>
          <td v-bind:class="{ active: this.inning === 12 }">{{ score[12] }}</td>
          <td v-bind:class="{ active: this.inning === 14 }">{{ score[14] }}</td>
          <td v-bind:class="{ active: this.inning === 16 }">{{ score[16] }}</td>
        </tbody>
        <tbody>
          <td>Home</td>
          <td v-bind:class="{ active: this.inning === 1 }">{{ score[1] }}</td>
          <td v-bind:class="{ active: this.inning === 3 }">{{ score[3] }}</td>
          <td v-bind:class="{ active: this.inning === 5 }">{{ score[5] }}</td>
          <td v-bind:class="{ active: this.inning === 7 }">{{ score[7] }}</td>
          <td v-bind:class="{ active: this.inning === 9 }">{{ score[9] }}</td>
          <td v-bind:class="{ active: this.inning === 11 }">{{ score[11] }}</td>
          <td v-bind:class="{ active: this.inning === 13 }">{{ score[13] }}</td>
          <td v-bind:class="{ active: this.inning === 15 }">{{ score[15] }}</td>
          <td v-bind:class="{ active: this.inning === 17 }">{{ score[17] }}</td>
        </tbody>
      </table>
    </div>

    <div class="outcome">
      <li v-for="swing in outcome.slice().reverse()" :key=swing>{{ swing }}</li>
    </div>
    
    <div class="action-box">
      <button @click.prevent="swing" class="button">PITCH</button>
    </div>

    <div class="count-box">
      <h1>count-box</h1>
      <div id = "countBox"> {{ balls }} | {{ strikes }} | {{ outs }}</div>
    </div>

    <div class="field-box">
        <ul>
            <li v-if="firstBase">Runner on First</li>
            <li v-if="secondBase">Runner on Second</li>
            <li v-if="thirdBase">Runner on Third</li>
        </ul>
    </div>
  </div>
</template>

<script>
export default {
    name: 'scoreboard',
    
    data() {
        return {
            balls: 0,
            strikes: 0,
            outs: 0,
            inning: 0,
            score: [0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0],
            inningScore: 0,
            outcome: [],
            firstBase: false,
            secondBase: false,
            thirdBase: false
        }},

    computed: {
        currentInning() {
            return Math.floor(this.inning/2 + 1);
        }
    },
    
    methods: {

        swing() {
            // pick a number 1 to 100
            let number = (Math.floor(Math.random()*100)+1)
            
            //strike swinging
            if (number <=40) {

                this.strike();

            } else if (number > 40 && number <= 50) {
                
                this.ball();

            } else if (number > 50 && number <= 60) {
                
                this.groundOut();

            } else if (number > 60 && number <= 72) {
                
                this.flyOut();

            } else if (number > 72 && number <= 82) {
                
                this.foulBall();
                
            } else if (number > 82 && number <= 89) {
            
                this.single();
            
            } else if (number > 90 && number <= 94) {
                
                this.double();

            } else if (number > 94 && number <= 98) {
                
                this.triple();
            
            } else if (number > 98 && number <= 100) {

                this.homeRun();
                
            }
        },

        strike() {
            this.strikes += 1;
            this.outcome.push("Strike " + this.strikes + "!");
            
            if(this.strikes === 3) {
                this.strikes = 0;
                this.outs +=1;
                if (this.outs === 3) {
                    this.nextInning();
                } else {
                    this.nextBatter();
                }
            }
        },

        ball() {
            this.balls += 1;
            this.outcome.push("Ball " + this.balls + "!");

            if (this.balls === 4) {
                this.advanceRunners();

                this.nextBatter();
            }
        },

        foulBall() {
            this.outcome.push("Foul Ball!");
                
            if (this.strikes < 2) {
                this.strikes += 1;
            }
        },

        single() {
            this.outcome.push("Single!")
            
            if (this.thirdBase) {
                this.addRun();
                this.thirdBase = false;
            }

            if (this.secondBase) {
                this.thirdBase = true;
                this.secondBase = false;
            }

            if (this.firstBase) {
                this.secondBase = true;
                this.firstBase = false;
            }
            
            this.firstBase = true;
            this.nextBatter();
        },

        double() {
            this.outcome.push("Double!");

            if (this.thirdBase) {
                this.thirdBase = false;
                this.addRun();
            }

            if (this.secondBase) {
                this.secondBase = false;
                this.addRun();
            }

            if (this.firstBase) {
                this.firstBase = false;
                this.thirdBase = true;
            }
            
            this.secondBase = true;
            this.nextBatter();
        },

        triple() {
            this.outcome.push("Triple!")

            //advance runners
            if (this.firstBase) {
                this.firstBase = false;
                this.addRun();
            }

            if (this.secondBase) {
                this.secondBase = false;
                this.addRun();
            }

            if (this.thirdBase) {
                this.thirdBase = false;
                this.addRun();
            }

            this.thirdBase = true;
            this.nextBatter();
        },

        homeRun() {
            let audio = new Audio(require('../assets/Ball+Hit+Cheer.mp3'));
            
            audio.play();
            
            this.outcome.push("Home Run!")

            //advance runners
            if (this.firstBase) {
                this.firstBase = false;
                this.addRun();
            }

            if (this.secondBase) {
                this.secondBase = false;
                this.addRun();
            }

            if (this.thirdBase) {
                this.thirdBase = false;
                this.addRun();
            }
            
            this.addRun();
            
            this.nextBatter();

        },

        nextInning() {
            this.inning += 1;
            this.inningScore = 0;
            this.balls = 0;
            this.strikes = 0;
            this.outs = 0;
            this.firstBase = false;
            this.secondBase = false;
            this.thirdBase = false;
        },
        nextBatter() {
            this.balls = 0;
            this.strikes = 0;
        },
        addRun() {
            this.inningScore += 1;
            this.score[this.inning] = this.inningScore;
        },
        advanceRunners() {
            if (this.firstBase) {
                if(this.secondBase) {
                    if(this.thirdBase) {
                        // bases loaded
                        this.firstBase = true;
                        this.secondBase = true;
                        this.thirdBase = true;

                        this.addRun();
                    }
                    // first and second
                    this.firstBase = true;
                    this.secondBase = true;
                    this.thirdBase = true;
                }

                // runner on first
                this.firstBase = true;
                this.secondBase = true;
            }

            this.firstBase = true;
        }

}}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
  .scoreboard {
    border: 1px solid black;
    border-radius: 5px;
  }

  .container {
  width: 100%;
  height: 400px;
  padding: 20px;
  font-family: Helvetica;
  box-sizing: border-box;
  text-align: center;
}

.outcome {
  width: 100%;
  height: 6em;
  overflow: scroll;
  box-sizing: border-box;
  /* border: 2px solid black; */
}
.active {
  background-color: black;
  color: white;
}
.action-box {
  width: 50%;
  height: 50%;
  box-sizing: border-box;
  /* border: 2px solid black; */
  float: left;
}
.count-box {
  width: 50%;
  height: 50%;
  box-sizing: border-box;
  /* border: 2px solid black; */
  float: left;
}

.button {
  background-color: whitesmoke;
  color: grey;
  width: 10rem;
  height: 4rem;
  /* border: .5px solid black; */
  border-radius: 10px;
  margin: 1rem;
  text-decoration: none; 
}

.button:hover {
    cursor: hand;
    background-color: blue;
}

button:focus { 
  outline: none; 
}

.table {
  padding: 0px;
  margin: 20px auto;
}
th {
  text-align: center;
  /* border: 1px solid black; */
  width: 20px;
}

td {
  text-align: center;
  /* border: 1px solid black; */
  width: 20px;
}

</style>
