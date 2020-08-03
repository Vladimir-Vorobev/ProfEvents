<template>
    <div class="main">
        <div class="container warp">
            <div class="card">
                <div class="card-body row">
                    <div class="col-md-5 col-12"> <p class="card-text" style="font-weight: bold; font-size: 1.3em">Сортировка мероприятий: </p> </div> 
                    <div class="col-md-7 col-12" v-if="this.$route.path == '/it-events'"> 
                        <select class="custom-select custom-select-sm mb-3 events" onchange="location.href=this.value">
                            <option value="/it-events" selected>IT</option>
                            <option value="/engineering-events">Инженерия</option>
                            <option value="/service-events">Сфера услуг</option>
                        </select>
                    </div>
                    <div class="col-md-7 col-12" v-else-if="this.$route.path == '/service-events'"> 
                        <select class="custom-select custom-select-sm mb-3 events" onchange="location.href=this.value">
                            <option value="/it-events">IT</option>
                            <option value="/engineering-events">Инженерия</option>
                            <option value="/service-events" selected>Сфера услуг</option>
                        </select>
                    </div>
                    <div class="col-md-7 col-12" v-else> 
                        <select class="custom-select custom-select-sm mb-3 events" onchange="location.href=this.value">
                            <option value="/it-events">IT</option>
                            <option value="/engineering-events" selected>Инженерия</option>
                            <option value="/service-events">Сфера услуг</option>
                        </select>
                    </div>
                </div>
            </div>
            <div class="card" v-for="item in data" :key="item.value">
                <div class="card-header">{{item.date}}</div>
                <div class="card-body">
                    <h5 class="card-title">{{item.name}}</h5>
                    <p class="card-text"><i class="far fa-clock"></i> {{item.time}}</p>
                    <p class="card-text"><i class="far fa-user"></i> {{item.places}}</p>
                    <p class="card-text">Тип: {{item.type}}</p>
                    <div class="row">
                        <div class="col-12 col-md-6">
                            <button class="btn btn-outline-almbb-info"  @click="add(item)">Собираюсь посетить</button>
                        </div>
                        <div class="col-12 col-md-6 gotoevents">
                            <a :href="item.link" class="btn btn-blue" @click="setScroll()">Перейти к мероприятию</a>
                        </div>
                    </div>
                </div>
            </div>




            <!-- <div class="col-1"><a class="btn btn-almbb-small btn-red-orange" style="border-radius: 50%; color: white"><i class="fas fa-plus"></i></a></div> -->
            <!-- <div class="ChosenClassList">
                <div class="row align-items-end">
                    <div class="col-12 col-lg-3" style="font-weight: bold; font-size: 1.3em; margin-top: 0.5em">Направления: </div>
                    <div class="col-9 col-lg-8" id="classList" style="text-align: left; font-size: 1.3em;">
                        <span class="badge badge-pill badge-primary" id="itbadge" style="display: none; margin-right: 0.5em">IT <a class="deleteDirection" @click="delActive('it')">&times;</a></span>
                        <span class="badge badge-pill badge-primary" id="engineeringbadge" style="display: none; margin-right: 0.5em">Инженерия <a class="deleteDirection" @click="delActive('engineering')">&times;</a></span>
                        <span class="badge badge-pill badge-primary" id="serviceEventsbadge" style="display: none;">Сфера услуг <a class="deleteDirection" @click="delActive('serviceEvents')">&times;</a></span>
                    </div>
                    <div class="col-3 col-lg-1"><a class="btn btn-almbb-small btn-red-orange" style="border-radius: 50%; color: white" @click="showChoseModal()"><i class="fas fa-plus"></i></a></div>
                </div>
                <hr>
                <div class="row">
                    <div class="col-12 col-md-10" style="padding-right: 0px">
                        <input type="text" class="form-control" name="search" style="border-radius: 50px;">
                    </div>
                    <div class="col-12 col-md-2" style="padding: 0px"><button class="btn btn-blue btn-almbb-small">Найти</button></div>
                </div>
            </div>


            <div id="my_modal" class="modal">
                <div class="modal_content">
                    <span class="close_modal_window" @click="closeChoseModal()"><i class="fas fa-times"></i></span>
                    <div class="row">
                        <div class="col-12" style="font-weight: bold; font-size: 1.5em;">Выберите направления</div>
                    </div>
                    <div class="selectGroup">
                        <div class="row item" id="it-item" @click="Chose('it')">IT</div>
                        <div class="row item" id="engineering-item" @click="Chose('engineering')">Инженерия</div>
                        <div class="row item" id="serviceEvents-item" @click="Chose('serviceEvents')">Сфера услуг</div>
                    </div>
                </div>
            </div> -->



            <!-- <div class="block" @click="setActive('it')" id="it">
                <div class="bname">IT</div>
                <div>Lorem ipsum dolor sit amet, consectetur adipiscing elit. Sed aliquet nisi id quam sollicitudin molestie. Curabitur fermentum augue nulla, in vehicula augue tincidunt vitae. Etiam mollis massa vitae velit semper, lacinia suscipit est imperdiet. Suspendisse maximus tincidunt accumsan. Morbi ac iaculis enim. Nunc convallis nec lorem nec mollis. Fusce molestie.</div>
            </div>

            <div class="block" @click="setActive('engineering')" id="engineering">
                <div class="bname">Инженерия</div>
                <div>Lorem ipsum dolor sit amet, consectetur adipiscing elit. Sed aliquet nisi id quam sollicitudin molestie. Curabitur fermentum augue nulla, in vehicula augue tincidunt vitae. Etiam mollis massa vitae velit semper, lacinia suscipit est imperdiet. Suspendisse maximus tincidunt accumsan. Morbi ac iaculis enim. Nunc convallis nec lorem nec mollis. Fusce molestie.</div>
            </div>

            <div class="block" @click="setActive('serviceEvents')" id="serviceEvents">
                <div class="bname">Сфера услуг</div>
                <div>Lorem ipsum dolor sit amet, consectetur adipiscing elit. Sed aliquet nisi id quam sollicitudin molestie. Curabitur fermentum augue nulla, in vehicula augue tincidunt vitae. Etiam mollis massa vitae velit semper, lacinia suscipit est imperdiet. Suspendisse maximus tincidunt accumsan. Morbi ac iaculis enim. Nunc convallis nec lorem nec mollis. Fusce molestie.</div>
            </div> -->

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
        }
    },
    beforeMount(){
        fetch(this.$store.state.serverIp+'/api/getAllEvents', {
            method: 'get',
        })
        .then(response => {
            console.log("res", response)
            return response.json()
        })
        .then(datan => {
            if(this.$route.path == '/it-events') this.data = datan.programming
            else if(this.$route.path == '/service-events') this.data = datan.service
            else this.data = datan.engeniring
        })
    },
    mounted(){
        setTimeout(() => {
            window.scrollTo({
                top: this.$store.getters.allEventsScroll,
                behavior: 'auto'
            });
        }, 500);
        this.$store.commit('SET_ALL_EVENTS_SCROLL', 0)
    },
    methods:{
        add(event){
            let email = this.$store.getters.email
            let SessionID = this.$store.getters.SessionID
            if(email != ''){
                delete event.places
                if(this.$route.path == '/it-events') event.mainType = 'programming'
                else if(this.$route.path == '/service-events') event.mainType = 'service'
                else event.mainType = 'engeniring'
                console.log(event)
                needle.post(this.$store.state.serverIp+'/api/checkedEventsUpdate', {email: email, events: event, sessionid: SessionID}, {"json": true}, function(err, res){
                    if (err) throw err
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
                        });
                    }
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

        setActive(elem){
            if (!document.getElementById(elem).classList.contains('active')){
                document.getElementById(elem).classList.add('active')
                document.getElementById(elem+'badge').style.display = 'inline-block'
            }
            else{
                document.getElementById(elem).classList.remove('active')
                document.getElementById(elem+'badge').style.display = 'none'

            }
        
        },
        delActive(elem){
            // document.getElementById(elem).classList.remove('active')
            document.getElementById(elem+'badge').style.display = 'none'
            document.getElementById(elem+'-item').classList.remove('selected')
        },

        Chose(elem){
            if(document.getElementById(elem+'badge').style.display == 'none'){
                document.getElementById(elem+'badge').style.display = 'inline-block'
                document.getElementById(elem+'-item').classList.add('selected')
            }
            else{
                document.getElementById(elem+'badge').style.display = 'none'
                document.getElementById(elem+'-item').classList.remove('selected')
            }
        },
        showChoseModal(){
            let modal = document.getElementById("my_modal");
            modal.style.display = "block";
        },
        closeChoseModal(){
            let modal = document.getElementById("my_modal");
            modal.style.display = "none"
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

.block{
    background-color: #ffffff;
    width: 100%;
    height: 100%;
    border-radius: 4px;
    -webkit-box-shadow: 0px 0px 5px 2px rgba(34, 60, 80, 0.2);
    -moz-box-shadow: 0px 0px 5px 2px rgba(34, 60, 80, 0.2);
    box-shadow: 0px 0px 5px 2px rgba(34, 60, 80, 0.2);
    min-height: 300px;
    padding: 15px 20px;
    margin: 1em 0em;
    cursor: pointer;
}
.block:hover{
    -webkit-box-shadow: 0px 0px 5px 4px rgba(34, 60, 80, 0.26);
    -moz-box-shadow: 0px 0px 5px 4px rgba(34, 60, 80, 0.26);
    box-shadow: 0px 0px 5px 4px rgba(34, 60, 80, 0.26);
}
.block.active{
    background: #ff8a65;
    color: rgb(255, 255, 255);
}
.bname{
    text-align: center; 
    font-weight: bold; 
    font-size: 1.3em;
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
    background: #ebecec;
}
.selectGroup .item.selected{
    background: #3F729B;
    color: white;
}
</style>