<template>
    <div class="main">
        <section class="page_404 warp">
            <div class="container">
                <div class="row">	
                <div class="col-sm-12 ">
                <div class="col-sm-12 col-sm-offset-1  text-center">
                <div class="four_zero_four_bg">
                    <h1 class="text-center ">404</h1>
                
                
                </div>
                
                <div class="contant_box_404">
                <h3 class="h2">
                Похоже, Вы заблудились
                </h3>
                
                <p>Страница, которую Вы ищете, недоступна или находится в разработке!</p>
                
                <router-link to='/' class="router-link">
                    <!-- <a type="button" class="link_404" ref="profile">Домой</a> -->
                    <a class="btn btn-rounded-almbb-success" ref="profile" style="text-decoration: none; color: white;">Домой</a>
                </router-link>
                
            </div>
                </div>
                </div>
                </div>
            </div>
        </section>
        <div class="footer"><Footer></Footer></div> 
    </div>
</template>

<script>
import Footer from './footer.vue'
export default {
    name: 'PageNotFound',
    components: { Footer },
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
    background-color: #ffffff;
    height: 100%;
    padding: 30px 0px 0px;
    min-height: 100vh;
    margin-bottom: 0px;
}



.page_404{ padding:40px 0; background:#fff; font-family: 'Arvo', serif;
}

.page_404  img{ width:100%;}

.four_zero_four_bg{
    /* background-image: url(https://cdn.dribbble.com/users/285475/screenshots/2083086/dribbble_1.gif); */
    background-image: url(./../assets/dribbble_1.gif);
    height: 400px;
    background-position: center;
 }
 
 
.four_zero_four_bg h1{
    font-size:80px;
 }
 
.four_zero_four_bg h3
{
    font-size:80px;
}			 
.link_404{			 
    color: #fff!important;
    padding: 10px 20px;
    background: #39ac31;
    margin: 20px 0;
    display: inline-block;
}
.contant_box_404{ margin-top:-50px;}
</style>