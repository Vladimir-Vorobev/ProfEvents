<template>
<div>
    <footer class="mt-auto py-3">
        <!-- <div class="container">
            <span class="text-muted">Разработано DARK_LED</span>
        </div> -->
        <div class="row" style="margin: 0px">
            <div class="col-4" style="text-align: left !important;">
                <a href="#" class="links">Разработано DARK_LED</a>
            </div>
            <div class="col-3" style="text-align: center !important;">
                <span class="text-muted">&#169; Profevents, 2020</span>
            </div>
            <div class="col-5" style="text-align: right !important;">
                <a href="#" class="links">Нашли ошибку? </a>
                <a href="#" class="links">Политика конфиденциальности</a>
            </div>
        </div>
    </footer>
</div>

</template>


<script>

export default {
    name: 'Footer',
}
</script>

<style scoped>
footer {
    background-color: #f8f9fa;
}
.links{
    color: #6C757D;
    cursor: pointer;
    text-decoration: none;
}
.links:hover{
    color: #EF5B65;
    text-decoration: none;
}
</style>