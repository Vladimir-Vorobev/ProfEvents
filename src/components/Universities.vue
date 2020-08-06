<template>
    <div class="main">
        <div class="container warp">
            <div class="card" v-for="item in data" :key="item.value">
                <div class="card-body">
                    <h5 class="card-title">{{item.name}}</h5>
                    <p class="card-text">Минимальный проходной балл на бюджетную основу: {{item.min}}</p>
                    <p class="card-text">Минимальный проходной балл на платную основу: {{item.max}}</p>
                    <p class="card-text">Минимальная цена обучения. Тыс/год: {{item.price}}</p>
                </div>
            </div>
        </div>
        <div class="footer"><Footer></Footer></div>
    </div>
</template>

<script>
import Footer from './footer.vue'
export default {
    name: 'Universities',
    components: { Footer },
    data(){
        return{
            data: []
        }
    },
    beforeMount(){
        let email = this.$store.getters.email
        let SessionID = this.$store.getters.SessionID 
        needle('post',this.$store.state.serverIp+'/api/getAllEvents', {email: email, sessionid: SessionID}, {"json": true})
        .then(res => {
            this.data = res.body['points']
        })
        .catch(function(err) {
            console.log(err)
        })
    },
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
.card{ 
    margin-top: 10px !important;
}
.card-body{
    text-align: left !important;
}
.card-body h5{
    font-weight: bold;
}
</style>