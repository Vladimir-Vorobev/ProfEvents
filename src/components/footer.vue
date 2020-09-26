<template>
<div>
    <footer class="mt-auto py-3">
        <div class="row" style="margin: 0px">
            <div class="col-12 col-lg-4 lb1">
                <router-link to="/privacy" class="router-link">
                    <a ref="PrivacyPolicy" class="links">Политика конфиденциальности</a>
                </router-link>
            </div>
            <div class="col-12 col-lg-4 lb2">
                <a href="#" class="links">Разработано DARK_LED</a>
            </div>
            <div class="col-12 col-lg-2 lb3">
                <a href="#" class="links" @click="findBug()">Нашли ошибку?</a>
            </div>
            <div class="col-12 col-lg-2 lb3">
                <div class="icons">
                    <div class="icon"><a href="#"><i class="fab fa-instagram"></i></a></div>
                    <div class="icon"><a href="#"><i class="fab fa-vk"></i></a></div>
                    <div class="icon"><a href="#"><i class="fab fa-facebook"></i></a></div>
                </div>
            </div>
        </div>
    </footer>
</div>

</template>


<script>
import Swal from 'sweetalert2'
export default {
    name: 'Footer',

    methods: {
        findBug(){
            (async () => {

                const { value: formValues } = await Swal.fire({
                title: 'Добавте описание и фото ошибки',
                html:
                    '<textarea class="form-control" id="Bug_comment"></textarea>' +
                    '<br>' +
                    '<input type="file" class="form-control-file" id="Bug_screen">'+
                    '<style>.swal2-close:focus{ outline: 0 }</style>',
                focusConfirm: false,
                confirmButtonText: 'Отправить',
                confirmButtonColor: '#2096ff',
                buttonsStyling: false,
                allowOutsideClick: false,
                showCloseButton: true,
                customClass: {
                    confirmButton: 'btn btn-rounded-blue'
                },
                preConfirm: () => {
                    return [
                    document.getElementById('Bug_comment').value,
                    document.getElementById('Bug_screen').value
                    ]
                }
                })
                if (formValues) {
                    Swal.fire(JSON.stringify(formValues))   
                }

            })()
        },
    },
}
</script>

<style scoped>
footer {
    background-color: #efefef;
}
.links{
    color: black;
    cursor: pointer;
    text-decoration: none;
    font-family: 'PT Mono', monospace;
    font-weight: bold;
}
.links:hover{
    color: #EF5B65;
    text-decoration: none;
}
.link_Led:hover{
    color: #EF5B65;
    text-decoration: none;
}
.router-link{
    text-decoration: none;
}
 @media (max-width: 992px) {
        .lb1{
            padding: 0px;
            text-align: center !important;
        }
        .lb2{
            padding: 0px;
            text-align: center !important;
        }
        .lb3{
            padding: 0px;
            text-align: center !important;
        }
}
 @media (min-width: 993px) {
        .lb1{
            text-align: center !important;
        }
        .lb2{
            text-align: center !important;
        }
        .lb3{
            text-align: center !important;
        }
}
.icons{
    display: table;
    width: 100%;
}
.icon{
    display: table-cell;
}
.icon a{
    text-decoration: none;
    color: #c60000;
}
.icon a:hover{
    color: #e90000;
}
</style>