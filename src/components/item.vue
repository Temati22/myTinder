<template>
    <figure class="card-item">
        <div class="image-wrapper">
            <img :src="img" alt="">
        </div>

        <div class="card-item__descr">
            <div class="card-item__descr-title">
                {{name}}
            </div>
            <div class="card-item__descr-description">
                {{description}}
            </div>
            <div class="card-item__descr-tags-like">
                <ul>
                    <li>
                        Songs
                    </li>
                    <li>
                        Art
                    </li>
                    <li>
                        Sex
                    </li>
                    <li>
                        Buisenes
                    </li>
                </ul>
            </div>
        </div>
    </figure>
</template>

<script>

    export default {
        name: 'Item',
        props: [
            'img',
            'name',
            'description'
        ]
    }
</script>

<style lang="scss" scoped>

    ul {
        list-style: none;
        display: flex;
        flex-direction: row;
        justify-content: flex-start;
        flex-wrap: wrap;

        li {
            padding: 5px 10px;
            border-radius: 6px;
            border: 1px solid #f0f0f0;
            margin-right: 10px;
            background: #fff;
        }
    }


    .card-item {
        height: 450px;
        border-radius: 10px;
        overflow: hidden;
        box-shadow: 7px 7px 10px #bababa;
        transition: all ease-in-out .3s;
        position: absolute;
        top: 0;
        left: 0;
        right: 0;
        background: #fff;
        transform-origin: 50% 100%;
        transition: all ease-in-out .3s;

        &.like {
            transform: rotate3d(1, 1, 3, 40deg);
            animation-duration: .3s;
            animation-name: fadeout;
        }

        &.dislike {
            transform: rotate3d(1, 1, 3, -40deg);
            animation-duration: .3s;
            animation-name: fadeout;
        }

        .image-wrapper {
            overflow: hidden;
            height: 450px;
            position: relative;

            img {
                min-width: 100%;
                transform: translateX(-50%);
                position: absolute;
                left: 50%;
                top: 0;
                bottom: 0;
                min-height: 100%;
            }
        }


        &__descr {
            position: absolute;
            bottom: 0;
            width: 100%;
            padding: 20px;

            div {
                line-height: 1.2;
            }

            &-title {
                font-size: 34px;
                font-weight: bold;
                text-align: left;
                text-shadow: 0 0 5px #000;
                color: #fff;
            }

            &-description {
                font-size: 18px;
                text-align: left;
                padding: 10px 0;
                color: #fff;
            }
        }


    }

    @keyframes fadeout {
        from {
            opacity: 1;
        }

        to {
            opacity: 0;
        }

        75% {
            opacity: 0.75;
        }
    }

</style>
