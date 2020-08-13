<template>
    <div class="main" id="main">
        <div class="container warp">
            <div v-if="data.length == 0"><h3>Возможно, Вы еще не добавили ни одного мероприятия, посмотрите страницу всех мероприятий</h3></div>
            <div class="card" v-for="(item, index) in data" :key="item.value">
                <div class="card-header">{{item.data.date}}</div>
                <div class="card-body">
                    <div class="row" v-if="item.status != 'checked'">
                        <h5 class="card-title col-11">{{item.data.name}}</h5>
                        <h5><button class="btn btn-almbb-danger btn-almbb-small" @click="deleteEvent(item.data, index)"> <i class="fas fa-trash-alt"></i> </button></h5>
                    </div>
                    <div class="row" v-if="item.status == 'checked'">
                        <h5 class="card-title col-12">{{item.data.name}}</h5>
                    </div>
                    <p class="card-text"><i class="far fa-clock"></i> {{item.data.time}}</p>
                    <p class="card-text">Тип: {{item.data.type}}</p>
                    <p v-if="item.status == 'checked'" class="card-text" style="color: green;">Участие подтверждено</p>
                    <p v-if="item.status == 'on_moderate'" class="card-text" style="color: #0099CC;">Участие проверяется модератором</p>
                    <p v-if="item.status == 'not_checked'" class="card-text" style="color: red;">Участие не подтверждено</p>
                    <div class="row" v-if="item.status != 'checked'">
                        <div class="col-12 col-md-6">
                            <a :href="item.data.link" class="btn btn-blue">Перейти к мероприятию</a>
                        </div>
                        <div class="col-12 col-md-6">
                            <button class="btn btn-blue" @click="moderate(item.data)">Отправить на модерацию</button>
                        </div>
                    </div>
                </div>
            </div>
        </div>
        <div class="footer"><Footer></Footer></div> 
    </div>
</template>

<script>
import Swal from 'sweetalert2'
import needle from 'needle'
import anime from 'animejs'
import Vue from 'vue';
import Footer from './footer.vue'
export default {
    name: 'YourEvents',
    components: { Footer },
    data(){
        return{
            data: [],
            email: this.$store.getters.email,
            SessionID: this.$store.getters.SessionID,
            files: [],
        }
    },
     beforeMount(){
        fetch(this.$store.state.serverIp+'/api/getCheckedEvents', {
            method: 'get',
            headers: {studemail: this.email, sessionid: this.SessionID},
		})
        .then(response => {
            console.log("res", response)
            return response.json()
        })
        .then(data => {
            if(data == '310'){
                document.cookie = "email=" + ";expires=Thu, 01 Jan 1970 00:00:01 GMT"
                document.cookie = "SessionID=" + ";expires=Thu, 01 Jan 1970 00:00:01 GMT"
                window.location.href = '/login'
            }
            console.log(data)
            this.data = data.checkedEvents
        })
        .catch(err => {
            console.log(err)
        })
     },
     mounted(){
        let rec = false
        let socket = require('socket.io-client')(this.$store.state.socketIp)
        socket.on('connect', () => {
            if(rec){
                socket.emit('recon', this.email)
            }
        })
        socket.on('disconnect', () => {
            rec = true
        })
        socket.emit('new_user', this.email)
        let numOfUploadedFiles = 0
        socket.on('send_image', (data) => {
            console.log(data)
            this.photo.push({contentType: data.data.contentType, data: data.data.file})
        })
        socket.on('add_system_image', () => {
            numOfUploadedFiles += 1
            let pers = document.querySelector('.persents')
            let num = numOfUploadedFiles / this.num * 100
            anime({
                targets: 'progress',
                value: num,
                easing: 'linear'
            });
            anime({
                targets: pers,
                innerHTML: num + '%',
                easing: 'linear',
                round: 1,
            });
            pers.value = num
            if(numOfUploadedFiles == this.num){
                setTimeout(() => {
                    Swal.fire({
                        icon: 'success',
                        title: 'Изображения успешно сохранены',
                        showConfirmButton: false,
                        timer: 2000
                    })
                    setTimeout(() => {
                        window.location.reload()
                    }, 2000);
                }, 2000);
            }
        })
    },
     methods:{
        deleteEvent(events, eventPlace){
            this.$swal({
                icon: 'warning',
                title: 'Вы уверены что хотите удалить?',
                showCancelButton: true,
                confirmButtonColor: '#3085d6',
                cancelButtonColor: '#d33',
                confirmButtonText: 'Да',
                cancelButtonText: 'Отмена'
            }).then((result) => {
                if (result.value) {
                    event.preventDefault()
                    needle('post',this.$store.state.serverIp+'/api/deleteEvent', {email: this.email, event: events, sessionid: this.SessionID}, {"json": true})
                    .then(res => {
                        if(res.body == '310'){
                            document.cookie = "email=" + ";expires=Thu, 01 Jan 1970 00:00:01 GMT"
                            document.cookie = "SessionID=" + ";expires=Thu, 01 Jan 1970 00:00:01 GMT"
                            window.location.href = '/login'
                        }
                        Vue.swal({
                            icon: 'success',
                            text: 'Мероприятие успешно удаленно',
                            showConfirmButton: false,
                            timer: 1500,
                            timerProgressBar: true,
                        }).then(() => {
                            // window.location.reload()
                            this.$delete(this.data, eventPlace)
                        });
                    })
                    .catch(function(err) {
                        console.log(err)
                    })

                }
            });
        },
        moderate(events){
            this.$swal({
                title: 'Выберите изображения, подтверждающие Ваше участие в мероприятии',
                html: '<div class="custom-file">' +
                        '<input type="file" class="custom-file-input" id="inputGroupFile04" aria-describedby="inputGroupFileAddon04" accept="image/*" multiple>' +
                        '<label class="custom-file-label" for="inputGroupFile04" style="text-align: left"></label>' +
                    '</div>',
                showCancelButton: true,
                cancelButtonText: 'Отмена',
                confirmButtonText: 'Отправить',
            }).then((result) => {
                if (result.value) {
                    this.files = document.querySelector('.custom-file-input').files
                    this.send_on_moderation(events)
                }
            })
        },
        send_on_moderation(events){
            event.preventDefault()
            let data = []
            let len = this.files.length
            let url = this.$store.state.serverIp
            if(len == 0){
                this.$swal({
                    icon: 'error',
                    title: 'Ошибка',
                    text: 'Файл не выбран',
                    showConfirmButton: false,
                    timer: 2000
                })
                setTimeout(() => {
                    return
                }, 2000);
            }
            this.num = len
            this.$swal({
                icon: 'info',
                title: 'Ваши файлы загружаются',
                html: '<div class="justify-content-center h2 persents" value="0">0%</div>' +
                '<progress class="progress_swal" value="0" max="100"></progress>',
            })
            for(let i = 0; i < len; i++){
                let reader = new FileReader();
                let file = this.files[i]
                reader.readAsDataURL(file);
                let email = this.email
                let SessionID = this.SessionID
                reader.onload = function () {
                    data.push({file: reader.result, type: file.type})
                    if(i == len - 1){
                        needle.post(url + '/api/moderateEvent', {type: 'student', images: data, email: email, sessionId: SessionID, data: events}, {"json": true}, function(err, res){
                            if(err){
                                this.$swal({
                                    icon: 'error',
                                    title: 'Ошибка',
                                    text: 'Непредвиденная ошибка, повторите попытку',
                                    timer: 2000
                                })
                                console.log(err)
                            }
                            else if(res.body == '310'){
                                document.cookie = "email=" + ";expires=Thu, 01 Jan 1970 00:00:01 GMT"
                                document.cookie = "SessionID=" + ";expires=Thu, 01 Jan 1970 00:00:01 GMT"
                                document.location.href = '/login'
                            }
                        })
                    }
                }
                reader.onerror = function (error) {
                    this.$swal({
                        icon: 'error',
                        title: 'Ошибка',
                        text: 'Непредвиденная ошибка, повторите попытку',
                        showConfirmButton: false,
                        timer: 2000
                    })
                    console.log('Error: ', error);
                };
            }
        },
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
.card-header{
    font-weight: bold;
}
.card-body h5{ 
    font-weight: bold;
}
.card-title{
    text-align: center;
}
</style>