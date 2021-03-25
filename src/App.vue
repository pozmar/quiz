
<template>

   <div class="container" id="app" :class="bgChange()">
      <div class="quiz" :class="isActive ==true? 'hide': 'block'">
         <h1>HARRY POTTER TEST</h1>
         <button class="start" @click="start()" :class="hasStarted ==true?'hide': 'inline'">Start</button>
         <div class="question" :class="hasStarted ==true?'block': 'hide'" >
            <h1>{{ questions[questionIndex].text }}</h1>
         </div>
         <div class="answers" :class="hasStarted ==true?'block': 'hide'">
            <button 
            class="btn" 
            :key="i" 
            v-for="(e, i) in questions[questionIndex].answers"
            @click="selection(i)">
            {{ e.text }}</button>
            
         </div>
         <div class="buttons" :class="hasStarted ==true?'block': 'hide'">
            
            <button 
            class="next" 
            v-on:click="next();"
            :class="questionIndex == questions.length-1? 'hide': 'inline'"
            :disabled="questionIndex== questions.length">
            Next</button>
            <button class="back" v-on:click="prev()" :disabled="questionIndex < 1">
               Back
            </button>
         </div>
      </div>
         <div class="results" :class="selectedAnswers.length == questions.length ? 'block': 'hide'" :key="selectedAnswers.length" >

            <button class="resultsBtn" @click="activate()">
               Vedi i risultati
            </button>
            <button class="restart" @click="restart()">
               Restart!
            </button>

            
         </div>
         <div class="score" :class="isActive == true ? 'active': 'hide'">
            {{ score()}} / {{questions.length}}
            <div class="text">
               {{resultsChange()}}
            </div>
         </div>
      
      

   </div>
  

</template>

<script>

export default {
  name: 'App',
  components: {
    
  },
  data(){
     return {
        questions: [
           {
              text: "Come si chiama il topo di Ron?",
              answers:[
                 {text:"Vinny"},
                 {text:"Crosta", correct:true},
                 {text:"Edvige"},
                 {text:"Thor"},
              ]
           },
           {
              text: "Con quale mezzo Harry e Ron arrivano a Hogwarts nell'episodio 2?",
              answers:[
                 {text:"Treno"},
                 {text:"Bicicletta"},
                 {text:"Scope"},
                 {text:"Automobile", correct:true},
              ]
           },
            {
              text: "Chi sono i genitori di Hermione?",
              answers:[
                 {text:"Due maghi"},
                 {text:"La madre è una strega, il padre un babbano"},
                 {text:"Sono entrambi babbani", correct:true},
                 {text:"Il padre è un mago, la madre una strega"},
              ]
           },
           {
              text: "Cos'è un horcrux?",
              answers:[
                 {text:"Un oggetto o una persona in cui un mago può nascondere un frammento della propria anima", correct:true},
                 {text:"Una pietra che consente di diventare immortali"},
                 {text:"Una maleficio proibito con il quale si può uccidere"},
                 {text:"Una bacchetta potente che permette a chi la possiede di essere il mago più pericoloso del mondo"},
              ]
           },
           {
              text: "Come si chiama la madre di Lord Voldemort?",
              answers:[
                 {text:"Merope", correct:true},
                 {text:"Ariana"},
                 {text:"Lily"},
                 {text:"Isotta"},
              ]
           },
           {
              text: "Quando è nato Lord Voldemort?",
              answers:[
                 {text:"12 ottobre 1926"},
                 {text:"30 novembre 1925"},
                 {text:"29 dicembre 1926"},
                 {text:"31 dicembre 1926", correct:true},
              ]
           },
           {
              text: "Qual è lo sport magico più famoso in America?",
              answers:[
                 {text:"Quidditch"},
                 {text:"Baseball"},
                 {text:"Quiffle"},
                 {text:"Quodpot", correct:true},
              ]
           },
           {
              text: "Che sembianze ha il patronus di Luna Lovegood?",
              answers:[
                 {text:"Cervo"},
                 {text:"Gatto"},
                 {text:"Lepre", correct:true},
                 {text:"Lontra"},
              ]
           },
           {
              text: "Come si chiama il figlio di Draco Malfoy?",
              answers:[
                 {text:"Scorpius", correct:true},
                 {text:"Salazar"},
                 {text:"Lucius"},
                 {text:"Severus"},
              ]
           },
           {
              text: "Che lavoro farà Harry da adulto?",
              answers:[
                 {text:"Ministro della magia"},
                 {text:"Auror", correct:true},
                 {text:"Preside di Hogwarts"},
                 {text:"Insegnante di Difesa contro le Arti Oscure"},
              ]
           },
           

        ],
        questionIndex: 0,
        disabled: false,
        selectedAnswers: [],
        isActive:false,
        hasStarted: false,
     }
  },
  methods:{
     selection(i){
        this.$set(this.selectedAnswers, this.questionIndex, i);
        
     },
     next(){
        if(this.questionIndex < this.selectedAnswers.length){
           this.questionIndex++;
        }
     },
     prev(){
        if(this.questions.length > 0){
           this.questionIndex--;
        }
     },
     score(){
        let score = 0;
        for(let i = 0; i < this.selectedAnswers.length; i++){
           if(typeof(this.questions[i].answers[this.selectedAnswers[i]] != "undefined") && this.questions[i].answers[this.selectedAnswers[i]].correct){
              score = score+1;
           }
        }
         
        return score;
      
     },
     activate(){
        this.isActive = !this.isActive;
        console.log(this.isActive);
     },
     start(){
        this.hasStarted =!this.hasStarted;
        
     },
     restart(){
        this.questionIndex=0;
        this.isActive=!this.isActive;
        console.log(this.isActive);
        this.selectedAnswers=[];
     },
    bgChange(){
       if(this.score() <= 4 && this.isActive ==true){
         
          return 'poor';
       }else if(this.score() > 4 && this.score() <= 7 && this.isActive ==true){
          return 'good';
       }else if(this.score() > 7 && this.isActive ==true){
          return 'great';
       }else{
          return '';
       } 
    },
    resultsChange(){
       if(this.score() <= 4 && this.isActive ==true){
         
          return 'Accidenti! Non sai proprio niente di magia. Il tuo posto è nello sgabuzzino di Dursley!';
       }else if(this.score() > 4 && this.score() <= 7 && this.isActive ==true){
          return 'Bravo! Se continui così, avrai un grande futuro davanti a te!';
       }else if(this.score() > 7 && this.isActive ==true){
          return 'Complimenti! Potresti essere uno dei più grandi maghi del mondo! Ps. Sicuro di non essere Silente?';
       }else{
          return '';
       } 
    }
    
  },
 
}
</script>

<style>
*{
   margin:0;
   padding:0;
   box-sizing: border-box;
   font-family: 'Poppins', sans-serif;
   font-size: 1.2rem;
}
:root{
   --bg-poor: url("https://www.dailydot.com/wp-content/uploads/116/66/8742a84f09c19e4c3fd928bd9d0b181f.jpg");
   --bg-good: url("https://static3.srcdn.com/wordpress/wp-content/uploads/2018/11/Harry-Potter-Ron-Ginny-Fred-George-Weasley-Gryffindor.jpg");
   --bg-great: url("http://images6.fanpop.com/image/photos/32700000/Hogwarts-Professors-Wallpaper-hogwarts-professors-32795958-1024-768.jpg");

}
body{
   background-image: url("https://observatoriodocinema.uol.com.br/wp-content/uploads/2020/06/harry-potter-e-o-enigma-do-principe-505-poster.jpg");
   background-size: cover;
   background-repeat: no-repeat;
}
.poor{
   background-image: var(--bg-poor);
}
.good{
   background-image: var(--bg-good);
}
.great{
   background-image: var(--bg-great);
}
.container{
   margin:100px auto;
   height: 100vh;
   width:70%;
   text-align:center;
   border-radius: 20px;
   box-shadow: 2px 5px 3px rgba(0,0,0, 0.25);
   background-size: cover;
   background-repeat: no-repeat;
   background-position: center;
}
.question, h1, .score{
   color: white;
   text-shadow: 3px 3px 2px black;
}
.text{
   font-size:2rem;
}
.question, .answers, .buttons{
   margin:30px;
   padding: 10px;
}
.start{
   padding: 10px 20px;
   border-radius:10px;
   border: 3px solid white;
   color:white;
   background-color:transparent;
   font-weight:bold;
}
.start:hover{
   cursor: pointer;
   border: 3px solid transparent;
   color:white;
   background-color:transparent;
}
.btn{
   padding:10px 15px;
   background-color: white;
   border-radius:10px;
   border:2px solid black;
   margin: 10px auto;
   display: block;
}
.btn:hover{
   cursor:pointer;
   background-image: linear-gradient(to left, #9298e4, #e56466 );
}
.btn:focus{
   background-image: linear-gradient(to left, #9298e4, #e56466 );
}
.buttons> *{
   margin: 0 10px;
   padding: 10px 15px;
   border-radius: 15px;
   border: 1px solid transparent;
}
.buttons button:hover:not([disabled]){
   cursor: pointer;
   background-image: linear-gradient(to left, lightgrey, transparent);
}
.buttons button:focus, .btn:focus{
   outline: none;
}
.results{
   margin: 20px 0;
   padding:40px;
   
}
.resultsBtn, .restart{
   padding:15px 20px;
   color: white;
   background-color: darkblue;
   font-weight: bold;
   border: 1px solid transparent;
   border-radius:10px;
   margin: 0 10px;
}
.resultsBtn:hover, .restart:hover{
   background-color: #0d0560;
}
.score{
   padding:40px;
   font-size: 2rem;
   font-weight: 1000;
   margin-top: 70px;
}
.hide{
   display: none;
}
.inline{
   display: inline;
}
.active, .block{
   display:block;
}

</style>
