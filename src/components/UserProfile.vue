<template>
    <div class="main">
        <div class="container warp">
            <div class="person_info row" >
                <div class="col-12 col-md-4" style="padding: 30px 15px 15px">
                    <div class="photo pblock">
                        <div class="avatar">
                            <img src="./../assets/noavatar.jpg" alt="">
                        </div>
                        <button @click="GoToEditor()" class="btn btn-almbb-light" style="width: 100%; margin-top: 30px; color: black;">Редактировать</button>
                    </div>
                </div>
                
                <div class="col-12 col-md-8" style="padding: 30px 15px 15px">
                    <div class="pblock">
                        <div class="row">
                            <div class="col-9"><h4 style="margin-bottom: 0px;">{{person_name}}</h4></div>
                            <div class="col-3">
                                <small class="text-muted" style="vertical-align: bottom;">online</small>
                            </div>
                        </div>
                        <hr style="margin-top: 3px;">
                        <div class="info">
                            <div class="row">
                                <div class="col-6 text-muted">Роль: </div>
                                <div class="col-6">{{person_role}}</div>
                            </div>
                            <div class="row">
                                <div class="col-6 text-muted">Дата рождения: </div>
                                <div class="col-6">{{person_date}}</div>
                            </div>
                            <div class="row">
                                <div class="col-6 text-muted">Школа: </div>
                                <div class="col-6">{{person_school}}</div>
                            </div>
                            <div class="row">
                                <div class="col-6 text-muted">Класс и символ: </div>
                                <div class="col-6">{{person_grade}}</div>
                            </div>
                        </div>
                        <div style="border-top: 1px solid lightgray; margin-top: 150px; margin-right: -20px; margin-left: -20px">
                            <div class="row justify-content-center container" style="padding: 0px 20px;">
                                <div class="col-4 col-md-2" style="height: 100%; padding: 15px 0px; margin: 0px 15px">
                                    <div @click="PersonFriends()" style="font-size: 22px; color: #2a5885; padding-bottom: 3px; line-height: 21px; cursor: pointer;">22</div>
                                    <div style="line-height: 15px; color: #828282;">друга</div>
                                </div>
                                <div class="col-5 col-md-3" style="height: 100%; padding: 15px 0px; margin: 0px 15px">
                                    <div @click="PersonEvents()" style="font-size: 22px; color: #2a5885; padding-bottom: 3px; line-height: 21px; cursor: pointer;">30</div>
                                    <div style="line-height: 15px; color: #828282;">мероприятий</div>
                                </div>
                            </div>
                        </div>
                    </div>
                </div>
            </div>
            <div class="person_statistic row" style="padding: 15px 15px">
                <div class="pblock">
                    <h4>Статистика:</h4>
                    <div style="text-align: center;"><i class='fa fa-spinner fa-pulse fa-3x' :id='person_email + "x"' style="display: inline-block;"></i></div>
                    <form :id="'form' + person_email" class="row">
                        <!-- <input class="radio" :name="'donaught' + person_email" type="radio" value="donaught" checked @click="changeInfo(person_email, 'donaught')"> Круговая диаграмма
                        <input class="radio" :name="'bar' + person_email" type="radio" value="bar" @click="changeInfo(person_email, 'bar')"> Столбчатая диаграмма -->
                        <div class="form-check col-12 col-md-6">
                            <input id="donaughtCheck" class="radio" :name="'donaught' + person_email" type="radio" value="donaught" checked @click="changeInfo(person_email, 'donaught')">
                            <label class="form-check-label" for="donaughtCheck">
                               Круговая диаграмма
                            </label>
                        </div>
                        <div class="form-check col-12 col-md-6">
                            <input class="radio" id="barCheck" :name="'bar' + person_email" type="radio" value="bar" @click="changeInfo(person_email, 'bar')">
                            <label class="form-check-label" for="barCheck">
                                Столбчатая диаграмма
                            </label>
                        </div>
                        <!-- <input class="radio" :name="'full' + person_email" type="radio" value="full" @click="changeInfo(person_email)"> Полная статистика -->
                    </form>
                    <div class="chart-container" :id="'chartDiv' + person_email" style="display: none;"><canvas :id="'chart' + person_email"></canvas></div>
                    <div class="chart-container" :id="'chartDiv2' + person_email" style="display: none;"><canvas :id="'chart2' + person_email"></canvas></div>
                    <!-- <div :id="'chartDiv3' + person_email" style="display: none;">
                        <div v-if="studentEvents[person_email] != undefined && studentEvents[person_email].length == 0"><h3>Нет мероприятий</h3></div>
                        <div class="card" v-for="item3 in studentEvents[person_email]" :key="item3.value">
                            <div class="card-header">{{item3.date}}</div>
                            <div class="card-body">
                                <div class="row">
                                    <h5 class="card-title col-11">{{item3.name}}</h5>
                                </div>
                            </div>
                        </div>
                    </div> -->
                </div>
            </div>
            <div class="person_events" style="padding: 15px 0px 30px">
                <div class="pblock">
                    <h4>Ближайшие мероприятия:</h4>
                    <div class="row">
                        <div class="col-12 col-md-5">
                            <h5>Направеление:</h5> 
                        </div>
                        <div class="col-12 col-md-7">
                            <select class="custom-select custom-select-sm mb-3 events" style="width: 100%;">
                                <option value="" selected>Все</option>
                                <option value="">IT</option>
                                <option value="">Инженерия</option>
                                <option value="">Сфера услуг</option>
                            </select>
                        </div>
                    </div>
                </div>
            </div>
            <div class="person_events_archive" style="padding: 15px 0px 30px">
                <div class="pblock">
                    <h4>Архив мероприятий:</h4>
                    <div class="row">
                        <div class="col-12 col-md-5">
                            <h5>Направеление:</h5> 
                        </div>
                        <div class="col-12 col-md-7">
                            <select class="custom-select custom-select-sm mb-3 events" style="width: 100%;">
                                <option value="" selected>Все</option>
                                <option value="">IT</option>
                                <option value="">Инженерия</option>
                                <option value="">Сфера услуг</option>
                            </select>
                        </div>
                    </div>
                    <div v-if="studentEvents[person_email] != undefined && studentEvents[person_email].length == 0"><h3>Нет мероприятий</h3></div>
                    <div class="card" v-for="item3 in studentEvents[person_email]" :key="item3.value" style="margin-bottom: 1em">
                        <div class="card-header">{{item3.data.date}}</div>
                        <div class="card-body">
                            <div class="row">
                                <h5 class="card-title col-12">{{item3.data.name}}</h5>
                            </div>
                            <p class="card-text"><i class="far fa-clock"></i> {{item3.data.time}}</p>
                            <p class="card-text"><i class="far fa-user"></i> {{item3.data.places}}</p>
                            <p class="card-text">Тип: {{item3.data.type}}</p>
                            <p v-if="item3.status == 'checked'" class="card-text" style="color: green;">Участие подтверждено</p>
                            <p v-if="item3.status == 'on_moderate'" class="card-text" style="color: #0099CC;">Участие проверяется модератором</p>
                            <p v-if="item3.status == 'not_checked'" class="card-text" style="color: red;">Участие не подтверждено</p>
                        </div>
                    </div>
                </div>
            </div>
        </div>
        <div class="footer"><Footer></Footer></div> 
    </div>
</template>

<script>
import Footer from './footer.vue'
import Vue from 'vue';
import Chart from 'chart.js'
export default {
    name: 'UserProfile',
    components: { Footer },
    data(){
      return{
        id: this.$route.params.id,
        email: this.$store.getters.email,
        SessionID: this.$store.getters.SessionID,
        person_name: ' ',
        person_date: ' ',
        person_grade: ' ',
        person_school: ' ',
        person_email: '',
        studentEvents: [],
        person_role: '',
      }
    },
    beforeMount(){ 
        fetch(this.$store.state.serverIp+'/api/getIdInformation', {
            method: 'POST',
            headers: {id: this.id, email: this.email, sessionid: this.SessionID},
        })
        .then(response => {
            console.log("res", response)
            return response.json()
        })
        .then(data => {
            if(data == '310'){
                // document.cookie = "email=" + ";expires=Thu, 01 Jan 1970 00:00:01 GMT"
                // document.cookie = "SessionID=" + ";expires=Thu, 01 Jan 1970 00:00:01 GMT"
                // window.location.reload()
                console.log("err 310")
            }
            if(data.email == undefined) window.location.pathname = "/login"
            this.person_name = data.name + ' ' + data.surname
            this.person_email = data.email
            this.person_role = data.role
            let roles = {
                user: 'Пользователь',
                student: 'Ученик',
                teacher: 'Учитель',
                schooladmin: 'Школьный Администратор',
                admin: 'Администратор',
            }
            if(this.person_role == 'school-admin'){
                this.person_role = roles['schooladmin']
            }
            else{
                this.person_role = roles[this.person_role]
            }
            let personDate = data.age
            personDate = personDate.split('-')
            personDate = new Date(personDate[0], personDate[1]-1, personDate[2]);
            var options = {
                year: 'numeric',
                month: 'long',
                day: 'numeric',
            };
            this.person_date = personDate.toLocaleString("ru", options)
            if(data.class_number != undefined || data.simvol != undefined){
                this.person_grade = data.class_number + ' ' + data.simvol
            }
            else{
                this.person_grade = 'Не указаны'
            }
            if(data.school != undefined){
                this.person_school = data.school
            }
            else{
                this.person_school = 'Не указана'
            }
            this.showInfo()
        })
        .catch(err => {
            console.log(err)
        })
    },
    methods: {
        PersonEvents(){
            //надо сделать переход на мероприятия этого человека, пока тут alert
            Vue.swal({
                icon: 'error',
                text: 'Функция временно не доступна!',
                showConfirmButton: false,
                timer: 1500,
                timerProgressBar: true,
            });
        },
        PersonFriends(){
            //надо сделать переход на список друзей этого человека, пока тут alert
            Vue.swal({
                icon: 'error',
                text: 'Функция временно не доступна!',
                showConfirmButton: false,
                timer: 1500,
                timerProgressBar: true,
            });
        },
        GoToEditor(){
            this.$router.push({ path: `/user-profile-edit` })
        },
        showInfo(){
            let SessionID = this.$store.getters.SessionID
            let user_email = this.email
            fetch(this.$store.state.serverIp+'/api/getCheckedEvents', {
                method: 'get',
                headers: {email: user_email, studEmail: this.person_email, sessionid: SessionID},
            })
            .then(response => {
                console.log("res", response)
                return response.json()
            })
            .then(datan => {
                let statistics = datan.stat
                this.$set(this.studentEvents, this.person_email, datan.checkedEvents)
                console.log(this.studentEvents)
                let ctx = document.getElementById('chart' + this.person_email)
                let ctx2 = document.getElementById('chart2' + this.person_email)
                makeChart('doughnut', [statistics.service, statistics.programming, 0, 0,  statistics.engeniring, 0], ctx)
                makeChart('bar', [statistics.service, statistics.programming, 0, 0,  statistics.engeniring, 0], ctx2)
                document.getElementById(this.person_email + "x").style.display = 'none'
                document.getElementById('chartDiv' + this.person_email).style.display = 'block'
            })
            .catch(err => {
                console.log(err)
            })
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
        changeInfo(email, chart){
            let form = document.getElementById('form' + email)
            if(chart == 'donaught'){
                form['bar' + email].checked = false
                // form['full' + email].checked = false
                document.getElementById('chartDiv' + email).style.display = 'block'
                document.getElementById('chartDiv2' + email).style.display = 'none'
                // document.getElementById('chartDiv3' + email).style.display = 'none'
            }
            else if(chart == 'bar'){
                form['donaught' + email].checked = false
                // form['full' + email].checked = false
                document.getElementById('chartDiv' + email).style.display = 'none'
                document.getElementById('chartDiv2' + email).style.display = 'block'
                // document.getElementById('chartDiv3' + email).style.display = 'none'
            }
            // else{
            //     form['donaught' + email].checked = false
            //     form['bar' + email].checked = false
            //     document.getElementById('chartDiv' + email).style.display = 'none'
            //     document.getElementById('chartDiv2' + email).style.display = 'none'
            //     document.getElementById('chartDiv3' + email).style.display = 'block'
            // }
        },
    },
}
</script>

<style scoped>
.warp{
    flex: 1 0 auto;
}
.footer{
    flex: 0 0 auto;
}
.main{
    display: flex;
	flex-direction: column;
    padding-top: 95px !important;
}
.main{
    height: 100%;
    padding: 30px 0px 0px;
    min-height: 100vh;
    margin-bottom: 0px;
}
.card{ 
    margin-top: 10px !important;
}
.card-body{ 
    text-align: left !important;
}
.card-header{
    font-weight: bold;
}
.card-body h5{ 
    font-weight: bold;
}
.card-title{
    text-align: center;
}

.pblock{
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
.avatar{
    /* object-fit: fill; fill or contain */
    display: block;
    /* max-width: 100%;
    max-height: 100%; */
    max-width: 310px;
    max-height: 310px;
    min-width: 310px;
    min-height: 310px;
}
.avatar img{
    max-width: 100%;
    min-height: 310px;
    max-height: 310px;
}


.info{
    text-align: left;
}

</style>