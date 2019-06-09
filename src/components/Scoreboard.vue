<template>
  <div class="container">
    <div class="inningBox"><h1>Current Inning: {{ inning }}</h1></div>
    <div class="scoreboard">
      
      <table class="table">
        <thead>
          <th style="background-color: black"></th>
          <th v-for="item in 9" :key="item">{{ item  }}</th>
        </thead>
        <tbody>
          <td>Home</td>
          <td v-bind:class="{ active: this.inning === 1 }">{{ homeOne }}</td>
          <td v-bind:class="{ active: this.inning === 3 }">{{ homeTwo }}</td>
          <td v-bind:class="{ active: this.inning === 5 }">{{ homeThree }}</td>
          <td v-bind:class="{ active: this.inning === 7 }">{{ homeFour }}</td>
          <td v-bind:class="{ active: this.inning === 9 }">{{ homeFive }}</td>
          <td v-bind:class="{ active: this.inning === 11 }">{{ homeSix }}</td>
          <td v-bind:class="{ active: this.inning === 13 }">{{ homeSeven }}</td>
          <td v-bind:class="{ active: this.inning === 15 }">{{ homeEight }}</td>
          <td v-bind:class="{ active: this.inning === 17 }">{{ homeNine }}</td>
        </tbody>
        <tbody>
          <td>Away</td>
          <td v-bind:class="{ active: this.inning === 2 }">{{ awayOne }}</td>
          <td v-bind:class="{ active: this.inning === 4 }">{{ awayTwo }}</td>
          <td v-bind:class="{ active: this.inning === 6 }">{{ awayThree }}</td>
          <td v-bind:class="{ active: this.inning === 8 }">{{ awayFour }}</td>
          <td v-bind:class="{ active: this.inning === 10 }">{{ awayFive }}</td>
          <td v-bind:class="{ active: this.inning === 12 }">{{ awaySix }}</td>
          <td v-bind:class="{ active: this.inning === 14 }">{{ awaySeven }}</td>
          <td v-bind:class="{ active: this.inning === 16 }">{{ awayEight }}</td>
          <td v-bind:class="{ active: this.inning === 18 }">{{ awayNine }}</td>
        </tbody>
      </table>
    </div>

    <div class="outcome">
      <li v-for="swing in outcome.slice().reverse()" :key=swing>{{ swing }}</li>
    </div>
    
    <div class="action-box">
      <button @click.prevent="swing" class="button">Swing!</button>
    </div>

    <div class="count-box">
      <h1>count-box</h1>
      <div id = "countBox"> {{ balls }} | {{ strikes }} | {{ outs }}</div>
    </div>




  </div>

   
</template>

<script>
export default {
  name: 'scoreboard',
  data() {
  	return {
      currentInning: 1,
      balls: 0,
      strikes: 0,
      outs: 0,
      inning: 1,
      homeOne: 0, homeTwo: 0, homeThree: 0, homeFour: 0, homeFive: 0, homeSix: 0, homeSeven: 0, homeEight: 0, homeNine: 0,
      awayOne: 0, awayTwo: 0, awayThree: 0, awayFour: 0, awayFive: 0, awaySix: 0, awaySeven: 0, awayEight: 0, awayNine: 0,
      outcome: [],
      firstBase: false,
      secondBase: false,
      thirdBase: false
    }},
    methods: {
      swing() {
        
        // pick a number 1 to 100
        let number = (Math.floor(Math.random()*100)+1)
        
        //strike swinging
        if (number <=20) {
          
            //   let audio = new Audio('http://soundbible.com/grab.php?id=504&type=mp3');
            //   audio.play();

            this.outcome.push("Strike " + this.strikes + "!");
            this.strikes += 1;
            
            if(this.strikes === 3) {
                this.outs +=1;
                if (this.outs === 3) {
                    this.nextInning();
                }
            }
        } else if (number > 20 && number <= 25) {
            this.outcome.push("Foul Ball!");
            
            if (this.strikes < 2) {
                this.strikes += 1;
            };
            
        } else if (number > 25 && number <= 30) {
            this.outcome.push("Ground Out!");
            
            this.outs += 1;

            if (this.outs === 3) {
                this.nextInning();
            }
        } else if (number > 30 && number <= 35) {
            this.outcome.push("Fly Out!");
            
            this.outs += 1;

            if (this.outs === 3) {
                this.nextInning();
            }
        }
        // single
        else if (number > 50 && number <= 70) {
          
            this.outcome.push("Single!")

          // double
        } else if (number > 70 && number <= 85) {
            this.outcome.push("Double!")

          // triple
        } else if (number > 85 && number <= 95) {
            this.outcome.push("Triple!")

          // home run
        } else if (number > 95 && number <= 100) {
            this.outcome.push("Home Run!")
        }
      },
      nextInning() {
        this.inning += 1;
        this.balls = 0;
        this.strikes = 0;
        this.outs = 0;
        this.firstBase = false;
        this.secondBase = false;
        this.thirdBase = false;

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
