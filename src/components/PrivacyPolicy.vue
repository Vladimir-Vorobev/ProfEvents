<template>
    <div class="main">
        <div class="container warp">
            <h2>Политика конфиденциальности</h2>
            <div class="block">
                <ol class="list">
                    <li>Test
                        <ol class="list">
                            <li>Test</li>
                            <li>Test</li>
                        </ol>
                    </li>
                    <li>Test</li>
                </ol>
            </div>
        </div>
        <div class="footer"><Footer></Footer></div> 
    </div>
</template>

<script>
import Footer from './footer.vue'
export default {
    name: 'PrivacyPolicy',
    components: { Footer },
}
</script>

<style scoped>
.warp{
    flex: 1 0 auto;
    padding-top: 110px !important;
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
    padding: 0px 0px;
    min-height: 100vh;
    margin-bottom: 0px;
}
ol {
    list-style: none; 
    counter-reset: li; 
}
li:before {
    counter-increment: li; 
    content: counters(li,".") ". "; 
}
.list{ 
    text-align: left; 
    margin: 0px 2em 
}
.block{
    font-size: 1.2em;
}
</style>