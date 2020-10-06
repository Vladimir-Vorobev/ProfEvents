<template>
    <div class="main">
        <div class="warp">
            <!-- <div class="person_info row" >
                <div class="col-12 col-md-4" style="padding: 30px 15px 15px">
                    <div class="photo pblock">
                        <div class="avatar" v-lazy-container="{ selector: 'img' }">
                            <img width="310" height="310" :data-src="avatar.file" alt="">
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
                            <div class="row" v-if="person_role == 'Пользователь' || person_role == 'Ученик' || person_role == 'Учитель'">
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
                    </form>
                    <div class="chart-container" :id="'chartDiv' + person_email" style="display: none;"><canvas :id="'chart' + person_email"></canvas></div>
                    <div class="chart-container" :id="'chartDiv2' + person_email" style="display: none;"><canvas :id="'chart2' + person_email"></canvas></div>
                </div>
            </div>
            <div class="person_events" style="padding: 15px 0px 30px">
                <div class="pblock">
                    <h4>Ближайшие мероприятия:</h4>
                    <div v-if="studentEventsUpcoming[person_email] != undefined && studentEventsUpcoming[person_email].length == 0"><h3>Нет мероприятий</h3></div>
                    <div class="card" v-for="item3 in studentEventsUpcoming[person_email]" :key="item3.value" style="margin-bottom: 1em">
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
            <div class="person_events_archive" style="padding: 15px 0px 30px">
                <div class="pblock">
                    <h4>Архив мероприятий:</h4>
                    <div v-if="studentEventsOld[person_email] != undefined && studentEventsOld[person_email].length == 0"><h3>Нет мероприятий</h3></div>
                    <div class="card" v-for="item3 in studentEventsOld[person_email]" :key="item3.value" style="margin-bottom: 1em">
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
            </div> -->
            <div class="row justify-content-center" style="max-width: 100%; margin: 0px 0px;">
                <div class="col-4 col-lg-1"><img src="/user_profile.png" style="max-height: 53px;"></div>
                <div class="col-8 col-lg-3"><button class="btn btn-rounded-outline-elegant-dark btn-almbb-small">Редактировать</button></div>
                <div class="col-12 col-lg-7 nameandstatus" style="padding-right: 0em;">
                    <div class="name">Имя Фамилия</div>
                    <div class="status">Online</div>
                </div>
            </div>
            <br>
            <div class="row information" style="max-width: 100%; margin: 0px 0px;">
                <div class="col-12 col-lg-5">
                    <div class="photo ml-auto mr-auto">
                        <div class="fototext">Фото</div>
                    </div>
                </div>
                <div class="col-12 col-lg-7 info mt-auto mb-auto">
                    <div class="row ml-auto mr-auto">
                        <div class="list col-6 col-lg-6">
                            <p class="item">Роль:</p>
                            <p class="item">Дата рождения:</p>
                            <p class="item">Школа:</p>
                            <p class="item">Класс и символ:</p>
                        </div>
                        <div class="listr col-6 col-lg-6">
                            <p class="itemr">Пользователь</p>
                            <p class="itemr">20 января 2020 г.</p>
                            <p class="itemr">ГБОУ «Школа № 1329»</p>
                            <p class="itemr">9 «Б»</p>
                        </div>
                    </div>
                    <div class="frandevents">
                        <div class="itemcir">
                            <div class="cir">22</div>
                            <div class="l">друга</div>
                        </div>
                        <div class="itemcir">
                            <div class="cir">30</div>
                            <div class="l">мероприятий</div>
                        </div>
                    </div>
                </div>
            </div>
            <div class="blocksheader">
                <div class="mt-auto mb-auto">Статистика</div>
            </div>
            <div class="stat">
                
            </div>
            <div class="blocksheader">
                <div class="mt-auto mb-auto">Ближайшие мероприятия</div>
            </div>
            <div class="nearevents">
                
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
        studentEventsOld: [],
        studentEventsUpcoming: [],
        person_role: '',
        avatar: '',
      }
    },
    beforeMount(){ 
        fetch(this.$store.state.serverIp+'/api/getIdInformation', {
            method: 'POST',
            headers: {id: this.id, email: this.email, sessionid: this.SessionID},
        })
        .then(response => {
            // console.log("res", response)
            return response.json()
        })
        .then(data => {
            // console.log(data)
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
                if(data.class_number != undefined && data.simvol == undefined){
                    this.person_grade = data.class_number + ' -'
                }
                else if(data.class_number == undefined && data.simvol != undefined){
                    this.person_grade = '- ' + data.simvol
                }
                else{
                    this.person_grade = data.class_number + ' ' + data.simvol
                }
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
            this.showAvatar()
        })
        .catch(err => {
            //window.location.pathname = "/login"
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
        showAvatar(){
            fetch(this.$store.state.serverIp+'/api/getAvatar', {
                method: 'POST',
                headers: {email: this.email, dopemail: this.person_email, sessionid: this.SessionID},
            })
            .then(response => {
                // console.log("res", response)
                return response.json()
            })
            .then(data => {
                // console.log(data)
                if(data == '310'){
                    document.cookie = "email=" + ";expires=Thu, 01 Jan 1970 00:00:01 GMT"
                    document.cookie = "SessionID=" + ";expires=Thu, 01 Jan 1970 00:00:01 GMT"
                    window.location.reload()
                    window.location.pathname = "/login"
                    // console.log('310 getAvatar')
                }
                else{
                    this.avatar = data.data
                    // console.log(data.data)
                }
            })
        },
        showInfo(){
            let SessionID = this.$store.getters.SessionID
            let user_email = this.email
            fetch(this.$store.state.serverIp+'/api/getCheckedEvents', {
                method: 'get',
                headers: {adminemail: user_email, studemail: this.person_email, sessionid: SessionID, type: 'studentAll'},
            })
            .then(response => {
                // console.log("res", response)
                return response.json()
            })
            .then(datan => {
                // console.log(datan)
                let statistics = datan.stat
                // console.log(statistics)
                this.$set(this.studentEvents, this.person_email, datan.checkedEvents)
                // console.log(this.studentEvents)

                let dates = []
                for(let event of this.studentEvents[this.person_email]){
                    let eventDay = ''
                    let eventMonth = ''
                    let s = false
                    for(let i = 0; i < event.data.date.length; i++){
                        if(event.data.date[i] != ' ' && s == false) eventDay += event.data.date[i]
                        else if(s == true) eventMonth += event.data.date[i]
                        else{
                            s = true
                            continue
                        }
                    }
                    dates.push({day: eventDay, month: eventMonth})
                }
                let months = {January: 0, February: 1, March: 2, April: 3, May: 4, June: 5, July: 6, August: 7, September: 8, October: 9, November: 10, December: 11,}
                for(let event of dates){
                    event.month = months[event.month]
                }
                let now = new Date();
                let nowYear = now.getFullYear()
                for(let [index, event] of dates.entries()){
                    let date = new Date(nowYear, event.month, event.day)
                    dates[index] = date
                }
                let eventsOld = []
                let eventsUpComing = []
                for(let [index, event] of dates.entries()){
                    if(event < now){
                        eventsOld.push(this.studentEvents[this.person_email][index])
                    }
                    else{
                        eventsUpComing.push(this.studentEvents[this.person_email][index])
                    }
                }
                this.$set(this.studentEventsOld, this.person_email, eventsOld)
                this.$set(this.studentEventsUpcoming, this.person_email, eventsUpComing)

                let ctx = document.getElementById('chart' + this.person_email)
                let ctx2 = document.getElementById('chart2' + this.person_email)
                makeChart('doughnut', [statistics.service, statistics.programming, 0, 0, statistics.engineering, 0], ctx)
                makeChart('bar', [statistics.service, statistics.programming, 0, 0, statistics.engineering, 0], ctx2)
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
                new Chart(place, {
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
                // console.log(myChart)
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
    background-color: white;
}

.nameandstatus{
    background-color: #c60000;
    border-radius: 50px;
    text-align: left;
    vertical-align: middle;
    font-family: 'PT Mono', monospace;
    font-weight: bold;

    display: table;
}
.name{
    display: table-cell;

    font-size: 1.6em;
    color: white;
    padding-left: 1em;
}
.status{
    display: table-cell;
    vertical-align:middle;
    text-align: center;

    color: black;
    background-color: white;
    border: 2px black solid;
    border-radius: 50px;
}
.information{
    min-height: 450px;
}
.photo{
    background-color: lightgray;
    border-radius: 100%;
    /* min-height: 450px;
    min-width: 450px; /* ломает все на мобилках */
    /* max-width: 450px; */

    display: table;
}
@media (max-width: 767px) {  
    .photo{
        min-height: 310px;
        min-width: 310px; /* ломает все на мобилках */
        max-width: 310px;
    }
}
@media (min-width: 768px) and (max-width: 1024px) {  
    .photo{
        min-height: 410px;
        min-width: 410px; /* ломает все на мобилках */
        max-width: 410px;
    }
}
@media (min-width: 1025px) {  
    .photo{
        min-height: 450px;
        min-width: 450px; /* ломает все на мобилках */
        max-width: 450px;
    }
}
.fototext{
    display: table-cell; 
    vertical-align: middle; 
    text-align: center; 
    font-size: 3em;
    color: white;
    font-family: 'PT Mono', monospace;
}
.list{
    border-right: 3px solid #c60000;
}
.item{
    font-family: 'PT Mono', monospace;
    text-align: left;
}
.itemr{
    font-family: 'PT Mono', monospace;
    text-align: left;
    font-weight: bold;
    color: black;
}
@media (max-width: 767px) {  
    .item{
        font-size: 0.9em;
    }
    .itemr{
        font-size: 1em;
    }
    .info{
        padding: 1em 2px;
    }
    .listr{
        padding-left: 15px !important;
    }
}
@media (min-width: 768px) {  
    .item{
        font-size: 1em;
    }
    .itemr{
        font-size: 1.1em;
    }
    .listr{
        padding-left: 30px !important;
    }
}
.frandevents{
    min-width: 100%;
    max-height: 60px;
    min-height: 60px;
    display: block;
    padding: 0px 15px;
}
@media (max-width: 768px) {  
    .frandevents{
        text-align: center;
    }
}
@media (min-width: 769px) {  
    .frandevents{
        text-align: left;
    }
}
.itemcir{
    min-height: 60px;
    min-width: 50px;
    max-width: 50px;
    display: inline-block;
    margin-right: 1em;
    text-align: center;
}
.itemcir .cir{
    font-size: 30px;
    min-width: 50px;
    max-width: 50px;
    min-height: 50px;
    border-radius: 100%;
    border: #c60000 3px solid;
    color: #c60000;
}
.itemcir .l{
    font-size: 15px;
}
.blocksheader{
    background-color: #efefef;
    font-family: 'PT Mono', monospace;
    font-weight: bold;
    vertical-align: middle;
    color: black;
}
@media (max-width: 1024px) {  
    .blocksheader{
        min-height: 140px;
        max-height: 140px;
        line-height: 140px;
        margin: 1.5em 0px 1em;
        font-size: 2em;
    }
}
@media (min-width: 1025px) {  
    .blocksheader{
        min-height: 240px;
        max-height: 240px;
        line-height: 240px;
        margin: 2.5em 0px 1em;
        font-size: 2.5em;
    }
}
.stat{
    min-height: 500px;
    max-height: 500px;
}
.nearevents{
    min-height: 500px;
}
/* .card{ 
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
    display: block;
    max-width: 310px;
    max-height: 310px;
    min-width: 310px;
    min-height: 310px;
}
.avatar img{
    max-width: 100%;
    min-height: 310px;
    max-height: 310px;
    object-fit: cover;
}


.info{
    text-align: left;
} */
</style>