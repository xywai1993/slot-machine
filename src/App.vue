<template>
    <!-- <HelloWorld msg="Hello Vue 3.0 + Vite" /> -->

    <section class="tiger-box">
        <div class="demo" :class="{ demo1: ani.ani1 }">
            <div class="demo-1">
                <p>1111</p>
                <p>2</p>
                <p>3</p>
            </div>
            <div class="demo-2">
                <p>1</p>
                <p>2</p>
                <p>3</p>
            </div>
            <div class="demo-3">
                <p>1</p>
                <p>2</p>
                <p>3</p>
            </div>
            <div class="demo-4">
                <p>1</p>
                <p>{{ ani.randomNum }}</p>
                <p>3</p>
            </div>
        </div>
        <div class="demo" :class="{ demo2: ani.ani1 }">
            <div class="demo-1">
                <p>1</p>
                <p>2</p>
                <p>3</p>
            </div>
            <div class="demo-2">
                <p>1</p>
                <p>2</p>
                <p>3</p>
            </div>
            <div class="demo-3">
                <p>1</p>
                <p>2</p>
                <p>3</p>
            </div>
            <div class="demo-4">
                <p>1</p>
                <p>{{ ani.randomNum2 }}</p>
                <p>3</p>
            </div>
        </div>
        <div class="demo" :class="{ demo3: ani.ani1 }">
            <div class="demo-1">
                <p>1</p>
                <p>2</p>
                <p>3</p>
            </div>
            <div class="demo-2">
                <p>1</p>
                <p>2</p>
                <p>3</p>
            </div>
            <div class="demo-3">
                <p>1</p>
                <p>2</p>
                <p>3</p>
            </div>
            <div class="demo-4">
                <p>1</p>
                <p>{{ ani.randomNum3 }}</p>
                <p>3</p>
            </div>
        </div>
    </section>

    <button @click="startAni">旋转</button>

    <div class="demo-test">
        <div class="demo-test1">1</div>
        <div class="demo-test2">2</div>
        <div class="demo-test3">3</div>
        <div class="demo-test4">4</div>
    </div>
</template>

<script>
import { reactive } from 'vue';
import HelloWorld from './components/HelloWorld.vue';

export default {
    name: 'App',
    components: {
        HelloWorld,
    },
    setup() {
        const ani = reactive({
            ani1: false,
            ani2: false,
            ani3: false,
            randomNum: 0,
            randomNum2: 0,
            randomNum3: 0,
        });

        const startAni = () => {
            // ani.randomNum = 2;
            ani.ani1 = false;

            // console.log(parseInt(Math.random() * 10));

            setTimeout(() => {
                ani.randomNum = parseInt(Math.random() * 10);
                ani.randomNum2 = parseInt(Math.random() * 10);
                ani.randomNum3 = parseInt(Math.random() * 10);

                ani.ani1 = true;
                // ani.ani1 = false;
            }, 60);
        };
        return { ani, startAni };
    },
};
</script>
<style lang="less" scoped>
.demo {
    // margin: 100px auto;
    position: relative;
    perspective: 0;
    width: 100px;
    height: 100px;
    transform-style: preserve-3d;
    // backface-visibility: hidden;
    transform-origin: center center -50px;
    will-change: transform;
    > div {
        position: absolute;
        left: 0;
        top: 0;
        width: 100px;
        height: 100px;
        color: #000;
        text-align: center;
    }
}
p {
    font-size: 15px;
    color: #000;
}

.demo1 {
    animation: totateX 2s forwards;
}
.demo2 {
    animation: totateX 2.5s forwards;
}
.demo3 {
    animation: totateX 3s forwards;
}

//
.demo-1 {
    // background-color: rgba(255, 0, 0, 1);
    background: #fff;
}
.demo-2 {
    // background-color: green;
    background: #fff;
    transform-origin: center top;
    transform: translateZ(-100px) rotateX(90deg);
}
.demo-3 {
    // background-color: yellow;
    background: #fff;
    transform: translateZ(-100px) rotateX(180deg);
}
.demo-4 {
    background: #fff;
    // background-color: rgba(0, 0, 255, 1);
    transform-origin: center bottom;
    transform: translateZ(-100px) rotateX(-90deg);
}

.tiger-box {
    display: flex;
    width: 300px;
    height: 100px;
    border: 1px solid #000;
    overflow: hidden;
}

@keyframes totateX {
    0% {
        transform: rotateX(0deg);
    }
    100% {
        transform: rotateX(810deg);
    }
}

.demo-test {
    margin: 100px auto;
    position: relative;
    perspective: 550px;
    width: 100px;
    height: 100px;
    transform-style: preserve-3d;
    // backface-visibility: hidden;
    transform-origin: center center -50px;
    animation: totateX 10s infinite linear both;
    // transform: rotateX(-180deg);
    > div {
        position: absolute;
        left: 0;
        top: 0;
        width: 100px;
        height: 100px;
        color: #000;
        text-align: center;
        font-size: 40px;
        line-height: 100px;
    }
}
.demo-test1 {
    background-color: rgba(255, 0, 0, 0.336);
    // background: #fff;
}
.demo-test2 {
    background-color: rgba(0, 128, 0, 0.466);
    // background: #fff;
    transform-origin: center top;
    transform: translateZ(-100px) rotateX(90deg);
}
.demo-test3 {
    background-color: rgba(255, 255, 0, 0.37);
    // background: #fff;
    transform-origin: center center;
    transform: translateZ(-100px) rotateX(180deg);
}
.demo-test4 {
    background-color: rgba(0, 0, 255, 0.363);
    transform-origin: center bottom;
    transform: translateZ(-100px) rotateX(-90deg);
}
</style>
