<template>
  <div class="main">
    <div class="container warp">
        <div class="card">
            <div class="card-body">
                <div class="input-group">
                    <div class="custom-file">
                        <input type="file" class="custom-file-input" id="inputGroupFile04" aria-describedby="inputGroupFileAddon04">
                        <label class="custom-file-label" for="inputGroupFile04" style="text-align: left">Выбрать файл</label>
                    </div>
                    <div class="input-group-append">
                        <button class="btn btn-outline-success" type="button" id="inputGroupFileAddon04">Добавить</button>
                    </div>
                </div>
            </div>
        </div>
        <div>
            <div class="card" >
                <img src="./../assets/for_main.jpg" class="card-img-top">
                <div class="card-body">
                    <p class="card-text">Some quick example text to build on the card title and make up the bulk of the card's content.</p>
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
export default {
    name: 'Portfolio',
    components: { Footer },
    
}


</script>

<style scoped>
.warp{
    flex: 1 0 auto;
    padding-top: 110px !important;
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
.card img{
    display: block;
    max-width: 100%;
    max-height: 100%;
    min-width: 310px;
    min-height: 310px;
}
</style>