<template>
    <div class="main">
        <!-- <div class="container warp">
            <div class="name">ProfEvents</div>
            <div class="description">
                <div>Поисковый сервис профориентационных мероприятий</div> <br>
                <div>Здесь вы сможете найти самые интересные и бесплатные профориентационные мероприятия.</div>
            </div>
            <div class="row for_who" >
                <div class="col-12 col-lg-4">
                    <h4 style="border-bottom: 1px solid black">Для обучающихся</h4> 
                    <ul class="list">
                        <li>Профориентационный тест, помогающий выбрать нужные мероприятия</li>
                        <li>Поиск профориентационных мероприятий по критериям</li>
                        <li>Формирование портфолио</li>
                    </ul>
                </div>
                <div class="col-12 col-lg-4">
                    <h4 style="border-bottom: 1px solid black">Для учителей</h4> 
                    <ul class="list">
                        <li>Подбор актуальных мероприятий</li>
                        <li>Учет посещения мероприятий по классам</li>
                        <li>Учет посещения ДО и Внеурочной деятельности* <small>(дополнительная функция)</small></li>
                    </ul>
                </div>
                <div class="col-12 col-lg-4">
                    <h4 style="border-bottom: 1px solid black">Для администрации</h4> 
                    <ul class="list">
                        <li>Статистика посещений профориентационных мероприятий школьниками</li>
                        <li>Доступ к полной статистике по школе посещения ДО и внеурочной деятельности* <small>(дополнительная функция)</small></li>
                    </ul>
                </div>
            </div> <hr>
            <div>
                <p>Возможность приглашать пользователей на профориентационные мероприятия</p> 
                <p>Делиться фотографиями с мероприятий</p>
                <p>Регистрация на мероприятия</p>
            </div>
        </div> -->
        <div class="warp">
            <div class="block1">
                <div class="row" style="max-width: 100%;">
                    <div class="col-12 col-lg-5" style="vertical-align: center; text-align: left; padding-left: 3em;">
                        <div style="font-weight: bolder; font-size: 2.3em; color: black; margin-top: 1.5em">Поисковый сервис профориентационных мероприятий</div>
                        <div style="font-size: 1em; color: black; margin-top: 1em">Здесь вы сможете найти самые интересные и бесплатные профориентационные мероприятия</div>
                    </div>
                    <div class="col-12 col-lg-7" style="padding-right: 0em;">
                        <img style="margin-right: -15px;" class="ml-auto" src="mainPic.png">
                    </div>
                </div>
            </div>
            <div class="block2">
                <div class="row br2" style="max-width: 100%;">
                    <div class="col-12 col-lg-4">
                        <div class="check-circle"><i class="far fa-check-circle"></i></div>
                        <p class="circle_des">Возможность приглашать пользователей</p>
                    </div>
                    <div class="col-12 col-lg-4">
                        <div class="check-circle"><i class="far fa-check-circle"></i></div>
                        <p class="circle_des">Делиться фотографиями с мероприятий</p>
                    </div>
                    <div class="col-12 col-lg-4">
                        <div class="check-circle"><i class="far fa-check-circle"></i></div>
                        <p class="circle_des">Регистрация на мероприятия</p>
                    </div>
                </div>
            </div>
            <div class="block3">
                <div class="row br3" style="max-width: 100%;">
                    <div class="col-12 col-lg-4">
                        <div><img src="for_user.png"></div>
                        <div class="for_block">Для обучающихся</div>
                        <ul class="for_ul">
                            <li><span>Профориентационный тест, помогающий выбрать нужные мероприятия</span></li>
                            <li><span>Поиск профориентационных мероприятий по критериям</span></li>
                            <li><span>Формирование портфолио</span></li>
                        </ul>
                    </div>
                    <div class="col-12 col-lg-4">
                        <div><img src="for_teachers.png"></div>
                        <div class="for_block">Для учителей</div>
                        <ul class="for_ul">
                            <li><span>Подбор актуальных мероприятий</span></li>
                            <li><span>Учет посещения мероприятий по классам</span></li>
                            <li><span>Учет посещения ДО и Внеурочной деятельности* (дополнительная функция)</span></li>
                        </ul>
                    </div>
                    <div class="col-12 col-lg-4">
                        <div><img src="for_administrations.png"></div>
                        <div class="for_block">Для администрации</div>
                        <ul class="for_ul">
                            <li><span>Статистика посещений профориентационных</span></li>
                            <li><span>мероприятий школьниками</span></li>
                            <li><span>Доступ к полной статистике по школе посещения ДО и внеурочной деятельности* (дополнительная функция)</span></li>
                        </ul>
                    </div>
                </div>
            </div>
        </div>
        <div class="footer"><Footer></Footer></div> 
    </div>
</template>

<script>
import Footer from './footer.vue'
export default {
    name: 'MainPage',
    components: { Footer },
}
</script>

<style scoped>
.warp{
    flex: 1 0 auto;
    padding-top: 110px !important;
    background-color: #fff;
    padding: 0px;
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
    padding: 0px 0px;
    min-height: 100vh;
    margin-bottom: 0px;
}
.name{
    font-size: 2em;
    font-weight: bold !important;
    margin-bottom: 1em;
}
.description{
    font-size:1.25em;  
}
.for_who{
    margin-top: 0.7em;
}
.list li{
    margin-bottom: 0.3em;
}
.photo{
  width: 100%;
  height: 100%;
}
.photo img{
  object-fit: cover;
  width: 100%;
  height: 100%;
}

.block1{
    font-family: 'PT Mono', monospace;
    min-height: 500px;
    padding: 0px;
}
.block1 img{
    display: block;
    max-width: 100%;
}
.block2{
    font-family: 'PT Mono', monospace;
    min-height: 535px;
    background-color: #efefef;
}
.block3{
    font-family: 'PT Mono', monospace;
    min-height: 535px;
}
.check-circle{
    font-size: 8em;
    color: rgb(32, 32, 32);
}
.br2{
    padding: 7%;
}
.br3{
    padding: 7%;
}
.circle_des{
    font-size: bold;
}
.for_block{
    margin-top: 0.5em;
    padding: 12px;
    border-radius: 50px;
    background-color: #c60000;
    color: white;
    font-size: 1.2em;
    font-weight: bold;
}
.for_ul{
    margin-top: 0.4em;
    font-size: 0.7em;
}
.for_ul li{
    color: #c60000;
    font-size: 1.5em;
}
.for_ul li span{
    color: black;
    font-size: 0.8em;
}
</style>