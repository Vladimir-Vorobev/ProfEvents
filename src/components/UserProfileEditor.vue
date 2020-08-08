<template>
    <div class="main">
        <div class="container warp">
            <div class="row">
                <div class="col-1">
                    <button class='btn btn-almbb-light btn-almbb-small' @click="backInProfile()" style="color: black;"><i class="fas fa-arrow-left"></i></button>
                </div>
                <div class="col-11">
                    <h3>Редактирование профиля:</h3>
                </div>
            </div>
            <form style="margin-top: 30px;">
                <div class="row">
                    <div class="col-12 col-md-6">
                        <div class="input-group mb-3">
                            <div class="input-group-prepend">
                                <span class="input-group-text" id="basic-addon1">Имя</span>
                            </div>
                            <input type="text" name="name" class="form-control name" aria-describedby="basic-addon1">
                        </div>
                    </div>
                    <div class="col-12 col-md-6">
                        <div class="input-group mb-3">
                            <div class="input-group-prepend">
                                <span class="input-group-text" id="basic-addon2">Фамилия</span>
                            </div>
                            <input type="text" name="surname" class="form-control surname" aria-describedby="basic-addon2">
                        </div>
                    </div>
                </div>
                <div class="row">
                    <div class="col-12 col-md-6">
                        <input type="date" class="form-control age" name="age" min="1900-01-01">
                    </div>
                    <div class="col-12 col-md-6">
                        <div class="input-group mb-3">
                            <div class="input-group-prepend">
                                <span class="input-group-text" id="basic-addon3">Город</span>
                            </div>
                            <input name="city" class="form-control city" placeholder="Город" aria-describedby="basic-addon3">
                        </div>
                    </div>
                </div>
                <div class="row">
                    <div class="col-12 col-md-6">
                        <div class="input-group mb-3">
                            <div class="input-group-prepend">
                                <span class="input-group-text" id="basic-addon4">@</span>
                            </div>
                            <input type="text" class="form-control" placeholder="Ник пользователя" aria-label="Короткое имя" aria-describedby="basic-addon4">
                        </div>
                    </div>
                    <div class="col-12 col-md-6">
                        <div class="input-group mb-3">
                            <div class="input-group-prepend">
                                <span class="input-group-text" id="basic-addon5">Школа</span>
                            </div>
                            <input type="text" class="form-control school" name="school" aria-describedby="basic-addon5">
                        </div>
                    </div>
                </div>
                <div class="row">
                    <div class="col-12 col-md-6">
                        <div class="input-group mb-3">
                            <div class="input-group-prepend">
                                <span class="input-group-text" id="basic-addon6">Класс</span>
                            </div>
                            <input type="text" name="class_number" class="form-control class_number" aria-describedby="basic-addon6">
                        </div>
                    </div>
                    <div class="col-12 col-md-6">
                        <div class="input-group mb-3">
                            <div class="input-group-prepend">
                                <span class="input-group-text" id="basic-addon7">Символ</span>
                            </div>
                            <input type="text" name="simvol" class="form-control simvol" aria-describedby="basic-addon7">
                        </div>
                    </div>
                </div>
                <div class="input-group row" style="margin: 0px 0px 1em">
                    <div name="avatar" class="custom-file col-12 col-lg-8">
                        <input name="img_for_avatar" type="file" accept="image/*" method="post" enctype="multipart/form-data" class="custom-file-input" id="inputGroupFile04" aria-describedby="inputGroupFileAddon04">
                        <label class="custom-file-label" for="inputGroupFile04" style="text-align: left"></label>
                    </div>
                    <div class="form-group text-center col-12 col-lg-4" style="padding: 0px 5px; border: 1px solid grey; margin: 0px" v-lazy-container="{ selector: 'img' }">
                        <img width="300" height="200" :data-src="avatar.file" alt="">
                    </div>
                </div>

                <div class="form-group row">
                    <div class="col-12"> 
                        <select name="schoolType" class="custom-select custom-select-lg mb-3 schoolType" disabled>
                            <option selected>Тип учебного заведения</option>
                            <option value="школа">Школа</option>
                            <option value="колледж">Колледж</option>
                            <option value="университет">ВУЗ</option>
                        </select>
                    </div>
                </div>
                <h4>Данные для входа:</h4>
                <div class="form-group row">
                <div class="col-12">
                    <div class="input-group mb-3">
                        <div class="input-group-prepend">
                            <span class="input-group-text" id="basic-addon7">Email</span>
                        </div>
                        <input type="text" name="email" class="form-control email" aria-describedby="basic-addon7">
                    </div>
                </div>
                </div>
                <div class="form-group row">
                <div class="col-12"> 
                    <input type="password" class="form-control pass" name="password" placeholder="Новый пароль">
                </div>
                </div>
                <div class="form-group row">
                <div class="col-12"> 
                    <input name="password2" class="form-control returnpass" placeholder="Повторите пароль">
                </div>
                </div>
                <div class="form-group"> 
                    <button class="btn btn-blue btn-lg" @click="updateUser()">Сохранить</button>
                </div>
            </form>
        </div>
        <div class="footer"><Footer></Footer></div> 
    </div>
</template>

<script>
import Footer from './footer.vue'
import Swal from 'sweetalert2'
import needle from 'needle'
import anime from 'animejs'
export default {
    name: 'UserProfileEditor',
    components: { Footer },
    data(){
      return{
        email: this.$store.getters.email,
        SessionID: this.$store.getters.SessionID,
        userId: 0,
        avatar: '',
      }
    },
    beforeMount(){
        if(this.email == '') window.location.pathname = "/login"
        fetch(this.$store.state.serverIp+'/api/getAvatar', {
            method: 'POST',
            headers: {email: this.email, sessionid: this.SessionID},
        })
        .then(response => {
            console.log("res", response)
            return response.json()
        })
        .then(data => {
            console.log(data)
            if(data == '310'){
                document.cookie = "email=" + ";expires=Thu, 01 Jan 1970 00:00:01 GMT"
                document.cookie = "SessionID=" + ";expires=Thu, 01 Jan 1970 00:00:01 GMT"
                window.location.reload()
            }
            else{
                this.avatar = data.data
                console.log(data)
            }
        })
        fetch(this.$store.state.serverIp+'/api/getInformation', {
            method: 'POST',
            headers: {email: this.email, sessionid: this.SessionID},
        })
        .then(response => {
            console.log("res", response)
            return response.json()
        })
        .then(data => {
            if(data == '310'){
                document.cookie = "email=" + ";expires=Thu, 01 Jan 1970 00:00:01 GMT"
                document.cookie = "SessionID=" + ";expires=Thu, 01 Jan 1970 00:00:01 GMT"
                window.location.reload()
            }
            if(data.city != undefined){
                document.querySelector(".city").value = data.city;
            }
            if(data.school != undefined){
                document.querySelector(".school").value = data.school;
            }
            if(data.schoolType != undefined){
                document.querySelector(".schoolType").value = data.schoolType;
            }
            // if(data.role != 'user' && data.role != 'student') {
            //   this.role = true
            // }
            if(data.class_number != undefined){
                document.querySelector(".class_number").value = data.class_number;
            }
            if(data.simvol != undefined){
                document.querySelector(".simvol").value = data.simvol;
            }
            // if(data.statNumber != undefined){
            //     var span = document.querySelector(".statNumber");
            //     if ('textContent' in span) {
            //     span.textContent = data.statNumber;
            //     } else {
            //     span.innerText = data.statNumber;
            //     }
            // }
            this.userId = data._id
            document.querySelector(".name").value = data.name;
            document.querySelector(".surname").value = data.surname;
            document.querySelector(".email").value = data.email;
            document.querySelector(".age").value = data.age;
        })
        .catch(err => {
            console.log(err)
        })
        },
        mounted(){
            let rec = false
            console.log(this.$store.state.socketIp)
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
                this.photo.push({contentType: data.photo.contentType, data: data.photo.data})
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
                            timer: 2000
                        })
                        setTimeout(() => {
                            window.location.reload()
                        }, 2000);
                    }, 2000);
                }
            })
        },
    methods: {
        updateUser(){
            event.preventDefault()
            let re = /^([A-Za-z0-9_\-.])+@([A-Za-z0-9_\-.])+\.([A-Za-z]{2,4})$/
            let form = document.forms[0]
            let name = form.elements.name.value
            let surname = form.elements.surname.value
            let email = form.elements.email.value.replace(/\s/g, '')
            let age = form.elements.age.value
            let password = form.elements.password.value
            let password2 = form.elements.password2.value
            let city = form.elements.city.value
            let school = form.elements.school.value
            let schoolType = form.elements.schoolType.value
            let class_number = form.elements.class_number.value
            let simvol = form.elements.simvol.value
            let filedata = document.querySelector(".custom-file-input");
            let file = filedata.files[0]
            console.log(file)
            if(password != password2){
            //alert("Пароли не совпадают")
            this.$swal({
                icon: 'error',
                text: 'Пароли не совпадают'
            });
            }
            else if(re.test(email) == false && email.trim() != ''){
            //alert("Введен некорректный email")
            this.$swal({
                icon: 'error',
                text: 'Введен некорректный email'
            })
            }
            else if(password.length < 5 && password.trim() != ''){
            //alert("Пароль слишком короткий")
            this.$swal({
                icon: 'error',
                text: 'Пароль слишком короткий'
            })
            }
            else if(password.length > 15){
            //alert("Пароль слишком длинный")
            this.$swal({
                icon: 'error',
                text: 'Пароль слишком длинный'
            })
            }
            else{
                let crypto = require('crypto')
                let dataq = {}
                if(name.trim() != '') dataq.name = name
                if(surname.trim() != '') dataq.surname = surname
                if(email.trim() != '') dataq.email = email
                if(age.trim() != '') dataq.age = age
                if(password.trim() != '') dataq.password = crypto.createHash('md5').update(password).digest("hex")
                if(city.trim() != '') dataq.city = city
                if(school.trim() != '') dataq.school = school
                if(schoolType.trim() != '' && schoolType != "Тип учебного заведения") dataq.schoolType = schoolType
                if(class_number.trim() != '') dataq.class_number = class_number
                if(simvol.trim() != '') dataq.simvol = simvol
                if(file != undefined){
                    let data = []
                    let len = 1
                    let url = this.$store.state.serverIp
                    if(len == 0){
                        Swal.fire({
                            icon: 'error',
                            title: 'Ошибка',
                            text: 'Файл не выбран',
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
                    let reader = new FileReader();
                    let file = document.querySelector('.custom-file-input').files[0]
                    reader.readAsDataURL(file);
                    let email = this.email
                    let SessionID = this.SessionID
                    reader.onload = function () {
                        data.push({file: reader.result, type: file.type})
                        needle.post(url + '/api/uploadAvatar', {images: data, email: email, sessionid: SessionID}, {"json": true}, function(err, res){
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
                        reader.onerror = function (error) {
                            Swal.fire({
                                icon: 'error',
                                title: 'Ошибка',
                                text: 'Непредвиденная ошибка, повторите попытку',
                                timer: 2000
                            })
                            console.log('Error: ', error);
                        };
                    }
                }
                needle.post(this.$store.state.serverIp+'/api/updateInformation', {email: this.email, sessionid: this.SessionID, update: dataq}, {"json": true}, function(err){
                    if (err) console.log(err)
                    //window.location.reload()
                })
            }
        },
        backInProfile(){
            //надо сделать переход назад к профилю пользователя, пока тут alert
            // Vue.swal({
            //     icon: 'error',
            //     text: 'Функция временно не доступна!',
            //     showConfirmButton: false,
            //     timer: 1500,
            //     timerProgressBar: true,
            // });
            let userId = this.userId
            this.$router.push({ path: `/user-profile/${userId}` })
        },
    }
}
</script>

<style scoped>
.warp{
    flex: 1 0 auto;
    padding-top: 150px !important;
    background-color: #fff;
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
    content: "Выбрать фото для аватара";
}
</style>