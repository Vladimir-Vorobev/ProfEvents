<template>
        <nav class="navbar navbar-expand-lg navbar-light  fixed-top navStyle">
            <router-link to="/" class="router-link">
                <a class="navbar-brand" ref="home" style="color: #f23333">Profevents</a>
            </router-link>
            <button @click="animate_toggler()" class="navbar-toggler" type="button" data-toggle="collapse" data-target="#navbarSupportedContent" aria-controls="navbarSupportedContent" aria-expanded="false" aria-label="Toggle navigation">
                <div class="anim-bar-dark anim-2">
                    <span class="icon-bar"></span>
                    <span class="icon-bar"></span>
                    <span class="icon-bar"></span>
                </div>
            </button>
            <div class="collapse navbar-collapse" id="navbarSupportedContent" @click="animate_navbar()">
                <ul class="navbar-nav mr-auto">
                    <li class="nav-item">
                        <router-link to="/events" class="router-link">
                            <a class="nav-link" ref="allEvents" style="color: #000 !important">Все мероприятия</a>
                        </router-link>
                    </li>
                    <li class="nav-item">
                        <router-link to="/career-guidance" class="router-link">
                            <a class="nav-link" ref="CareerGuidance" style="color: #000 !important">Тест на профориентацию</a>
                        </router-link>
                    </li>
                    <li class="nav-item dropdown">
                        <a class="nav-link dropdown-toggle useful"  href="#" id="navbarDropdown" role="button" data-toggle="dropdown" aria-haspopup="true" aria-expanded="false" style="color: #000 !important">
                            Полезное
                        </a>
                        <div class="dropdown-menu" aria-labelledby="navbarDropdown" >
                            <router-link to="/literature" class="dropdown-item">
                                <a ref="Literature">Литература</a>
                            </router-link>
                            <router-link to="/open-days" class="dropdown-item">
                                <a ref="OpenDays">Дни открытых дверей</a>
                            </router-link>
                            <router-link to="/universities" class="dropdown-item">
                                <a ref="Universities">Баллы в ВУЗы</a>
                            </router-link>
                            <router-link to="/partners" class="dropdown-item">
                                <a ref="Partners">Партнёры</a>
                            </router-link>
                            <router-link to="/404" class="dropdown-item">
                                <a ref="PageNotFound">Трансляция мероприятий</a>
                            </router-link>
                        </div>
                    </li>
                </ul>
                <router-link to='/login' class="router-link login" style="display:block">
                    <a type="button" class="btn btn-rounded-blue btn-lg" ref="login">{{loginText}}</a>
                </router-link>
                <div class="dropdown person_menu" style="display:none">
                    <button class="btn btn-rounded-blue dropdown-toggle user" type="button" id="dropdownMenuButton" data-toggle="dropdown" aria-haspopup="true" aria-expanded="false">
                        {{loginText}}
                    </button>
                    <div class="dropdown-menu dropdown-menu-right" aria-labelledby="dropdownMenuButton">
                        <a @click="person_profile()" class="dropdown-item router-link" style="color: #16181b !important; cursor: pointer;">Моя страница</a>
                        <router-link to="/your-events" class="router-link">
                            <a class="dropdown-item" ref="yourEvents" style="color: #16181b !important">Мои мероприятия</a>
                        </router-link>
                        <div v-if="role == 'user' || role == 'student'">
                            <router-link to="/portfolio" class="router-link">
                                <a class="dropdown-item" ref="Portfolio" style="color: #16181b !important">Портфолио</a>
                            </router-link>
                        </div>
                        <div v-if="role != 'user' && role != 'student'">
                            <router-link to="/admin" class="router-link">
                                <a class="dropdown-item" ref="Admin" style="color: #16181b !important">Админ панель</a>
                            </router-link>
                        </div>
                        <div class="dropdown-divider"></div>
                        <button @click="exit()" class="dropdown-item exit personMenuDrop" href="#">Выйти</button>
                    </div>
                </div>
            </div>
        </nav>
</template>


<script>
// import $ from 'jquery'
export default {
    name: 'Header',
    data(){
        return {
            loginText: 'Войти',
            userId: 0,
            role: '',
        }
    },
    beforeMount(){
      let email = this.$store.getters.email
      let SessionID = this.$store.getters.SessionID
      if(email != ''){
        fetch(this.$store.state.serverIp+'/api/getInformation', {
          method: 'POST',
          headers: {email: email, sessionid: SessionID},
        })
        .then(response => {
            console.log("res", response)
            return response.json()
        })
        .then(data => {
            if(data == '310'){
              console.log('header ' + email + ' ' + SessionID)
              alert('header ' + email + ' ' + SessionID)
              document.cookie = "email=" + ";expires=Thu, 01 Jan 1970 00:00:01 GMT"
              document.cookie = "SessionID=" + ";expires=Thu, 01 Jan 1970 00:00:01 GMT"
              window.location.reload()
            }
            this.loginText = data.name + ' ' + data.surname
            this.userId = data.userId
            this.role = data.role
            document.querySelector('.login').style.display = 'none'
            document.querySelector('.person_menu').style.display = 'block'
        })
        .catch(err => {
            console.log(err)
        })
      }
    },

    methods: {
      exit(){
        document.cookie = "email=" + ";expires=Thu, 01 Jan 1970 00:00:01 GMT"
        document.cookie = "SessionID=" + ";expires=Thu, 01 Jan 1970 00:00:01 GMT"
        // this.$router.push({ path: `/login` })
        window.location.reload()
        document.location.href = "/login"
      },
      person_profile(){
        let userId = this.userId
        // this.$router.replace({ path: `/user-profile/${userId}` }) //хз как без перезагрузки поправить
        document.location.href = "/user-profile/" + userId
      },
      animate_navbar(){
          if(event.target.classList.contains('useful') == false && event.target.classList.contains('user') == false){
                document.querySelector('.navbar-collapse').classList.remove('show')
                document.querySelector('.navbar-toggler').classList.remove('active')
          }
      },
      animate_toggler(){
          document.querySelector('.navbar-toggler').classList.toggle('active')
      },
    //   show_useful(){
    //       document.querySelector('.useful').classList.toggle('show')
    //       document.querySelector('.dropdown-menu').classList.toggle('show')
    //   }
    }

}
</script>

<style scoped>
.router-link {
  text-decoration: none; /* отменяем подчеркивание ссылки */
  color: #fff;
}
.exit{
  color: red;
}

.dropdown-item:active {
  background: #f1f2f3 !important;
  outline: 0px;
    -ms-user-select: none;
    -moz-user-select: none;
    -khtml-user-select: none;
    -webkit-user-select: none;
}
.navStyle{
    background-color: rgba(255,255,255,1);
    box-shadow: 0px 1px 3px rgba(0,0,0,0.10);
}
</style>