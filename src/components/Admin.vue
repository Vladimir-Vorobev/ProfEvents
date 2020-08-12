<template>
    <div class='main'>
        <div class="container warp">
            <transition-group name="auth">
                <div v-if="show" key="div">
                    <form class="formbox">
                        <h2>Войти</h2>
                        <div class="form-group row">
                            <label for="exampleInputEmail1">Email адресс</label>
                            <input name="email" class="form-control formInput" placeholder="example@gmail.com">
                        </div>
                        <div class="form-group row">
                            <label for="exampleInputPassword1">Пароль</label>
                            <input type="password" class="form-control formInput" name="password" placeholder="Пароль">
                        </div>
                        <div class="form-group row"> 
                            <button class="btn btn-rounded-blue btn-lg" @click="loginUser()">Войти в админ панель</button>
                        </div>
                    </form>
                </div>
                <div v-if="!show" key="div">
                    <ul key="ul" class="nav nav-pills almbb-pills mb-3" id="pills-tab" role="tablist" v-if="role == 'teacher'">
                        <li class="nav-item" role="presentation">
                            <a class="nav-link active" @click="showList()" id="pills-home-tab" data-toggle="pill" role="tab" aria-selected="true">Список класса</a>
                        </li>
                        <li class="nav-item" role="presentation">
                            <a class="nav-link" @click="showTop(), showInfo('teacher', email)" id="pills-home-tab" data-toggle="pill" role="tab" aria-selected="false">Рейтинг класса</a>
                        </li>
                        <li class="nav-item" role="presentation">
                            <a class="nav-link" @click="showModerationList(), getModerationList()" id="pills-home-tab" data-toggle="pill" role="tab" aria-selected="false">Модерация посещаемости</a>
                        </li>
                        <li class="nav-item" role="presentation">
                            <a class="nav-link" @click="showAdd()" id="pills-home-tab" data-toggle="pill" role="tab" aria-selected="false">Обновить список</a>
                        </li>
                    </ul>
                    <ul key="ul" class="nav nav-pills almbb-pills mb-3" id="pills-tab" role="tablist" v-if="role == 'school-admin'">
                        <li class="nav-item" role="presentation">
                            <a class="nav-link active" @click="showList()" id="pills-home-tab" data-toggle="pill" role="tab" aria-selected="true">Список учителей</a>
                        </li>
                        <li class="nav-item" role="presentation">
                            <a class="nav-link" @click="showTop(), showInfo('school', email)" id="pills-home-tab" data-toggle="pill" role="tab" aria-selected="false">Рейтинг школы</a>
                        </li>
                        <li class="nav-item" role="presentation">
                            <a class="nav-link" @click="showAdd()" id="pills-home-tab" data-toggle="pill" role="tab" aria-selected="false">Обновить список</a>
                        </li>
                    </ul>
                    <ul key="ul" class="nav nav-pills almbb-pills mb-3" id="pills-tab" role="tablist" v-if="role == 'admin'">
                        <li class="nav-item" role="presentation">
                            <a class="nav-link active schoolList" @click="showSchoolList()" id="pills-home-tab" data-toggle="pill" role="tab" aria-selected="true">Список школ</a>
                        </li>
                        <li class="nav-item" role="presentation">
                            <a class="nav-link list" @click="showList()" id="pills-home-tab" data-toggle="pill" role="tab" aria-selected="false">Список учителей</a>
                        </li>
                        <li class="nav-item" role="presentation">
                            <a class="nav-link" @click="showTop()" id="pills-home-tab" data-toggle="pill" role="tab" aria-selected="false">Рейтинг школ</a>
                        </li>
                        <li class="nav-item" role="presentation">
                            <a class="nav-link" @click="showSchoolAdd()" id="pills-home-tab" data-toggle="pill" role="tab" aria-selected="false">Добавить школьного администратора</a>
                        </li>
                        <li class="nav-item" role="presentation">
                            <a class="nav-link" @click="showAdd(), getAdmins()" id="pills-home-tab" data-toggle="pill" role="tab" aria-selected="false">Cписок администраторов</a>
                        </li>
                    </ul>
                    <div key="div" class="tab-content" id="pills-tabContent">
                        <hr>
                        <div v-if="role == 'teacher'">
                            <div v-if="ShowList">
                                <transition-group name="main">
                                    <div class="tab-pane fade show active" id="pills-list-student" v-for="item in students" :key="item.person">
                                        <a class="person" href="#" @click="showInfo(item.email, email)">
                                            <div class="person_box">
                                                <div class="name row">
                                                    <div class="name_group col-10">{{ item.person }} </div>
                                                    <div class="col-1 ar-collapse" :id='item.email'></div>
                                                    <div class="col-1" ><button class="btn btn-almbb-danger btn-almbb-small" @click="deletePerson(item.email, item.name, item.surname, 'student')"><i class="fas fa-trash-alt"></i></button></div>
                                                </div>
                                                <div :id="item.email + 'n'" style="display: none;">
                                                    <div style="text-align: center;"><i class='fa fa-spinner fa-pulse fa-3x' :id='item.email + "x"' style="display: inline-block;"></i></div>
                                                    <form :id="'form' + item.email">
                                                        <input class="radio" :name="'donaught' + item.email" type="radio" value="donaught" checked @click="changeInfo(item.email, 'donaught')"> Кругова диаграмма
                                                        <input class="radio" :name="'bar' + item.email" type="radio" value="bar" @click="changeInfo(item.email, 'bar')"> Столбчатая диаграмма
                                                        <input class="radio" :name="'full' + item.email" type="radio" value="full" @click="changeInfo(item.email, 'full')"> Полная статистика
                                                    </form>
                                                    <div class="chart-container" :id="'chartDiv' + item.email" style="display: none;"><canvas :id="'chart' + item.email"></canvas></div>
                                                    <div class="chart-container" :id="'chartDiv2' + item.email" style="display: none;"><canvas :id="'chart2' + item.email"></canvas></div>
                                                    <div :id="'chartDiv3' + item.email" style="display: none;">
                                                        <div v-if="data[item.email] != undefined && studentEvents[data.lastIndexOf(item.email)][0].length == 0"><h3>Нет мероприятий</h3></div>
                                                        <div class="card" v-for="item3 in studentEvents[data.lastIndexOf(item.email)]" :key="item3.value">
                                                            <div class="card-header">{{item3.date}}</div>
                                                            <div class="card-body">
                                                                <div class="row">
                                                                    <h5 class="card-title col-11">{{item3.name}}</h5>
                                                                    <h5><button class="btn btn-almbb-danger btn-almbb-small" @click="deletePerson(item.email, item.name, item.surname, 'student')"> <i class="fas fa-trash-alt"></i> </button></h5>
                                                                </div>
                                                            </div>
                                                        </div>
                                                    </div>
                                                </div>
                                            </div>
                                        </a>
                                    </div>
                                </transition-group>
                            </div>
                            <div v-if="ShowTop">
                                <transition-group name="main">
                                    <div key="div">
                                        <div style="text-align: center;"><i class='fa fa-spinner fa-pulse fa-3x' :id='"teacherx"' style="display: inline-block;"></i></div>
                                        <form :id="'formteacher'">
                                            <input class="radio" :name="'donaughtteacher'" type="radio" value="donaught" checked @click="changeInfo('teacher', 'donaught')"> Кругова диаграмма
                                            <input class="radio" :name="'barteacher'" type="radio" value="bar" @click="changeInfo('teacher', 'bar')"> Столбчатая диаграмма
                                        </form>
                                        <div class="chart-container" :id="'chartDivteacher'" style="display: none;"><canvas :id="'chartteacher'"></canvas></div>
                                        <div class="chart-container" :id="'chartDiv2'" style="display: none;"><canvas :id="'chart2teacher'"></canvas></div>
                                    </div>
                                </transition-group>
                            </div>
                            <div v-if="ShowModerationList">
                                <transition-group name="main">
                                    <div class="tab-pane fade show active" id="pills-list-student" v-for="item in students_on_moderation" :key="item.email">
                                        <a class="person" href="#">
                                            <div class="person_box" v-on:click="showModerationIvents(item.email)">
                                                <div class="name row">
                                                    <div class="name_group col-10">{{ item.name + ' ' + item.surname }}</div>
                                                    <div class="col-1 ar-collapse" :id='item.email + "s"'></div>
                                                </div>
                                                <div :id='item.email' style="display: none;">
                                                    <div v-for="item2 in item.events" :key="item2.data">
                                                        <a class="person" href="#" @click="showModerationInfo(item.email, item2.data.link)">
                                                            <div class="person_box a">
                                                                <div class="name row">
                                                                    <div class="name_group col-11 a">{{ item2.data.name }} </div>
                                                                    <div class="col-1 ar-collapse a" :id='item2.data.link + "n"'></div>
                                                                </div>
                                                                <div :id="item2.data.link" style="display: none;">
                                                                    <div class="row" v-for="item3 in item2.moderImg" :key="item3.file">
                                                                        <div class=" text-center col-12 col-lg-6" v-lazy-container="{ selector: 'img' }">
                                                                            <img class="moderationImg" width="300" height="200" :data-src="item3.file" alt="">
                                                                        </div>
                                                                    </div>
                                                                    <div class="row" style="margin-top: 1em">
                                                                        <div class="col-12 col-lg-6" style="text-align: center">
                                                                            <button class="btn btn-almbb-success" @click="moderate_event(item2.data, 'confirm', item.email)">Подтвердить</button>
                                                                        </div>
                                                                        <div class="col-12 col-lg-6" style="text-align: center">
                                                                            <button class="btn btn-almbb-danger" @click="moderate_event(item2.data, 'dismiss', item.email)">Отклонить</button>
                                                                        </div>
                                                                    </div>
                                                                </div>
                                                            </div>
                                                        </a>    
                                                    </div>
                                                </div>
                                            </div>
                                        </a>
                                    </div>
                                </transition-group>
                            </div>
                            <div v-if="ShowAdd">
                                <transition-group name="main">
                                    <form key='form' id='formList' style="text-align: left; font-size: 1.2em">
                                        <!-- <input key="input" class="radio" name='list' type="radio" checked @click="changeAddInfo('list')"> <p key="p">Добавить список учеников</p> -->
                                        <!-- <input key="input" class="radio" name='one' type="radio" @click="changeAddInfo('one')"> <p key="p">Добавить ученика</p> -->
                                        <div class="form-check row">
                                            <div class="col-12">
                                                <input key="input" name='list' class="form-check-input radio" type="radio" @click="changeAddInfo('list')" id="exampleRadios1" value="option1" checked>
                                                <label key="p" class="form-check-label" for="exampleRadios1">
                                                    Добавить список учеников
                                                </label>
                                            </div>
                                        </div>
                                        <div class="form-check row">
                                            <div class="col-12">
                                                <input  key="input" name='one' class="form-check-input radio" @click="changeAddInfo('one')" type="radio" id="exampleRadios2" value="option2">
                                                <label key="p" class="form-check-label" for="exampleRadios2">
                                                    Добавить ученика
                                                </label>
                                            </div>
                                        </div>
                                    </form> <br key='br'>
                                    <div key="div" v-if="ShowAddList" style="text-align: left">
                                        <p key="p" style="font-size:1.3em; text-align: center"> Загрузите актуальный список Вашего класса в excel файле </p>
                                        <p><input type="file" ref="file" class="form-control-file" @change="file()" key="input"></p>
                                        <p><button type="submit" @click="add('!1', 'student')" class="btn btn-primary btn-lg" key="button">Обновить список</button></p>
                                    </div>
                                    <form key="form" id='formOne' v-if="ShowAddOne" style="text-align: left">
                                        <div class="row">
                                            <div class="col-12 col-md-6">
                                                <div class="input-group mb-3">
                                                    <div class="input-group-prepend">
                                                        <span class="input-group-text" id="basic-addon1">Имя</span>
                                                    </div>
                                                    <input type="text" key="input" name="name" class="form-control name" aria-describedby="basic-addon1">
                                                </div>
                                            </div>
                                            <div class="col-12 col-md-6">
                                                <div class="input-group mb-3">
                                                    <div class="input-group-prepend">
                                                        <span class="input-group-text" id="basic-addon2">Фамилия</span>
                                                    </div>
                                                    <input type="text" key="input" name="surname" class="form-control surname" aria-describedby="basic-addon2">
                                                </div>
                                            </div>
                                        </div>
                                        <div class="row">
                                            <div class="col-12">
                                                <div class="input-group mb-3">
                                                    <div class="input-group-prepend">
                                                        <span class="input-group-text" id="basic-addon4">email</span>
                                                    </div>
                                                    <input class="form-control email" key="input" name="email" placeholder="example@gmail.com" aria-describedby="basic-addon4">
                                                </div>
                                            </div>
                                        </div>
                                        <!-- <p key="p">Email </p><input key="input" name="email">
                                        <p key="p">Имя </p><input key="input" name="name">
                                        <p key="p">Фамилия </p><input key="input" name="surname" style="margin-bottom: 0.7em"> -->
                                        <p><button type="submit" @click="add('one', 'student')" class="btn btn-primary btn-lg" key="button">Добавить ученика</button></p>
                                    </form>
                                </transition-group>
                            </div>
                        </div>
                
                        <div v-if="role == 'school-admin'">
                            <div v-if="ShowList">
                                <transition-group name="main">
                                    <div class="tab-pane fade show active" id="pills-list-student" v-for="item in teachers" :key="item.person">
                                        <a class="person" href="#">
                                            <div class="person_box" v-on:click="showTeacherInfo(item.email)">
                                                <div class="name row">
                                                    <div class="name_group col-10">{{ item.person }}</div>
                                                    <div class="col-1 ar-collapse" :id='item.email'></div>
                                                    <div class="col-1" ><button class="btn btn-almbb-danger btn-almbb-small" @click="deletePerson(item.email, item.name, item.surname, 'teacher')"> <i class="fas fa-trash-alt"></i> </button></div>
                                                </div>
                                                <div :id='item.email + "s"' style="display: none;">
                                                    <div v-for="item2 in students[item.email]" :key="item2.student" :class="item2.email">
                                                        <a class="person" href="#" @click="showInfo(item2.email, item.email)">
                                                            <div class="person_box a">
                                                                <div class="name row">
                                                                    <div class="name_group col-11 a">{{ item2.student }} </div>
                                                                    <div class="col-1 ar-collapse a" :id='item2.email'></div>
                                                                </div>
                                                                <div :id="item2.email + 'n'" style="display: none;">
                                                                    <div style="text-align: center;"><i class='fa fa-spinner fa-pulse fa-3x' :id='item2.email + "x"' style="display: inline-block;"></i></div>
                                                                    <form :id="'form' + item2.email">
                                                                        <input class="radio" :name="'donaught' + item2.email" type="radio" value="donaught" checked @click="changeInfo(item2.email, 'donaught')"> Кругова диаграмма
                                                                        <input class="radio" :name="'bar' + item2.email" type="radio" value="bar" @click="changeInfo(item2.email, 'bar')"> Столбчатая диаграмма
                                                                        <input class="radio" :name="'full' + item2.email" type="radio" value="full" @click="changeInfo(item2.email, 'full')"> Полная статистика
                                                                    </form>
                                                                    <div class="chart-container" :id="'chartDiv' + item2.email" style="display: none;"><canvas :id="'chart' + item2.email"></canvas></div>
                                                                    <div class="chart-container" :id="'chartDiv2' + item2.email" style="display: none;"><canvas :id="'chart2' + item2.email"></canvas></div>
                                                                    <div :id="'chartDiv3' + item2.email" style="display: none;">
                                                                        <div v-if="studentEvents[item2.email] != undefined && studentEvents[item2.email].length == 0"><h3>Нет мероприятий</h3></div>
                                                                        <div class="card" v-for="item3 in studentEvents[item2.email]" :key="item3.value">
                                                                            <div class="card-header">{{item3.date}}</div>
                                                                            <div class="card-body">
                                                                                <div class="row">
                                                                                    <h5 class="card-title col-11">{{item3.name}}</h5>
                                                                                </div>
                                                                            </div>
                                                                        </div>
                                                                    </div>
                                                                </div>
                                                            </div>
                                                        </a>    
                                                    </div>
                                                </div>
                                            </div>
                                        </a>
                                    </div>
                                </transition-group>
                            </div>
                            <div v-if="ShowTop">
                                <transition-group name="main">
                                    <div key="div">
                                        <div style="text-align: center;"><i class='fa fa-spinner fa-pulse fa-3x' :id='"schoolx"' style="display: inline-block;"></i></div>
                                        <form :id="'formschool'">
                                            <input class="radio" :name="'donaughtschool'" type="radio" value="donaught" checked @click="changeInfo('school', 'donaught')"> Кругова диаграмма
                                            <input class="radio" :name="'barschool'" type="radio" value="bar" @click="changeInfo('school', 'bar')"> Столбчатая диаграмма
                                        </form>
                                        <div class="chart-container" :id="'chartDivschool'" style="display: none;"><canvas :id="'chartschool'"></canvas></div>
                                        <div class="chart-container" :id="'chartDiv2'" style="display: none;"><canvas :id="'chart2school'"></canvas></div>
                                    </div>
                                </transition-group>
                            </div>
                            <div v-if="ShowAdd">
                               <transition-group name="main">
                                    <form key='form' id='formList' style="text-align: left; font-size: 1.2em">
                                        <!-- <input key="input" class="radio" name='list' type="radio" checked @click="changeAddInfo('list')"> <p key="p">Добавить список учеников</p> -->
                                        <!-- <input key="input" class="radio" name='one' type="radio" @click="changeAddInfo('one')"> <p key="p">Добавить ученика</p> -->
                                        <div class="form-check row">
                                            <div class="col-12">
                                                <input key="input" name='list' class="form-check-input radio" type="radio" @click="changeAddInfo('list')" id="exampleRadios1" value="option1" checked>
                                                <label key="p" class="form-check-label" for="exampleRadios1">
                                                    Добавить список учителей
                                                </label>
                                            </div>
                                        </div>
                                        <div class="form-check row">
                                            <div class="col-12">
                                                <input  key="input" name='one' class="form-check-input radio" @click="changeAddInfo('one')" type="radio" id="exampleRadios2" value="option2">
                                                <label key="p" class="form-check-label" for="exampleRadios2">
                                                    Добавить учителя
                                                </label>
                                            </div>
                                        </div>
                                    </form> <br key='br'>
                                    <div key="div" v-if="ShowAddList" style="text-align: left">
                                        <p key="p" style="font-size:1.3em; text-align: center"> Загрузите актуальный список Ваших учителей в excel файле </p>
                                        <p><input type="file" ref="file" class="form-control-file" @change="file()" key="input"></p>
                                        <p><button type="submit" @click="add('!1', 'teacher')" class="btn btn-blue btn-lg" key="button">Обновить список</button></p>
                                    </div>
                                    <form key="form" id='formOne' v-if="ShowAddOne" style="text-align: left">
                                        <div class="row">
                                            <div class="col-12 col-md-6">
                                                <div class="input-group mb-3">
                                                    <div class="input-group-prepend">
                                                        <span class="input-group-text" id="basic-addon1">Имя</span>
                                                    </div>
                                                    <input type="text" key="input" name="name" class="form-control name" aria-describedby="basic-addon1">
                                                </div>
                                            </div>
                                            <div class="col-12 col-md-6">
                                                <div class="input-group mb-3">
                                                    <div class="input-group-prepend">
                                                        <span class="input-group-text" id="basic-addon2">Фамилия</span>
                                                    </div>
                                                    <input type="text" key="input" name="surname" class="form-control surname" aria-describedby="basic-addon2">
                                                </div>
                                            </div>
                                        </div>
                                        <div class="row">
                                            <div class="col-12">
                                                <div class="input-group mb-3">
                                                    <div class="input-group-prepend">
                                                        <span class="input-group-text" id="basic-addon4">email</span>
                                                    </div>
                                                    <input class="form-control email" key="input" name="email" placeholder="example@gmail.com" aria-describedby="basic-addon4">
                                                </div>
                                            </div>
                                        </div>
                                        <!-- <p key="p">Email </p><input key="input" name="email">
                                        <p key="p">Имя </p><input key="input" name="name">
                                        <p key="p">Фамилия </p><input key="input" name="surname" style="margin-bottom: 0.7em"> -->
                                        <p><button type="submit" @click="add('one', 'teacher')" class="btn btn-blue btn-lg" key="button">Добавить учителя</button></p>
                                    </form>
                                </transition-group>
                            </div>
                        </div>
                        <div v-if="role == 'admin'">
                            <div v-if="ShowSchoolList">
                                <transition-group name="main">
                                    <div v-for="item in schools" :key="item.school">
                                        <div class="card school" @click="getSchoolInfo(item.school)">
                                            <div class="card-body school-body">
                                                {{item.school}}
                                            </div>
                                        </div> <br>
                                        <!-- <a href='#' class='school' @click="getSchoolInfo(item.school)">{{item.school}}</a><br> -->
                                    </div>
                                </transition-group>
                            </div>
                            <div v-if="showAdminInfo">
                                <transition-group name="main">
                                    <form id='schoolInfo' key="form">
                                        <input name='school' :value="schoolName">
                                        <button type='submit' @click="getSchoolInfo()">Найти информацию по школе</button>
                                    </form> <br key='br'>
                                    <form id="formForList" key="form">
                                        <input class="radio" name="school" type="radio" checked @click="changeInfo('school')"> Список школы
                                        <input class="radio" name="admin" type="radio" @click="changeInfo('admin')"> Список администраторов школы
                                    </form>
                                    <div v-if="ShowList" key="div">
                                        <div class="tab-pane fade show active" id="pills-list-student" v-for="item in teachers" :key="item.person">
                                            <a class="person" href="#">
                                                <div class="person_box" v-on:click="getAdminList(item.email, true), showTeacherInfo(item.email)">
                                                    <div class="name row">
                                                        <div class="name_group col-11">{{ item.person }}</div>
                                                        <div class="col-1 ar-collapse" :id='item.email'></div>
                                                    </div>
                                                    <div :id='item.email + "s"' style="display: none;">
                                                        <div v-for="item2 in students[item.email]" :key="item2.student" :class="item2.email">
                                                            <a class="person" href="#" @click="showInfo(item2.email, item.email)">
                                                                <div class="person_box a">
                                                                    <div class="name row">
                                                                        <div class="name_group col-11 a">{{ item2.student }} </div>
                                                                        <div class="col-1 ar-collapse a" :id='item2.email'></div>
                                                                    </div>
                                                                    <div :id="item2.email + 'n'" style="display: none;">
                                                                        <div style="text-align: center;"><i class='fa fa-spinner fa-pulse fa-3x' :id='item2.email + "x"' style="display: inline-block;"></i></div>
                                                                        <form :id="'form' + item2.email">
                                                                            <input class="radio" :name="'donaught' + item2.email" type="radio" value="donaught" checked @click="changeInfo(item2.email, 'donaught')"> Кругова диаграмма
                                                                            <input class="radio" :name="'bar' + item2.email" type="radio" value="bar" @click="changeInfo(item2.email, 'bar')"> Столбчатая диаграмма
                                                                            <input class="radio" :name="'full' + item2.email" type="radio" value="full" @click="changeInfo(item2.email, 'full')"> Полная статистика
                                                                        </form>
                                                                        <div class="chart-container" :id="'chartDiv' + item2.email" style="display: none;"><canvas :id="'chart' + item2.email"></canvas></div>
                                                                        <div class="chart-container" :id="'chartDiv2' + item2.email" style="display: none;"><canvas :id="'chart2' + item2.email"></canvas></div>
                                                                        <div :id="'chartDiv3' + item2.email" style="display: none;">
                                                                            <div v-if="studentEvents[item2.email] != undefined && studentEvents[item2.email].length == 0"><h3>Нет мероприятий</h3></div>
                                                                            <div class="card" v-for="item3 in studentEvents[item2.email]" :key="item3.value">
                                                                                <div class="card-header">{{item3.date}}</div>
                                                                                <div class="card-body">
                                                                                    <div class="row">
                                                                                        <h5 class="card-title col-11">{{item3.name}}</h5>
                                                                                    </div>
                                                                                </div>
                                                                            </div>
                                                                        </div>
                                                                    </div>
                                                                </div>
                                                            </a>    
                                                        </div>
                                                    </div>
                                                </div>
                                            </a>
                                        </div>
                                    </div>
                                    <div v-if="ShowAdminList" key="div">
                                        <div class="tab-pane fade show active" id="pills-list-student" v-for="item in schoolAdmins" :key="item.person">
                                            <div class="person">
                                                <div class="person_box">
                                                    <div class="name row">
                                                        <div class="name_group col-11">{{ item.person }}</div>
                                                        <div class="col-1" ><button class="btn btn-almbb-danger btn-almbb-small" @click="deletePerson(item.email, item.name, item.surname, 'school-admin')"><i class="fas fa-trash-alt"></i></button></div>
                                                    </div>
                                                </div>
                                            </div>
                                        </div>
                                    </div>
                                </transition-group>   
                            </div>
                            <div v-if="ShowTop">
                                <transition-group name="main">
                                    <p key="p">Рейтинг тут</p>
                                </transition-group>
                            </div>
                            <div v-if="ShowSchoolAdd">
                               <transition-group name="main">
                                    <form key="form" id='formOne' style="text-align: left">
                                        <div class="row">
                                            <div class="col-12 col-md-6">
                                                <div class="input-group mb-3">
                                                    <div class="input-group-prepend">
                                                        <span class="input-group-text" id="basic-addon1">Номер</span>
                                                    </div>
                                                    <input type="text" key="input" name="name" class="form-control name" aria-describedby="basic-addon1">
                                                </div>
                                            </div>
                                            <div class="col-12 col-md-6">
                                                <div class="input-group mb-3">
                                                    <div class="input-group-prepend">
                                                        <span class="input-group-text" id="basic-addon2">Фамилия</span>
                                                    </div>
                                                    <input type="text" key="input" name="surname" class="form-control surname" aria-describedby="basic-addon2">
                                                </div>
                                            </div>
                                            <div class="col-12 col-md-6">
                                                <div class="input-group mb-3">
                                                    <div class="input-group-prepend">
                                                        <span class="input-group-text" id="basic-addon2">Email</span>
                                                    </div>
                                                    <input type="text" key="input" name="email" class="form-control surname" aria-describedby="basic-addon2">
                                                </div>
                                            </div>
                                            <div class="col-12 col-md-6">
                                                <div class="input-group mb-3">
                                                    <div class="input-group-prepend">
                                                        <span class="input-group-text" id="basic-addon2">Номер школы</span>
                                                    </div>
                                                    <input type="text" key="input" name="school" class="form-control surname" aria-describedby="basic-addon2">
                                                </div>
                                            </div>
                                        </div>
                                        <!-- <p key="p">Email </p><input key="input" name="email">
                                        <p key="p">Имя </p><input key="input" name="name">
                                        <p key="p">Фамилия </p><input key="input" name="surname" style="margin-bottom: 0.7em"> -->
                                        <p><button type="submit" @click="add('school-admin', 'school-admin')" class="btn btn-blue btn-lg" key="button">Добавить школьного администратора</button></p>
                                    </form>
                                </transition-group>
                            </div>
                            <div v-if="ShowAdd">
                               <transition-group name="main">
                                    <form id="formForAdmin" key="form">
                                        <input class="radio" name="adminList" type="radio" checked @click="changeInfo('adminList')"> Список администраторов
                                        <input class="radio" name="addAdmin" type="radio" @click="changeInfo('addAdmin')"> Добавить администратора
                                    </form>
                                    <div v-if="AdminList" key="div">
                                        <div class="tab-pane fade show active" id="pills-list-student" v-for="item in admins" :key="item.person">
                                            <div class="person">
                                                <div class="person_box">
                                                    <div class="name row">
                                                        <div class="name_group col-11">{{ item.person }}</div>
                                                        <div class="col-1" ><button class="btn btn-almbb-danger btn-almbb-small" @click="deletePerson(item.email, item.name, item.surname, 'admin')"><i class="fas fa-trash-alt"></i></button></div>
                                                    </div>
                                                </div>
                                            </div>
                                        </div>
                                    </div>
                                    <form key="form" id='formOne' style="text-align: left" v-if="AddAdmin">
                                        <div class="row">
                                            <div class="col-12 col-md-6">
                                                <div class="input-group mb-3">
                                                    <div class="input-group-prepend">
                                                        <span class="input-group-text" id="basic-addon1">Имя</span>
                                                    </div>
                                                    <input type="text" key="input" name="name" class="form-control name" aria-describedby="basic-addon1">
                                                </div>
                                            </div>
                                            <div class="col-12 col-md-6">
                                                <div class="input-group mb-3">
                                                    <div class="input-group-prepend">
                                                        <span class="input-group-text" id="basic-addon2">Фамилия</span>
                                                    </div>
                                                    <input type="text" key="input" name="surname" class="form-control surname" aria-describedby="basic-addon2">
                                                </div>
                                            </div>
                                        </div>
                                        <div class="row">
                                            <div class="col-12">
                                                <div class="input-group mb-3">
                                                    <div class="input-group-prepend">
                                                        <span class="input-group-text" id="basic-addon4">email</span>
                                                    </div>
                                                    <input class="form-control email" key="input" name="email" placeholder="example@gmail.com" aria-describedby="basic-addon4">
                                                </div>
                                            </div>
                                        </div>
                                        <!-- <p key="p">Email </p><input key="input" name="email">
                                        <p key="p">Имя </p><input key="input" name="name">
                                        <p key="p">Фамилия </p><input key="input" name="surname" style="margin-bottom: 0.7em"> -->
                                        <p><button type="submit" @click="add('admin', 'admin')" class="btn btn-blue btn-lg" key="button">Добавить админа</button></p>
                                    </form>
                                </transition-group>
                            </div>
                        </div> 
                    </div>
                </div>
            </transition-group>
        </div>
        <div class="footer"><Footer></Footer></div> 
    </div>
</template>

<script>
import needle from 'needle'
import readXlsxFile from 'read-excel-file'
import Chart from 'chart.js'
import Footer from './footer.vue'
import Vue from 'vue'
export default {
    name: 'Admin',
    components: { Footer },
    data(){
        return{
            show: true,
            role: '',
            students: [],
            teachers: [],
            admins: [],
            schoolAdmins: [],
            email: this.$store.getters.email,
            SessionID: this.$store.getters.SessionID,
            classData: [],
            ShowList: true,
            ShowAdminList: false,
            AdminList: true,
            AddAdmin: false,
            ShowSchoolList: false,
            ShowTop: false,
            ShowAdd: false,
            ShowSchoolAdd: false,
            ShowAddList: true,
            showAdminInfo: false,
            ShowAddSchoolList: false,
            ShowAddOne: false,
            ShowAddSchoolOne: false,
            ShowModerationList: false,
            data: [],
            studentEvents: [],
            schools: [],
            schoolName: '',
            students_on_moderation: {},
        }
    },
    methods:{
        loginUser(){
            event.preventDefault()
            let form = document.forms[0]
            let email = form.elements.email.value
            let password = form.elements.password.value
            let crypto = require('crypto')
            fetch(this.$store.state.serverIp+'/api/adminLogin', {
                method: 'POST',
                headers: {email: email, password: crypto.createHash('md5').update(password).digest("hex")},
            })
            .then(response => {
                console.log("res", response)
                return response.json()
            })
            .then(data => {
                if(data == 'Incorect password'){
                    //alert('Пользователь не найден')
                    this.$swal({
                        icon: 'error',
                        text: 'Пользователь не найден'
                    });
                }
                else if(data != 'user' && data != 'student'){
                    this.show = false
                    this.role = data
                    if(this.role != 'admin') this.getAdminList(this.email)
                    else{
                        this.ShowList = false
                        this.ShowSchoolList = true
                        this.getSchoolList(this.email)
                    }
                }
                else{
                    //alert("Неверный email или пароль")
                    this.$swal({
                        icon: 'error',
                        text: 'Неверный email или пароль или у Вас нет доступа к административной панели'
                    });
                }
                console.log(data)
            })
            .catch(err => {
                console.log(err)
            })
            // this.students = [{person: 'Иванова Мария', email: 'v11ru'}, {person: 'Иванов Иван', email: 'v12ru'}, {person: 'Сергеев Сергей', email: 'v13ru'}]
            // this.teachers = [{person: 'Иванова Мария', email: 'v14ru'}, {person: 'Иванов Иван', email: 'v15ru'}, {person: 'Сергеев Сергей', email: 'v16ru'}]
        },
        getAdminList(email, teacher){
            let people = []
            fetch(this.$store.state.serverIp+'/api/getAdminList', {
                method: 'POST',
                headers: {email: email, sessionid: this.SessionID},
            })
            .then(response => {
                console.log("res", response)
                return response.json()
            })
            .then(data => {
                console.log(data)
                if(this.role == 'teacher'){
                    for(let i = 0; i < data.length; i++){
                        people.push({person: data[i].name + ' ' + data[i].surname, email: data[i].email})
                        this.data.push(data[i].email)
                        this.studentEvents.push([])
                    }
                    this.students = people
                }
                else if(this.role == 'school-admin' || this.role == 'admin'){
                    if(teacher){
                        if(this.students[email].length == 0){
                            for(let i = 0; i < data.length; i++){
                                this.students[email].push({student: data[i].name + ' ' + data[i].surname, email: data[i].email})
                            }
                            console.log(this.students)
                        }
                    }
                    else{
                        for(let i = 0; i < data.length; i++){
                            people.push({person: data[i].name + ' ' + data[i].surname, email: data[i].email})
                            this.data.push(data[i].email)
                            this.$set(this.students, data[i].email, [])
                            console.log(this.students)
                            //this.studentEvents.push([])
                        }
                        this.teachers = people
                    }
                }
            })
            .catch(err => {
                console.log(err)
            })
        },
        getSchoolInfo(school){
            event.preventDefault()
            let people = []
            if(school){
                this.showList()
                document.querySelector(".school").value = school
                this.schoolName = school
                document.querySelector('.list').classList.add('active')
                document.querySelector('.schoolList').classList.remove('active')
            }
            else{
                let form = document.getElementById('schoolInfo')
                this.schoolName = form['school'].value
            }
            fetch(this.$store.state.serverIp+'/api/getListTeacher', {
                method: 'POST',
                headers: {email: this.$store.state.email, sessionid: this.SessionID, school: this.schoolName},
            })
            .then(response => {
                console.log("res", response)
                return response.json()
            })
            .then(data => {
                console.log(data)
                console.log(data[0])
                if(data[1].length == 0){
                    this.$swal({
                        icon: 'error',
                        text: 'Данная школа не зарегистрирована, проверьте точное название во вкладке списка школ'
                    });
                    return
                }
                for(let i = 0; i < data[0].length; i++){
                    people.push({person: data[0][i].name + ' ' + data[0][i].surname, email: data[0][i].email})
                    this.$set(this.students, data[0][i].email, [])
                    console.log(this.students)
                    //this.studentEvents.push([])
                }
                this.teachers = people
                people = []
                for(let i = 0; i < data[1].length; i++){
                    people.push({person: data[1][i].name + ' ' + data[1][i].surname, email: data[1][i].email})
                }
                this.schoolAdmins = people
            })
            .catch(err => {
                console.log(err)
            })
        },
        getModerationList(){
            if(Object.keys(this.students_on_moderation).length == 0){
                fetch(this.$store.state.serverIp+'/api/getModerationList', {
                    method: 'POST',
                    headers: {email: this.email, sessionid: this.SessionID},
                })
                .then(response => {
                    console.log("res", response)
                    return response.json()
                })
                .then(data => {
                    console.log(data)
                    for(let i = 0; i < data.length; i++){
                        this.$set(this.students_on_moderation, data[i].email, {name: data[i].name, surname: data[i].surname, events: data[i].events, email: data[i].email})
                    }
                    console.log(this.students_on_moderation)
                })
                .catch(err => {
                    console.log(err)
                })
            }
        },
        showInfo(email, teacherEmail){
            if(email == 'teacher' || email == 'school'){
                console.log(email)
                fetch(this.$store.state.serverIp+'/api/getRating', {
                    method: 'POST',
                    headers: {email: teacherEmail, sessionid: this.SessionID, type: email},
                })
                .then(response => {
                    console.log("res", response)
                    return response.json()
                })
                .then(data => {
                    console.log(data)
                    let ctx = document.getElementById('chart' + email)
                    let ctx2 = document.getElementById('chart2' + email)
                    makeChart('doughnut', [data.service, data.programming, 0, 0,  data.engeniring, 0], ctx)
                    makeChart('bar', [data.service, data.programming, 0, 0,  data.engeniring, 0], ctx2)
                    document.getElementById(email + "x").style.display = 'none'
                    document.getElementById('chartDiv' + email).style.display = 'block'
                })
                .catch(err => {
                    console.log(err)
                })
            }
            else if(this.role == 'teacher'){
                console.log(event.target.className)
                if(event.target.className != 'chartjs-render-monitor' && event.target.className != 'radio'){
                    for(let i = 0; i < this.students.length; i++){
                        if(document.getElementById(this.students[i].email + 'n').style.display == 'block' && this.students[i].email != email){
                            document.getElementById(this.students[i].email + 'n').style.display = 'none'
                            document.getElementById(this.students[i].email).classList.remove('ar-show');
                        }
                    }
                    if(document.getElementById(email + 'n').style.display == 'block'){
                        document.getElementById(email + 'n').style.display = 'none'
                        document.getElementById(email).classList.remove('ar-show');
                    }
                    else{
                        document.getElementById(email + 'n').style.display = 'block'
                        document.getElementById(email).classList.add('ar-show');
                        if(document.getElementById(email + "x").style.display == 'inline-block'){
                            fetch(this.$store.state.serverIp+'/api/getCheckedEvents', {
                                method: 'get',
                                headers: {adminemail: teacherEmail, studemail: email, sessionid: this.SessionID},
                            })
                            .then(response => {
                                console.log("res", response)
                                return response.json()
                            })
                            .then(datan => {
                                let statistics = datan.stat
                                for(let i = 0; i < datan.checkedEvents.length; i++){
                                    console.log(datan.checkedEvents[i].data)
                                    this.studentEvents[this.data.lastIndexOf(email)].push(datan.checkedEvents[i].data)
                                }
                                console.log(this.studentEvents)
                                let ctx = document.getElementById('chart' + email)
                                let ctx2 = document.getElementById('chart2' + email)
                                makeChart('doughnut', [statistics.service, statistics.programming, 0, 0,  statistics.engeniring, 0], ctx)
                                makeChart('bar', [statistics.service, statistics.programming, 0, 0,  statistics.engeniring, 0], ctx2)
                                document.getElementById(email + "x").style.display = 'none'
                                document.getElementById('chartDiv' + email).style.display = 'block'
                            })
                            .catch(err => {
                                console.log(err)
                            })
                        } 
                    }
                }
            }
            else if(this.role == 'school-admin' || this.role == 'admin'){
                console.log(this.students[teacherEmail])
                if(event.target.className != 'chartjs-render-monitor' && event.target.className != 'radio'){
                    for(let key in this.students[teacherEmail]){
                        console.log(this.students[teacherEmail][key].email)
                        if(document.getElementById(this.students[teacherEmail][key].email + 'n').style.display == 'block' && this.students[teacherEmail][key].email != email){
                            document.getElementById(this.students[teacherEmail][key].email + 'n').style.display = 'none'
                            document.getElementById(this.students[teacherEmail][key].email).classList.remove('ar-show');
                        }
                    }
                    if(document.getElementById(email + 'n').style.display == 'block'){
                        document.getElementById(email + 'n').style.display = 'none'
                        document.getElementById(email).classList.remove('ar-show');
                    }
                    else{
                        document.getElementById(email + 'n').style.display = 'block'
                        document.getElementById(email).classList.add('ar-show');
                        if(document.getElementById(email + "x").style.display == 'inline-block'){
                            fetch(this.$store.state.serverIp+'/api/getCheckedEvents', {
                                method: 'get',
                                headers: {studemail: email, adminemail: this.email, sessionid: this.SessionID},
                            })
                            .then(response => {
                                console.log("res", response)
                                return response.json()
                            })
                            .then(datan => {
                                let statistics = datan.stat
                                console.log(datan)
                                this.$set(this.studentEvents, email, datan.checkedEvents)
                                //this.studentEvents[email].push(datan.checkedEvents)
                                console.log(this.studentEvents)
                                let ctx = document.getElementById('chart' + email)
                                let ctx2 = document.getElementById('chart2' + email)
                                makeChart('doughnut', [statistics.service, statistics.programming, 0, 0,  statistics.engeniring, 0], ctx)
                                makeChart('bar', [statistics.service, statistics.programming, 0, 0,  statistics.engeniring, 0], ctx2)
                                document.getElementById(email + "x").style.display = 'none'
                                document.getElementById('chartDiv' + email).style.display = 'block'
                            })
                            .catch(err => {
                                console.log(err)
                            })
                        }
                    }
                } 
            }
            function makeChart(type, data, place){
                let myChart = new Chart(place, {
                    type: type,
                    data: {
                        labels: ['Сфера услуг', 'IT', 'Творчество и Дизайн', 'Строительство', 'Инжинерные технологии', 'Транспорт и логистика'],
                        datasets: [{
                            label: '# of Votes',
                            data: data,
                            backgroundColor: [
                                'rgba(255, 99, 132, 0.5)',
                                'rgba(54, 162, 235, 0.5)',
                                'rgba(255, 206, 86, 0.5)',
                                'rgba(75, 192, 192, 0.5)',
                                'rgba(153, 102, 255, 0.5)',
                                'rgba(255, 159, 64, 0.5)'
                            ],
                            borderColor: [
                                'rgba(255, 99, 132, 1)',
                                'rgba(54, 162, 235, 1)',
                                'rgba(255, 206, 86, 1)',
                                'rgba(75, 192, 192, 1)',
                                'rgba(153, 102, 255, 1)',
                                'rgba(255, 159, 64, 1)'
                            ],
                            borderWidth: 1,
                            hoverBorderWidth: 5,
                        }]
                    },
                    options: {
                        legend: {
                            position: 'bottom',
                        }
                    }
                });
                console.log(myChart)
            }
        },
        showTeacherInfo(email){
            if(event.target.className == 'person_box' || event.target.className == 'name row' || event.target.className == 'name_group col-11'|| event.target.className == 'col-1 ar-collapse' || event.target.className == 'col-1 ar-collapse ar-show'){
                for(let i = 0; i < this.teachers.length; i++){
                    if(document.getElementById(this.teachers[i].email + 's').style.display == 'block' && this.teachers[i].email != email){
                        document.getElementById(this.teachers[i].email + 's').style.display = 'none'
                        document.getElementById(this.teachers[i].email).classList.remove('ar-show');
                    }
                }
                if(document.getElementById(email + 's').style.display == 'block'){
                    document.getElementById(email + 's').style.display = 'none'
                    document.getElementById(email).classList.remove('ar-show');
                }
                else{
                    document.getElementById(email).classList.add('ar-show');
                    if(document.getElementById(email + 's').style.display == 'none'){
                        if(this.students[email].length == 0){
                            this.getAdminList(email, true)
                        }
                        document.getElementById(email + 's').style.display = 'block'
                    } 
                }
            }
        },
        showModerationIvents(email){
            if(event.target.className == 'person_box' || event.target.className == 'name row' || event.target.className == 'name_group col-10'|| event.target.className == 'col-1 ar-collapse' || event.target.className == 'col-1 ar-collapse ar-show'){
                for(let key in this.students_on_moderation){
                    if(document.getElementById(key).style.display == 'block' && key != email){
                        document.getElementById(key).style.display = 'none'
                        document.getElementById(key + 's').classList.remove('ar-show');
                    }
                }
                if(document.getElementById(email).style.display == 'block'){
                    document.getElementById(email).style.display = 'none'
                    document.getElementById(email + 's').classList.remove('ar-show');
                }
                else{
                    document.getElementById(email + 's').classList.add('ar-show');
                    if(document.getElementById(email).style.display == 'none'){
                        document.getElementById(email).style.display = 'block'
                    } 
                }
            }
        },
        showModerationInfo(email, link){
            if(event.target.className != 'chartjs-render-monitor' && event.target.className != 'radio' && event.target.classList.contains('btn') == false){
                for(let key in this.students_on_moderation[email].events.data){
                    if(document.getElementById(key.link).style.display == 'block' && key.link != link){
                        document.getElementById(key.link).style.display = 'none'
                        document.getElementById(key.link + 'n').classList.remove('ar-show');
                    }
                }
                if(document.getElementById(link).style.display == 'block'){
                    document.getElementById(link).style.display = 'none'
                    document.getElementById(link + 'n').classList.remove('ar-show');
                }
                else{
                    document.getElementById(link + 'n').classList.add('ar-show');
                    if(document.getElementById(link).style.display == 'none'){
                        document.getElementById(link).style.display = 'block'
                    } 
                }
            }
        },
        getSchoolList(email){
            fetch(this.$store.state.serverIp+'/api/getSchoolList', {
                method: 'POST',
                headers: {email: email, sessionid: this.SessionID},
            })
            .then(response => {
                console.log("res", response)
                return response.json()
            })
            .then(data => {
                for(let i = 0; i < data.length; i++){
                    this.schools.push({school: data[i]})
                }
            })
        },
        getAdmins(){
            if(this.admins.length == 0){
                let people = []
                fetch(this.$store.state.serverIp+'/api/getListAdmin', {
                    method: 'POST',
                    headers: {email: this.email, sessionid: this.SessionID},
                })
                .then(response => {
                    console.log("res", response)
                    return response.json()
                })
                .then(data => {
                    for(let i = 0; i < data.length; i++){
                        people.push({person: data[i].name + ' ' + data[i].surname, email: data[i].email})
                    }
                    this.admins = people
                    console.log(this.admins)
                })
                .catch(err => {
                    console.log(err)
                })
            }
        },
        file(){
            let data = []
            readXlsxFile(this.$refs.file.files[0]).then((rows) => {
                for(let i = 0; i < rows.length; i++){
                    data.push({email: rows[i][0], name: rows[i][1], surname: rows[i][2]})
                }
            })
            this.classData = data
        },
        add(value, type){
            event.preventDefault()
            if(value != '!1'){
                let form = document.getElementById('formOne')
                if(form['email'].value == ''){
                    this.$swal({
                        icon: 'error',
                        text: 'Введите email'
                    });
                }
                else if(form['name'].value == ''){
                    this.$swal({
                        icon: 'error',
                        text: 'Введите имя'
                    });
                }
                else if(form['surname'].value == ''){
                    this.$swal({
                        icon: 'error',
                        text: 'Введите фамилию'
                    });
                }
                else if(value == 'school-admin' && form['school'].value == ''){
                    this.$swal({
                        icon: 'error',
                        text: 'Введите название школы'
                    });
                }
                else{
                    if(value == 'school-admin'){
                        send({email: form['email'].value, name: form['name'].value, surname: form['surname'].value, school: form['school'].value}, this.email, 'uploadOne')
                    }
                    else{
                        send({email: form['email'].value, name: form['name'].value, surname: form['surname'].value}, this.email, 'uploadOne')
                    }
                }
            }
            else{
                if(this.classData.length != 0){
                    send(this.classData, this.email, 'uploadTable')
                }
                else this.$swal('Файл не выбран');   //alert('Файл не выбран')
            }
            let dopEmail = this.email
            let SessionID = this.SessionID
            function send(data, email, url){
                needle.post('http://78.155.219.12:3000/api/' + url, {data: data, email: email, type: 'update', dopType: type, dopEmail: dopEmail, sessionid: SessionID}, {"json": true}, function(err, res){
                    if(err) throw err
                    if(res.body == 'OK'){
                        //alert('Файл успешно добавлен')
                        Vue.swal({
                            icon: 'success',
                            text: 'Файл успешно добавлен'
                        });
                    }
                    else if(res.body == 'User undefined'){
                        //alert(res.body)
                        Vue.swal('Пользователь с данной почтой не зарегистрирован');
                    }
                })
            }
        },
        showList(){
            event.preventDefault()
            this.ShowList = true
            this.showAdminInfo = true
            this.ShowAdd = false
            this.ShowAdminList = false
            this.ShowSchoolList = false
            this.ShowSchoolAdd = false
            this.ShowTop = false
            this.ShowModerationList = false
        },
        showSchoolList(){
            event.preventDefault()
            this.ShowSchoolList = true
            this.showAdminInfo = false
            this.ShowAdd = false
            this.ShowList = false
            this.ShowSchoolAdd = false
            this.ShowTop = false
        },
        showTop(){
            event.preventDefault()
            this.ShowTop = true
            this.ShowList = false
            this.showAdminInfo = false
            this.ShowAdd = false
            this.ShowSchoolList = false
            this.ShowSchoolAdd = false
            this.ShowModerationList = false
        },
        showAdd(){
            event.preventDefault()
            this.ShowAdd = true
            this.ShowList = false
            this.showAdminInfo = false
            this.ShowTop = false
            this.ShowSchoolList = false
            this.ShowSchoolAdd = false
            this.ShowModerationList = false
        },
        showSchoolAdd(){
            event.preventDefault()
            this.ShowSchoolAdd = true
            this.ShowList = false
            this.ShowTop = false
            this.showAdminInfo = false
            this.ShowSchoolList = false
            this.ShowAdd = false
        },
        showModerationList(){
            event.preventDefault()
            this.ShowModerationList = true
            this.ShowAdd = false
            this.ShowList = false
            this.showAdminInfo = false
            this.ShowTop = false
            this.ShowSchoolList = false
            this.ShowSchoolAdd = false
        },
        changeInfo(email, chart){
            if(chart != undefined){
                let form = document.getElementById('form' + email)
                if(chart == 'donaught'){
                    form['bar' + email].checked = false
                    form['full' + email].checked = false
                    document.getElementById('chartDiv' + email).style.display = 'block'
                    document.getElementById('chartDiv2' + email).style.display = 'none'
                    document.getElementById('chartDiv3' + email).style.display = 'none'
                }
                else if(chart == 'bar'){
                    form['donaught' + email].checked = false
                    form['full' + email].checked = false
                    document.getElementById('chartDiv' + email).style.display = 'none'
                    document.getElementById('chartDiv2' + email).style.display = 'block'
                    document.getElementById('chartDiv3' + email).style.display = 'none'
                }
                else{
                    form['donaught' + email].checked = false
                    form['bar' + email].checked = false
                    document.getElementById('chartDiv' + email).style.display = 'none'
                    document.getElementById('chartDiv2' + email).style.display = 'none'
                    document.getElementById('chartDiv3' + email).style.display = 'block'
                }
            }
            else{
                if(this.ShowAdd == false){
                    let form = document.getElementById('formForList')
                    if(email == 'school'){
                        form['admin'].checked = false
                        this.ShowList = true
                        this.ShowAdminList = false
                    }
                    else if(email == 'admin'){
                        form['school'].checked = false
                        this.ShowList = false
                        this.ShowAdminList = true
                    }
                }
                else{
                    let form = document.getElementById('formForAdmin')
                    if(email == 'adminList'){
                        form['addAdmin'].checked = false
                        this.AdminList = true
                        this.AddAdmin = false
                    }
                    else if(email == 'addAdmin'){
                        form['adminList'].checked = false
                        this.AdminList = false
                        this.AddAdmin = true
                    }
                }
            }
        },
        changeAddInfo(value){
            let form = document.getElementById('formList')
            if(value == 'list'){
                form['one'].checked = false
                this.ShowAddList = true
                this.ShowAddOne = false
            }
            else{
                form['list'].checked = false
                this.ShowAddList = false
                this.ShowAddOne = true
            }
        },
        deletePerson(email, name, surname, type){
            event.preventDefault()
            let data = {
                email: email,
                name: name,
                surname: surname,
            }
            this.$swal({
                icon: 'warning',
                title: 'Вы уверены что хотите удалить?',
                showCancelButton: true,
                confirmButtonColor: '#3085d6',
                cancelButtonColor: '#d33',
                confirmButtonText: 'Да',
                cancelButtonText: 'Отмена'
            }).then((result) => {
                 if (result.value) {
                     needle.post(this.$store.state.serverIp+'/api/uploadOne', {data: data, email: email, type: 'delete', dopType: type, sessionid: this.SessionID}, {"json": true}, function(err, res){
                        if(err) throw err
                        if(res.body == 'OK'){
                            //alert('Файл успешно добавлен')
                            Vue.swal({
                                icon: 'success',
                                text: 'Ученик успешно удален'
                            });
                            window.location.reload
                        }
                        else{
                            //alert(res.body)
                            Vue.swal(res.body);
                        }
                    })
                }
            })
        },
        moderate_event(event, status, studEmail){
            needle.post('http://78.155.219.12:3000/api/moderateEvent', {email: this.email, type: 'teacher', sessionid: this.SessionID, action: status, data: event, studEmail: studEmail}, {"json": true}, function(err, res){
                if(err) throw err
                if(res.body == 'OK'){
                    //alert('Файл успешно добавлен')
                    Vue.swal({
                        icon: 'success',
                        text: 'Файл успешно добавлен'
                    });
                }
                else if(res.body == 'User undefined'){
                    //alert(res.body)
                    Vue.swal('Пользователь с данной почтой не зарегистрирован');
                }
            })
        },
    },
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
    padding: 0px;
    min-height: 100vh;
    margin-bottom: 0px;
}
.nav-pills{
    cursor: pointer;
}
.main-enter-active{
  transition: opacity .5s;
}
.main-leave-active{
    transition: opacity;
}
.main-enter, .main-leave-to /* .fade-leave-active до версии 2.1.8 */ {
  opacity: 0;
}
.show-enter-active, .auth-leave-active {
  transition: opacity .5s;
}
.show-enter, .auth-leave-to /* .fade-leave-active до версии 2.1.8 */ {
  opacity: 0;
}
.person{
    text-decoration: none;
    color: black;
}
.person_box:hover{
    text-decoration: none;
    color: black;
    background-color: rgba(236, 236, 236, 0.466);
}
.person_box{
    border: 1px solid rgba(0,0,0,.125);
    margin-bottom: 0.4em;
    border-radius: 0.25em;
    padding: 1em;
    text-align: left;
    
}
.person_box a{
    text-decoration: none;
}
.name_group{
    font-size: 1.2em;
}
.ar-collapse:after{
    content: "\f078";
    font-family: fontawesome !important;
}
.ar-show:after{
    content: "\f077";
    font-family: fontawesome !important;
}
.chart-container {
  position: relative;
  height: 500px;
  width: 1000px;
}
.school{
    font-size: 1.2em;
}
.school:hover{
    background-color: rgba(236, 236, 236, 0.466);
    cursor: pointer;
}
.school-body{
    padding: 0.6em;
}
.formInput{
    border-radius: 50px;
}
@media (max-width: 576px) { 
    .moderationImg{
        width: 250px !important;
    }   
}
</style>