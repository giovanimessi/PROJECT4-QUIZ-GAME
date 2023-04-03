<template>
  <meta charset="utf-8">
  <answers/>
  <ScoreBoard :contador='this.contador' :pontosComputador = 'this.pontosComputador'/>
<div>


  <template v-if="this.question">
 <h1 v-html=" this.question "></h1>

<template v-for="item in answers" :key="item.answers">
<input 
:disabled="this.answersSubmmited"
type="radio"
 name ="options"
  :value="item"
  v-model="this.choose_answer"
  >
<label>{{item }}</label><br>

</template>
<button v-if="!this.answersSubmmited"  @click="this.submitAnswer()"  
class="send" type="button">Send</button>
</template>
<section v-if="this.answersSubmmited" class="result">
    <h4 v-if="this.choose_answer == this.correctAnswer" 
    v-html=" '&#9989; A resposta é ' + this.correctAnswer  + ' Parabéns você acertou.'">
     
    </h4>
      <h4 v-else
      v-html = "'&#10060; I´m sorry,you picked the wrong answer.The coorect is ' + this.correctAnswer  + '.'"></h4>
    
    <button  @click="this.getNewQuestion()" class="send" type="button">
      Próxima questao
      
    </button>
</section>
</div>
</template>

<script>
  import ScoreBoard from '@/components/ScoreBoard.vue'
export default {
  name: 'App',
  components: {
  ScoreBoard
},

  data(){
    return {
      question:undefined,
      incorrectAnswers: undefined,
      correctAnswer: undefined,
      choose_answer:undefined,
      answersSubmmited:false,
      contador: 0,
      pontosComputador: 0,



    }
  },
  computed:{
    answers(){
      var answers = JSON.parse(JSON.stringify(this.incorrectAnswers));
      answers.splice(Math.round(Math.random() * answers.length) , 0, this.correctAnswer);
 
      return answers;
    }

  },
  methods:{
    submitAnswer(){
      if(!this.choose_answer){
        alert("Escolha uma opcão");
      }else{
        this.answersSubmmited = true;
        if(this.choose_answer == this.correctAnswer){
          this.contador++;
   
        }else{
         this.pontosComputador++;
        }
      }
    },
    getNewQuestion(){

      this.answersSubmmited = false;
      this.choose_answer = undefined;
      this.question = undefined;

       this.axios
      .get("https://opentdb.com/api.php?amount=1&category=18")
      .then((res) => {
        this.question =res.data.results[0].question;
        this.incorrectAnswers =res.data.results[0].incorrect_answers;
        this.correctAnswer =res.data.results[0].correct_answer;

              //console.log(res.data.results[0])
      })
      .catch((error) => {
              console.log(error);
      });
      }


  },
  created(){
    this.getNewQuestion();
  }
 
}
</script>

<style lang="scss">
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin: 60px auto;
  max-width: 960px;

  input[type-radio]{
  margin: 12px 4px;
}
button.send{
  margin-top:12px;
  height: 40px;
  min-width: 120px;
  padding: 0 16px;
  color:#fff;
  background-color: #1867c0;
  border: 1px solid #1867c0;
  cursor: pointer;

}
}

</style>
