<template>
  <div class="d-flex justify-content-center">
      
    <div v-if="showScore">
        <b-card
        title="Sonuçlar"
        style="max-width: 20rem;"
        >
        {{questions.length}} sorudan {{score}} soruyu bildiniz.       <b-button @click="reloadPage">Tekrarlayın</b-button>

        </b-card>

    </div>
    <div class="card-q" v-else>
    <span v-if="!startQuiz">
         <b-card
         img-src="https://image.freepik.com/free-vector/clipboard-with-survey-chronometer_23-2147611001.jpg"
    img-alt="Image"
    img-top
    title="VueJS Örnek Quiz Uygulaması"
    style="max-width: 20rem;"
    class="mb-2"
        >
        <b-card-text>
            Azure DevOps üzerinde geliştirdiğimiz proje için basit bir VueJS Test Uygulaması tasarladım. 
        </b-card-text>
      <b-button @click="startQuizFunc()">Quiz'e Başlayın</b-button>
    </b-card>
    </span>
    <span v-else>
    <b-card
    title="Simple Quiz Application"
    style="max-width: 20rem;"
    class="mb-2"
  
  >
   <b-card-text>
      Question No.{{currentQuestion + 1}} of {{questions.length}}
    </b-card-text>
    <br>
   <b-progress
        variant="warning"
        :max="30"
        :value="countDown"
        height="4px"
      ></b-progress>
  
     <b-card-text>
      <span style="font-size: 40px;"><strong>{{countDown}} </strong></span>
    </b-card-text>
    <b-card-text>
      {{questions[currentQuestion].questionText}}
    </b-card-text>
    <div class="answer-section">
    <b-button :key="index" v-for="(option, index) in questions[currentQuestion].answerOptions" @click="handleAnswerClick(option.isCorrect)" class="ans-option-btn" variant="primary">{{option.answerText}}</b-button>
    </div>
  </b-card>
    </span>
  </div>
  </div>
</template>

<script>
export default {
    data(){
        return {
            currentQuestion: 0,
            showScore: false,
            score:0,
            countDown : 30,
            timer:null,
            startQuiz: false,

            questions : [
		{
			questionText: 'Çift yönlü ciltleme için hangisi kullanılır?',
			answerOptions: [
				{ answerText: 'v-on', isCorrect: false },
                { answerText: 'v-bind', isCorrect: false },
				{ answerText: 'v-model', isCorrect: true },
				{ answerText: 'v-if', isCorrect: false },
                
			
			],
		},
		{
			questionText: 'VueJS in yaratıcısı kimdir?',
			answerOptions: [
				{ answerText: 'Jeff Bezos', isCorrect: false },
				{ answerText: 'Elon Musk', isCorrect: false },
				{ answerText: 'Evan You', isCorrect: true },
				{ answerText: 'Tony Stark', isCorrect: false },
			],
		},
		{
			questionText: 'VueJS bir Arka Uç Uygulamasıdır? - Doğru Yanlış',
			answerOptions: [
				{ answerText: 'Doğru', isCorrect: false },
				{ answerText: 'Yanlış', isCorrect: true },
			],
		},
		{
			questionText: '2021 yılında kullanıma sunulmuş Vue Sürümü hangisidir?',
			answerOptions: [
				{ answerText: 'Vue 2', isCorrect: false },
				{ answerText: 'Vue 1', isCorrect: false },
				{ answerText: 'Vue 4', isCorrect: false },
				{ answerText: 'Vue 3', isCorrect: true },
			],
        },
        {
			questionText: 'Vue bir açık kaynak projemidir ? - Doğru, Yanlış',
			answerOptions: [
				{ answerText: 'Doğru', isCorrect: true },
				{ answerText: 'Yanlış', isCorrect: false },
			],
        },
        {
			questionText: 'Aşağıdakilerden hangisi tam bir JS yapısıdır?',
			answerOptions: [
				{ answerText: 'Vue', isCorrect: false },
				{ answerText: 'node', isCorrect: false },
				{ answerText: 'react', isCorrect: false },
				{ answerText: 'Angular', isCorrect: true },
			],
        },
        {
			questionText: 'Azure DevOpsun Kendi Kubernetes Clusteri Var mıdır? - Doğru, Yanlış',
			answerOptions: [
				{ answerText: 'Doğru', isCorrect: true },
				{ answerText: 'Yanlış', isCorrect: false },
			],
		},
    ],
    

        }
    },

    methods:{
        startQuizFunc(){
            this.startQuiz = true
            this.countDownTimer()
        },
          reloadPage(){
           window.location.reload()
        },
        handleAnswerClick(isCorrect){
            clearTimeout(this.timer);
            let nextQuestion = this.currentQuestion + 1;
            if(isCorrect){
                this.score = this.score + 1;
            }
            if(nextQuestion < this.questions.length){
            this.currentQuestion = nextQuestion;

            this.countDown = 30;
            this.countDownTimer();
            }
            else{
                this.showScore = true;
            }

        },
        countDownTimer() {
                if(this.countDown > 0) {
                    this.timer = setTimeout(() => {
                        this.countDown -= 1
                        this.countDownTimer()
                    }, 1000)
                }
                else{
                    this.handleAnswerClick(false)
                }
            }
    },
     created() {

        }
    
}
</script>

<style scoped>
.card{
    min-width: 100%;
    border-radius: 15px;
    padding: 20px;
    box-shadow: 10px 10px 42px 0px rgba(0, 0, 0, 0.75);
}
.card-q{
    min-width: 60%;
}
.ans-option-btn{
    min-width: 50%;
    font-size: 16px;
    color: #ffffff;
    align-items: center;
    cursor: pointer;
    margin-bottom: 5px;


}
.answer-section {
  width: 100%;
  display: flex;
  flex-direction: column;
  justify-content: space-between;
}
.timer-text {
  background: rgb(230, 153, 12);
  padding: 15px;
  margin-top: 20px;
  margin-right: 20px;
  border: 5px solid rgb(255, 189, 67);
  border-radius: 15px;
  text-align: center;
}

.card-img, .card-img-top {
    border-top-left-radius: calc(0.25rem - 1px);
    border-top-right-radius: calc(0.25rem - 1px);
    height: 350px;
}
/* .ans-option-btn {
  width: 100%;
  font-size: 16px;
  color: #ffffff;
  background-color: #252d4a;
  border-radius: 15px;
  display: flex;
  padding: 5px;
  justify-content: flex-start;
  align-items: center;
  border: 5px solid #234668;
  cursor: pointer;
} */

</style>
