<template>
  <div class="main">
    <div class="container warp">

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
    padding-top: 210px !important;
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


</style>