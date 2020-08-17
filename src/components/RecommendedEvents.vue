<template>
    <div class="main">
        <div class="container warp">
            <div v-show="!resultShow">
                <div v-for="(q, index) in questionsData" :key="index">
                    <div class="card tblock" v-show="index === questionIndex">
                        <div class="card-body" style="padding: 27px 0px 0px">
                            <h5 class="card-title" style="font-weight: 600;">{{q.question}}</h5>
                            <ul class="list-group list-group-flush">
                                <li class="list-group-item" style="text-align: left">
                                    <div>
                                        <div class="custom-control custom-radio" v-for="(a, aindex) in q.answers" :key="aindex">
                                            <input type="radio" :name="index" class="custom-control-input" :id="aindex+index+index" @click="answered(index, a.value)">
                                            <label class="custom-control-label" :for="aindex+index+index">{{a.text}}</label>
                                        </div>
                                    </div>
                                </li>
                                <li class="list-group-item">
                                    <div class="row">
                                        <div class="col-12 col-md-6 backBtn">
                                            <button class="btn btn-stylish" style="width: 80%;" @click="previousQuestion()">Назад</button>
                                        </div>
                                        <div class="col-12 col-md-6">
                                            <button class="btn btn-almbb-success" style="width: 80%;" @click="nextQuestion()">Вперед</button>
                                        </div>
                                    </div>
                                </li>
                            </ul>
                        </div>
                        <div class="card-footer text-muted">
                            {{(index+1)+'/'+questionsData.length}}
                        </div>
                    </div>
                </div>
            </div>
            <div v-show="resultShow">
                <div class="block">
                    <a href="#" class="btn btn-almbb-light" @click="resultShow = false" style="color: black">назад</a>
                    <canvas id="chart"></canvas>
                </div>
            </div>
        </div>
        <div class="footer"><Footer></Footer></div> 
    </div>
</template>

<script>
import Footer from './footer.vue'
import Swal from 'sweetalert2'
import Chart from 'chart.js'
export default {
    name: 'RecommendedEvents',
    components: { Footer },
    data(){
        return{
            resultShow: false,
            questionIndex: 0,
            results: [],
            questionsData: [
                {
                    question: 'First question',
                    answers: [
                        {text: 'answer1.1', value: 'it'},
                        {text: 'answer1.2', value: 'engineering'}
                    ]
                },
                {
                    question: 'Second question',
                    answers: [
                        {text: 'answer2.1', value: 'service'},
                        {text: 'answer2.2', value: 'it'}
                    ]
                },
            ],
        }
    },
    methods:{
        Search(){
            event.preventDefault()
            let it = 0
            let eng = 0
            let med = 0
            let art = 0
            for(let i = 1; i <= 20; i++){
               if(document.getElementById(i).checked){
                   if(document.getElementById(i).value == 'it'){
                       it += 1
                   }
                   else if(document.getElementById(i).value == 'engineering'){
                       eng += 1
                   }
                   else if(document.getElementById(i).value == 'medicine'){
                       med += 1
                   }
                   else if(document.getElementById(i).value == 'art'){
                       art += 1
                   }
               }
            }
            if(it + eng + med + art != 10){
                //alert('Ответьте на все вопросы')
                this.$swal({
                    icon: 'error',
                    text: 'Ответьте на все вопросы'
                });
            }
            else{
                //alert('Ваши ответы: ' + ' It: ' + it + ' Инженерия: ' + eng + ' Медицина: ' + med  + ' Арт: ' + art  + ' Перейдите на страницу мероприятий и выберите, что Вам подходит!')
                this.$swal({
                    icon: 'success',
                    title: 'Результаты',
                    text: 'Ваши ответы: ' + ' It: ' + it + ' Инженерия: ' + eng + ' Медицина: ' + med  + ' Арт: ' + art  + ' Перейдите на страницу мероприятий и выберите, что Вам подходит!',
                    showCancelButton: true,
                    confirmButtonColor: '#3085d6',
                    cancelButtonColor: '#d33',
                    confirmButtonText: 'Перейти',
                    cancelButtonText: 'Отмена'
                }).then((result) => {
                    if (result.value) {
                        document.location.href = '/all-events'
                    }
                });
                //document.location.href = '/all-events'
            }
        },
        previousQuestion(){
            if(this.questionIndex != 0) this.questionIndex--;
        },
        nextQuestion(){
            if(this.questionIndex+1 === this.questionsData.length){
                Swal.fire({
                    icon: 'question',
                    title: 'Завершить тест?',
                    showCancelButton: true,
                    confirmButtonColor: '#3085d6',
                    cancelButtonColor: '#d33',
                    confirmButtonText: 'Да',
                    cancelButtonText: 'Отмена'
                }).then((result) => {
                    // Swal.fire({
                    //     text: JSON.stringify(this.results)
                    // });
                    if (result.value) {
                        let it = 0
                        let engineering = 0
                        let service = 0
                        for (let item of this.results){
                            if(item == 'it') it++
                            else if(item == 'engineering') engineering++
                            else if(item == 'service') service++
                        }
                        this.results = []
                        this.results[0] = it
                        this.results[1] = engineering
                        this.results[2] = service
                        this.resultShow = true
                        this.showResult()
                    }
                });
            }
            else{
                this.questionIndex++;
            }
        },
        answered(index, value){
            this.results[index] = value
        },
        showResult(){
            let ctx = document.getElementById('chart')
            // console.log(this.results[2] +' '+ this.results[0] +' '+ this.results[1])
            let it = this.results[0]
            let engineering = this.results[1]
            let service = this.results[2]
            makeChart('doughnut', [service, it, 0, 0, engineering, 0], ctx)
            function makeChart(type, data, place){
                let dispalyLable = true
                if (document.documentElement.clientWidth < 768){
                    dispalyLable = false 
                }
                let myChart = new Chart(place, {
                    type: type,
                    data: {
                        labels: ['Сфера услуг', 'IT', 'Творчество и Дизайн', 'Строительство', 'Инженерные технологии', 'Транспорт и логистика'],
                        datasets: [{
                            label: '# of Votes',
                            data: data,
                            backgroundColor: [
                                'rgba(255, 99, 132, 0.5)',
                                'rgba(54, 162, 235, 0.5)',
                                'rgba(255, 206, 86, 0.5)',
                                'rgba(75, 192, 192, 0.5)',
                                'rgba(153, 102, 255, 0.5)',
                                'rgba(255, 159, 64, 0.5)'
                            ],
                            borderColor: [
                                'rgba(255, 99, 132, 1)',
                                'rgba(54, 162, 235, 1)',
                                'rgba(255, 206, 86, 1)',
                                'rgba(75, 192, 192, 1)',
                                'rgba(153, 102, 255, 1)',
                                'rgba(255, 159, 64, 1)'
                            ],
                            borderWidth: 1,
                            hoverBorderWidth: 5,
                        }]
                    },
                    options: {
                        legend: {
                            position: 'bottom',
                            display: dispalyLable,
                        },
                        responsive: true,
                    }
                });
                console.log(myChart)
            }
        },
        
    }
}            
</script>

<style scoped>
.warp{
    flex: 1 0 auto;
    padding-top: 110px !important;
    /* background-color: #fff; */
    padding: 0px 30px;
}
.footer{
    flex: 0 0 auto;
}
.main{
    display: flex;
	flex-direction: column;
}
.main{
    height: 100%;
    padding: 0px;
    min-height: 100vh;
    margin-bottom: 0px;
}

.formb{
    margin-top: 1.5em;
}
.chooseb{
    margin-bottom: 1em;
    font-size: 1.3em;
    text-align: left;
    padding: 0.2em;
    border: 1.5px solid #818181; /* Белая рамка */
    border-radius: 10px; /* Радиус скругления */
}
.tblock{
    position: fixed;
    width: 80%;
    top: 25%;
    left: 10%;
}
@media (max-width: 767px) {  
    .backBtn{
        margin-bottom: 0.5em
    }
} 
.block{
    background-color: #ffffff;
    width: 100%;
    height: 100%;
    border-radius: 4px;
    -webkit-box-shadow: 0px 2px 5px 1px rgba(34, 60, 80, 0.18);
    -moz-box-shadow: 0px 2px 5px 1px rgba(34, 60, 80, 0.18);
    box-shadow: 0px 2px 5px 1px rgba(34, 60, 80, 0.18);
    min-height: 300px;
    padding: 15px 20px;
}
</style>