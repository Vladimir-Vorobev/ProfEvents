<template>
<div>
    <footer class="mt-auto py-3">
        <!-- <div class="container">
            <span class="text-muted">Разработано DARK_LED</span>
        </div> -->
        <div class="row" style="margin: 0px">
            <div class="col-12 col-lg-4 lb1">
                <a href="#" class="links" @click="showPolitics()">Политика конфиденциальности</a>
            </div>
            <div class="col-12 col-lg-4 lb2">
                <a href="#" class="link_Led">Разработано DARK_LED</a>
            </div>
            <div class="col-12 col-lg-4 lb3">
                <a href="#" class="links" @click="findBug()">Нашли ошибку?</a>
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
                    '<input type="file" class="form-control-file" id="Bug_screen">',
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

        showPolitics(){
            Swal.fire({
                title: 'Политика конфиденциальности',
                html: '<span>Test msg</span>',
                showCloseButton: true,
                showConfirmButton: false,
            })
        },
    },
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
.link_Led{
    color: black;
    cursor: pointer;
    text-decoration: none;
}
.link_Led:hover{
    color: #EF5B65;
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
</style>