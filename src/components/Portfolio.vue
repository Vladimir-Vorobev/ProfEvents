<template>
  <div class="main">
    <div class="container warp">
        <div class="card">
            <div class="card-body">
                <h5 class="card-title">Добавить в портфолио:</h5>
                <div class="input-group">
                    <div class="custom-file">
                        <input type="file" class="custom-file-input" id="inputGroupFile04" aria-describedby="inputGroupFileAddon04" multiple>
                        <label class="custom-file-label" for="inputGroupFile04" style="text-align: left"></label>
                    </div>
                    <div class="input-group-append">
                        <button class="btn btn-outline-success" type="button" id="inputGroupFileAddon04" @click="send()">Добавить</button>
                    </div>
                </div>
            </div>
        </div>
        <div class="img justify-content-center">
        {{photos}}
            <div v-if="photos.length == 0">
                {{photos.length}}
                <div class="justify-content-center h2">Загрузка портфолио</div>
                <div style="text-align: center;"><i class='fa fa-spinner fa-pulse fa-3x'></i></div>
            </div>
            <div v-for="item in photos" :key="item">
                <div class="form-group text-center my-sm-2 block" :id="item.id">
                    <a class="delItem" @click="delete_portfolio(item.id)"><i class="fas fa-times"></i></a>
                    <img :src="item.file" alt="" >
                </div>
            </div>
        </div>
    </div>
    <div class="footer"><Footer></Footer></div> 
  </div>
</template>

<script>
// import needle from "needle"
import Footer from './footer.vue'
import Swal from 'sweetalert2'
import needle from 'needle'
import anime from 'animejs'
export default {
    name: 'Portfolio',
    components: { Footer },
    data(){
        return{
            photos: [],
            email: this.$store.state.email,
            SessionID: this.$store.state.SessionID,
            num: 0,
        }
    },
    beforeMount(){
        fetch(this.$store.state.serverIp+'/api/getPortfolio', {
            method: 'POST',
            headers: {email: this.email, sessionid: this.SessionID},
        })
        .then(response => {
            // console.log("res", response)
            this.photo = []
            return response.json()
        })
    },
    mounted(){
        let rec = false
        let socket = require('socket.io-client')(this.$store.state.socketIp)
        socket.on('connect', () => {
            socket.emit('new_user', this.email)
            if(rec){
                socket.emit('recon', this.email)
            }
        })
        socket.on('disconnect', () => {
            rec = true
        })
        let numOfUploadedFiles = 0
        socket.on('send_image', (res) => {
            console.log(res)
            this.photos.splice(this.photos.length, 1, res)
            console.log(this.photos)
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
        send(){
            event.preventDefault()
            let data = []
            let len = document.querySelector('.custom-file-input').files.length
            let url = this.$store.state.serverIp
            if(len == 0){
                Swal.fire({
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
            Swal.fire({
                icon: 'info',
                title: 'Ваши файлы загружаются',
                html: '<div class="justify-content-center h2 persents" value="0">0%</div>' +
                '<progress class="progress_swal" value="0" max="100"></progress>',
            })
            for(let i = 0; i < len; i++){
                let reader = new FileReader();
                let file = document.querySelector('.custom-file-input').files[i]
                reader.readAsDataURL(file);
                let email = this.email
                let SessionID = this.SessionID
                reader.onload = function () {
                    data.push({file: reader.result, type: file.type, id: new Date().toISOString()})
                    if(i == len - 1){
                        // console.log(data)
                        needle.post(url + '/api/uploadPortfolio', {images: data, email: email, sessionid: SessionID, type: 'update'}, {"json": true}, function(err, res){
                            if(err){
                                Swal.fire({
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
                    Swal.fire({
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
        delete_portfolio(id){
            needle.post(this.$store.state.serverIp + '/api/uploadPortfolio', {id: id, email: this.email, sessionid: this.SessionID, type: 'delete'}, {"json": true}, function(err, res){
                if(err) throw err
                else if(res.body == "OK") document.getElementById(id).style.display = 'none'
            })
        },
    }
}


</script>

<style scoped>
.warp{
    flex: 1 0 auto;
    padding-top: 90px !important;
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

.custom-file-input:lang(ru)~.custom-file-label::after {
    content: "Выбрать";
}

.card{ 
    margin: 20px 0px !important; 
}
.card-body{ 
    text-align: left !important;
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
.block img{
    max-width: 100%;
}
.delItem{
    float: right;
    font-size: 1.5em;
    margin-top: -15px;
}
.delItem:hover{
    color: #ff4444;
}
</style>