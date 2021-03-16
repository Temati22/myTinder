<template>
    <div id="app">
        <div class="title">
            It`s my TINDER
        </div>

        <div class="wrapper-card">

            <!-- Item компонент с пропсами -->
            <div> <!-- обертка, чтобы небыло перерендера всей страницы-->
                <item v-touch:swipe="touchHandler"
                      v-touch:moving="movingHandler"
                      v-touch:start="startHandler"
                      v-for="item in itemsList"
                      :key="item.index"
                      :img="item.src.medium"
                      :description="item.photographer_url"
                      :name="item.photographer"/>
            </div>
            <!--SKELETON PREVIEW-->
            <div v-if="itemsList.length == 0">
                <figure class="card-item">
                    <div class="image-wrapper">
                        <Skeleton width="350px" height="400px"></Skeleton>
                    </div>
                </figure>
            </div>


            <!-- Группа кнопок для работы с лайками -->
            <div class="block-buttons">
                <button class="btn btn-red dislike" @click="dislike"></button>
                <button class="btn btn-green like" @click="like"></button>
            </div>


        </div>
    </div>
</template>

<script>
    import {Skeleton} from 'vue-loading-skeleton'; // немного красоты, в те моменты, пока нету постов
    import item from "@/components/item"; // компонент поста, занимается только выводом

    // сторонняя апи, которая отдает картинки
    import {createClient} from 'pexels';

    const client = createClient('563492ad6f917000010000012e26aad4b40241feb2d8f6bfbe3522f3');
    const query = 'girl';
    // это все к ней же


    export default {
        name: 'App',
        data: function () {
            return {
                itemsList: [],
                page: 1
            }
        },
        components: {
            item,
            Skeleton
        },
        created() {
            this.getMoreItems();
        },
        methods: {

            like: function () {
                let $this = this;
                let elem = this.$el.querySelectorAll('.wrapper-card .card-item');
                elem[this.itemsList.length - 1].classList.add('like');

                setTimeout(function () {
                    $this.itemsList.pop(); // удаляем элемент из массива, можно сохранить в переменную, так как pop вернет нам удаленный элемент

                    /* Тут можем делать все что угодно с нашим ответом, так как мы знаем какую карточку удалили и что ответил нам пользователь  */
                    /* Мысль была сохранить лайки и дизлайки с id карточками и всей пачкой посылать на сервер, либо при каждом ответе, что поможет нам не потерять ответ пользователя*/

                }, 300);

                // подгружаем новых людей, если они закончились в стэке
                if ($this.itemsList.length - 1 == 0) {
                    $this.getMoreItems();
                }

            },

            dislike: function () {
                let $this = this;
                let elem = this.$el.querySelectorAll('.wrapper-card .card-item');

                // добавляем класс для анимации действия
                elem[this.itemsList.length - 1].classList.add('dislike');

                setTimeout(function () {
                    $this.itemsList.pop(); // удаляем элемент из массива, можно сохранить в переменную, так как pop вернет нам удаленный элемент
                    /* Тут можем делать все что угодно с нашим ответом, так как мы знаем какую карточку удалили и что ответил нам пользователь  */
                    /* Мысль была сохранить лайки и дизлайки с id карточками и всей пачкой посылать на сервер, либо при каждом ответе, что поможет нам не потерять ответ пользователя*/

                }, 300);

                // подгружаем новых людей, если они закончились в стэке
                if ($this.itemsList.length - 1 == 0) {
                    $this.getMoreItems();
                }
            },

            getMoreItems: function () {
                let $this = this;

                client.photos.search({query, per_page: 10, page: $this.page}).then(photos => {
                    photos.photos.forEach((el) => {
                        $this.itemsList.push(el);
                    });
                    $this.page++;
                });
            },
            touchHandler: function (direction) {
                console.log(direction);
                if (direction == 'right') {
                    this.like();
                } else {
                    this.dislike();
                }
            },
            movingHandler: function (event) {
                let elemX = event.touches[0].clientX,
                    elemY = event.touches[0].clientY;

                console.log(elemX);
                console.log(elemY);
            },
            startHandler: function(){
                console.log('start');
            }
        }
    }
</script>

<style lang="scss">
    @import "/src/assets/scss/reset.scss";


    #app {
        font-family: Avenir, Helvetica, Arial, sans-serif;
        -webkit-font-smoothing: antialiased;
        -moz-osx-font-smoothing: grayscale;
        text-align: center;
        color: #2c3e50;

        .title {
            padding: 20px 0;
            text-align: center;
            font-size: 24px;
        }


        .wrapper-card {
            max-width: 350px;
            margin: 0 auto;
            position: relative;
            min-height: 550px;
        }

        .block-buttons {
            width: 100%;
            display: flex;
            flex-direction: row;
            justify-content: center;
            flex-wrap: wrap;
            margin: 20px auto;
            position: absolute;
            bottom: 0;

            .btn {
                background: none;
                padding: 10px 20px;
                border-radius: 10px;
                height: 50px;
                width: 120px;
                margin: 0 5px;
                cursor: pointer;

                &-red {
                    color: #fff;
                    background: #f0f0f0 url("../src/assets/img/dis.png") no-repeat center center;
                    background-size: 30px;
                    border: 1px solid #f0f0f0;

                    &:hover {
                        background: #000 url("../src/assets/img/dis.png") no-repeat center center;
                        background-size: 30px;
                    }
                }

                &-green {
                    color: #fff;
                    background: #f0f0f0 url("../src/assets/img/like2.png") no-repeat center center;
                    background-size: 30px;
                    border: 1px solid #f0f0f0;


                    &:hover {
                        background: #000 url("../src/assets/img/like2.png") no-repeat center center;
                        background-size: 30px;
                    }
                }


            }
        }
    }

</style>
