<template>
    <div class="main">
        <div class="container warp">
            <div class="ChosenClassList">
                <div class="row align-items-end">
                    <div class="col-12 col-lg-3" style="font-weight: bold; font-size: 1.3em; margin-top: 0.5em">Направления: </div>
                    <div class="col-9 col-lg-8" id="classList" style="text-align: left; font-size: 1.3em;">
                        <span class="badge badge-pill badge-primary" id="programmingbadge" style="display: none; margin-right: 0.5em">IT <a class="deleteDirection" @click="delActive('programming')">&times;</a></span>
                        <span class="badge badge-pill badge-primary" id="engineeringbadge" style="display: none; margin-right: 0.5em">Инженерия <a class="deleteDirection" @click="delActive('engineering')">&times;</a></span>
                        <span class="badge badge-pill badge-primary" id="servicebadge" style="display: none;">Сфера услуг <a class="deleteDirection" @click="delActive('service')">&times;</a></span>
                        <span class="badge badge-pill badge-primary" id="artbadge" style="display: none;">Дизайн <a class="deleteDirection" @click="delActive('art')">&times;</a></span>
                        <span class="badge badge-pill badge-primary" id="buildbadge" style="display: none;">Сфера строительства <a class="deleteDirection" @click="delActive('build')">&times;</a></span>
                        <span class="badge badge-pill badge-primary" id="transportbadge" style="display: none;">Сфера транспорта <a class="deleteDirection" @click="delActive('transport')">&times;</a></span>
                        <span class="badge badge-pill badge-primary" id="soft-skillsbadge" style="display: none;">Soft-skills <a class="deleteDirection" @click="delActive('soft-skills')">&times;</a></span>
                    </div>
                    <div class="col-3 col-lg-1"><a class="btn btn-almbb-small btn-red-orange" style="border-radius: 50%; color: white" @click="showChoseModal()"><i class="fas fa-plus"></i></a></div>
                </div>
                <!-- <hr>
                <div class="row">
                    <div class="col-12 col-md-10" style="padding-right: 0px">
                        <input type="text" class="form-control" style="border-radius: 50px;">
                    </div>
                    <div class="col-12 col-md-2" style="padding: 0px"><button class="btn btn-blue btn-almbb-small">Найти</button></div>
                </div> -->
            </div>


            <div id="my_modal" class="modal">
                <div class="modal_content">
                    <span class="close_modal_window" @click="closeChoseModal()"><i class="fas fa-times"></i></span>
                    <div class="row">
                        <div class="col-12" style="font-weight: bold; font-size: 1.5em;">Выберите направления</div>
                    </div>
                    <div class="selectGroup">
                        <div class="row item" id="programming-item" @click="Chose('programming')">IT</div>
                        <div class="row item" id="engineering-item" @click="Chose('engineering')">Инженерия</div>
                        <div class="row item" id="service-item" @click="Chose('service')">Сфера услуг</div>
                        <div class="row item" id="art-item" @click="Chose('art')">Дизайн</div>
                        <div class="row item" id="build-item" @click="Chose('build')">Сфера строительства</div>
                        <div class="row item" id="transport-item" @click="Chose('transport')">Сфера транспорта</div>
                        <div class="row item" id="soft-skills-item" @click="Chose('soft-skills')">Soft-skills</div>
                    </div>
                </div>
            </div>

            <div v-if="data.length == 0"><h3>Нет мероприятий по выбранным направлениям</h3></div>
            <div class="card" v-for="(item, index) in data" :key="item.value">
                <div class="card-header">{{item.date}}</div>
                <div class="card-body">
                    <h5 class="card-title">{{item.name}}</h5>
                    <p class="card-text"><i class="far fa-clock"></i> {{item.time}}</p>
                    <p class="card-text"><i class="far fa-user"></i> {{item.places}}</p>
                    <p class="card-text">Тип: {{item.type}}</p>
                    <div class="row">
                        <div class="col-12 col-md-6">
                            <button class="btn btn-outline-almbb-info"  @click="add(item, index)">Собираюсь посетить</button>
                        </div>
                        <div class="col-12 col-md-6 gotoevents">
                            <a :href="item.link" class="btn btn-blue" @click="setScroll()">Перейти к мероприятию</a>
                        </div>
                    </div>
                </div>
            </div>
        </div>
        <div class="footer"><Footer></Footer></div>
    </div>
</template>

<script>
import needle from 'needle'
import Vue from 'vue';
// import Swal from 'sweetalert2'
import Footer from './footer.vue'
export default {
    name: 'AllEvents',
    components: { Footer },
    data(){
        return{
            data: [],
            allEventsCategoriesData: {},
        }
    },
    beforeMount(){
        if(localStorage.eventsChosen == undefined){
            let eventsChose = {
                programming: false,
                engineering: false,
                service: false,
                build: false,
                transport: false,
                art: false,
                'soft-skills': false,
            }
            localStorage.eventsChosen = JSON.stringify(eventsChose)
        }
        // console.log(JSON.parse(localStorage.eventsChosen))
    },
    mounted(){
        setTimeout(() => {
            window.scrollTo({
                top: this.$store.getters.allEventsScroll,
                behavior: 'auto'
            });
        }, 500);
        this.$store.commit('SET_ALL_EVENTS_SCROLL', 0)

        let eventsChose = JSON.parse(localStorage.eventsChosen)
        let email = this.$store.getters.email
        let SessionID = this.$store.getters.SessionID 
        needle('post',this.$store.state.serverIp+'/api/getAllEvents', {email: email, sessionid: SessionID}, {"json": true})
        .then(res => {
            for (let key in eventsChose){
                if( eventsChose[key] == true ){
                    document.getElementById(key+'badge').style.display = 'inline-block'
                    document.getElementById(key+'-item').classList.add('selected')
                    this.data = this.data.concat(res.body[key])
                }
            }
            this.allEventsCategoriesData = {
                engineering: res.body.engineering,
                programming: res.body.programming,
                service: res.body.service,
                build: res.body.build,
                transport: res.body.transport,
                art: res.body.art,
                'soft-skills': res.body['soft-skills'],
            }
        })
        .catch(function(err) {
            console.log(err)
        })
        
        // let eventsChose = JSON.parse(localStorage.eventsChosen)
        // for (let key in eventsChose){
        //     if( eventsChose[key] == true ){
        //         document.getElementById(key+'badge').style.display = 'inline-block'
        //         document.getElementById(key+'-item').classList.add('selected')
        //     }
        // }
    },
    methods:{
        add(event, eventPlace){
            let email = this.$store.getters.email
            let SessionID = this.$store.getters.SessionID
            if(email != ''){
                // delete event.places
                // if(this.$route.path == '/it-events') event.mainType = 'programming'
                // else if(this.$route.path == '/service-events') event.mainType = 'service'
                // else event.mainType = 'engineering'
                if(event.id == '/events/allEvents/it') event.mainType = 'programming'
                else if(event.id == "/events/allEvents/inj") event.mainType = 'engineering'
                else if(event.id == "/events/allEvents/service") event.mainType = 'service'
                else if(event.id == "/events/allEvents/transport") event.mainType = 'transport'
                else if(event.id == "/events/allEvents/build") event.mainType = 'build'
                else if(event.id == "/events/allEvents/art") event.mainType = 'art'
                else if(event.id == "/events/allEvents/soft-skills") event.mainType = 'soft-skills'
                needle('post',this.$store.state.serverIp+'/api/checkedEventsUpdate', {email: email, events: event, sessionid: SessionID}, {"json": true})
                .then(res => {
                    if(res.body == '310'){
                        document.cookie = "email=" + ";expires=Thu, 01 Jan 1970 00:00:01 GMT"
                        document.cookie = "SessionID=" + ";expires=Thu, 01 Jan 1970 00:00:01 GMT"
                        window.location.href = '/login'
                    }
                    else{
                        //alert('Мероприятие успешно добавлено')
                        Vue.swal({
                            icon: 'success',
                            text: 'Мероприятие успешно добавлено',
                            showConfirmButton: false,
                            timer: 1500,
                            timerProgressBar: true,
                        }).then(()=>{
                            this.$delete(this.data, eventPlace)
                            if(event.id == '/events/allEvents/it'){
                                this.$delete(this.allEventsCategoriesData['programming'], eventPlace)
                            }
                            else if(event.id == "/events/allEvents/inj"){
                                let tmp = JSON.parse(localStorage.eventsChosen)
                                if(tmp['programming'] == true){
                                    this.$delete(this.allEventsCategoriesData['engineering'], eventPlace-this.allEventsCategoriesData['programming'].length)
                                }
                                else{
                                    this.$delete(this.allEventsCategoriesData['engineering'], eventPlace)
                                }
                            }
                            else if(event.id == "/events/allEvents/service"){
                                let tmp = JSON.parse(localStorage.eventsChosen)
                                if(tmp['programming'] == false && tmp['engineering'] == false){
                                    this.$delete(this.allEventsCategoriesData['service'], eventPlace)
                                }
                                else if(tmp['programming'] == true && tmp['engineering'] == false){
                                    this.$delete(this.allEventsCategoriesData['service'], eventPlace-this.allEventsCategoriesData['programming'].length)
                                }
                                else if(tmp['programming'] == false && tmp['engineering'] == true){
                                    this.$delete(this.allEventsCategoriesData['service'], eventPlace-this.allEventsCategoriesData['engineering'].length)
                                }
                                else if(tmp['programming'] == true && tmp['engineering'] == true){
                                    this.$delete(this.allEventsCategoriesData['service'], eventPlace-this.allEventsCategoriesData['engineering'].length-this.allEventsCategoriesData['programming'].length)
                                }
                            }
                        });
                    }
                })
                .catch(function(err) {
                    console.log(err)
                })
            }
            else{
                this.$swal({
                    icon: 'error',
                    title: 'Вы не авторизованы!',
                    showCancelButton: true,
                    confirmButtonColor: '#3085d6',
                    cancelButtonColor: '#d33',
                    confirmButtonText: 'Авторизоваться',
                    cancelButtonText: 'Отмена'
                }).then((result) => {
                    if (result.value) {
                        window.location.pathname = "/login"
                    }
                });
                //window.location.pathname = "/login"
            }
        },
        setScroll(){
            document.cookie = "allEventsScroll=" + window.pageYOffset
        },
        delActive(elem){
            document.getElementById(elem+'badge').style.display = 'none'
            document.getElementById(elem+'-item').classList.remove('selected')
            let tmp = JSON.parse(localStorage.eventsChosen)
            tmp[elem] = false
            localStorage.eventsChosen = JSON.stringify(tmp)
            // location.reload()
            let eventsChose = JSON.parse(localStorage.eventsChosen)
            this.data = []
            for (let key in eventsChose){
                if( eventsChose[key] == true ){
                    document.getElementById(key+'badge').style.display = 'inline-block'
                    document.getElementById(key+'-item').classList.add('selected')
                    this.data = this.data.concat(this.allEventsCategoriesData[key])
                }
            }
        },

        Chose(elem){
            if(document.getElementById(elem+'badge').style.display == 'none'){
                document.getElementById(elem+'badge').style.display = 'inline-block'
                document.getElementById(elem+'-item').classList.add('selected')
                let tmp = JSON.parse(localStorage.eventsChosen)
                tmp[elem] = true
                localStorage.eventsChosen = JSON.stringify(tmp)
                let eventsChose = JSON.parse(localStorage.eventsChosen)
                this.data = []
                for (let key in eventsChose){
                    if( eventsChose[key] == true ){
                        document.getElementById(key+'badge').style.display = 'inline-block'
                        document.getElementById(key+'-item').classList.add('selected')
                        this.data = this.data.concat(this.allEventsCategoriesData[key])
                    }
                }
            }
            else{
                document.getElementById(elem+'badge').style.display = 'none'
                document.getElementById(elem+'-item').classList.remove('selected')
                let tmp = JSON.parse(localStorage.eventsChosen)
                tmp[elem] = false
                localStorage.eventsChosen = JSON.stringify(tmp)
                let eventsChose = JSON.parse(localStorage.eventsChosen)
                this.data = []
                for (let key in eventsChose){
                    if( eventsChose[key] == true ){
                        document.getElementById(key+'badge').style.display = 'inline-block'
                        document.getElementById(key+'-item').classList.add('selected')
                        this.data = this.data.concat(this.allEventsCategoriesData[key])
                    }
                }
            }
        },
        showChoseModal(){
            let modal = document.getElementById("my_modal");
            modal.style.display = "block";
        },
        closeChoseModal(){
            let modal = document.getElementById("my_modal");
            modal.style.display = "none"
            // location.reload()
            // let eventsChose = JSON.parse(localStorage.eventsChosen)
            // this.data = []
            // for (let key in eventsChose){
            //     if( eventsChose[key] == true ){
            //         document.getElementById(key+'badge').style.display = 'inline-block'
            //         document.getElementById(key+'-item').classList.add('selected')
            //         this.data = this.data.concat(this.allEventsCategoriesData[key])
            //     }
            // }
        }
    }
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
    padding-top: 110px !important;
}
.main{
    background-color: #eef5ff;
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
.card-body h5{
    font-weight: bold;
}
.card-header{
    font-weight: bold;
}
@media (max-width: 767px) {  
    .gotoevents{
        margin-top: 0.5em
    }
}

.ChosenClassList{
    background-color: #ffffff;
    width: 100%;
    height: 100%;
    border-radius: 4px;
    -webkit-box-shadow: 0px 0px 5px 2px rgba(34, 60, 80, 0.2);
    -moz-box-shadow: 0px 0px 5px 2px rgba(34, 60, 80, 0.2);
    box-shadow: 0px 0px 5px 2px rgba(34, 60, 80, 0.2);
    min-height: 100px;
    padding: 15px 20px;
    margin: 1em 0em;
}

.badge-primary {
    color: #fff !important;
    background-color: #4285f4 !important;
}
.badge-pill {
    padding-right: .6em;
    padding-left: .6em;
    border-radius: 10rem;
}
.badge {
    -webkit-box-shadow: 0 2px 5px 0 rgba(0,0,0,0.16), 0 2px 10px 0 rgba(0,0,0,0.12);
    box-shadow: 0 2px 5px 0 rgba(0,0,0,0.16), 0 2px 10px 0 rgba(0,0,0,0.12);
}

.deleteDirection{
    cursor: pointer;
    font-size: 1.1em;
    -moz-user-select: none;
    -webkit-user-select: none;
    -ms-user-select: none;
    -o-user-select: none;
    user-select: none;
}
.deleteDirection:hover{
    color: #ff4444;
    text-transform: uppercase;
}

.modal {
    display: none;
    position: fixed;
    left: 0;
    top: 0;
    width: 100%;
    height: 100%;
    overflow: auto;
    background-color: rgba(0,0,0,0.6);
    z-index: 9999;
}
.modal .modal_content {
    background-color: #fefefe;
    padding: 20px;
    border: 1px solid #888;
    z-index: 99999;
}

@media (max-width: 575px) {
    .modal .modal_content {
        width: 100%;
        min-height: 60%;
        margin: 30% 0;
    }
}
@media (min-width: 576px) { 
    .modal .modal_content {
        margin: 15% auto;
        width: 60%;
        min-height: 300px;
    }
}

.modal .modal_content .close_modal_window {
    color: #aaa;
    float: right;
    font-size: 1.3em;
    font-weight: bold;
    cursor: pointer;
    margin-top: -20px;
    margin-right: -10px;
}
.modal .modal_content .close_modal_window:hover {
    color: #ff4444;
}
.selectGroup .item{
    font-size: 1.1em;
    padding: 0.5em;
    margin: 0.5em 0px;
}
.selectGroup .item:hover{
    background: rgba(236, 236, 236, 0.466);
}
.selectGroup .item.selected{
    background: #3F729B;
    color: white;
}
</style>