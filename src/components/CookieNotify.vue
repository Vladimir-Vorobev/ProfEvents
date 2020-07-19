<template>
    <div id="cookie_notification">
        <p>Для улучшения работы сайта и его взаимодействия с пользователями мы используем файлы cookie. Продолжая работу с
            сайтом, Вы разрешаете использование cookie-файлов. Вы всегда можете отключить файлы cookie в настройках Вашего
            браузера.</p>
        <button class="btn btn-rounded-outline-red-orange cookie_accept" @click="acceptCookie()">Принять</button>
    </div>
</template>

<script>
export default {
  name: 'CookieNotify',
  mounted(){
    let cookieDate = localStorage.getItem('cookieDate');
    let cookieNotification = document.getElementById('cookie_notification');
    // Если записи про кукисы нет или она просрочена на 1 год, то показываем информацию про кукисы
    if( !cookieDate || (+cookieDate + 31536000000) < Date.now() ){
        cookieNotification.classList.add('show');
    }
  },
  methods: {
        acceptCookie(){
            let cookieNotification = document.getElementById('cookie_notification');
            localStorage.setItem( 'cookieDate', Date.now() );
            cookieNotification.classList.remove('show');
        },
  },
}
</script>

<style>

</style>