<template>
    <!-- <HelloWorld msg="Hello Vue 3.0 + Vite" /> -->

    <section class="tiger-box">
        <div class="demo" :class="{ demo1: ani.ani1 }">
            <div class="demo-1">
                <p>1111</p>
                <p>{{ ani.randomNum }}</p>
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
                <p>2</p>
                <p>3</p>
            </div>
        </div>
        <div class="demo" :class="{ demo2: ani.ani1 }">
            <div class="demo-1">
                <p>1</p>
                <p>{{ ani.randomNum2 }}</p>
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
                <p>2</p>
                <p>3</p>
            </div>
        </div>
        <div class="demo" :class="{ demo3: ani.ani1 }">
            <div class="demo-1">
                <p>1</p>
                <p>{{ ani.randomNum3 }}</p>
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
                <p></p>
                <p>3</p>
            </div>
        </div>
    </section>

    <div>
        <button style="margin: 0" @click="startAni">旋转</button>
        <button style="margin: 0" @click="startCoin">撒金币动画</button>
    </div>

    <!-- <div class="demo-test">
        <div class="demo-test1">1</div>
        <div class="demo-test2">2</div>
        <div class="demo-test3">3</div>
        <div class="demo-test4">4</div>
    </div> -->

    <!-- <div class="coin-box">
        <div class="coin">1</div>
    </div> -->

    <canvas v-show="showCanvas" width="100%" height="100%" ref="canvas" class="canvas"></canvas>
</template>

<script>
import { onMounted, reactive, ref } from 'vue';
import HelloWorld from './components/HelloWorld.vue';
// import { gsap } from 'gsap';
import logoImg from './assets/logo.png';
console.log(logoImg);
export default {
    name: 'App',
    components: {
        HelloWorld,
    },
    setup() {
        const ani = reactive({
            ani1: false, //旋转动画
            randomNum: 0,
            randomNum2: 0,
            randomNum3: 0,
        });
        const { canvas, animation, showCanvas } = useCanvas();
        const startAni = async () => {
            // ani.randomNum = 2;
            ani.ani1 = false;
            await sleep(60);
            ani.ani1 = true;
            await sleep(1000);
            ani.randomNum = parseInt(Math.random() * 10);
            ani.randomNum2 = parseInt(Math.random() * 10);
            ani.randomNum3 = parseInt(Math.random() * 10);
            await sleep(1900);
            startCoin();

            // console.log(parseInt(Math.random() * 10));
        };

        const startCoin = async () => {
            showCanvas.value = true;
            const nowTime = new Date().getTime();
            const position = Array.from({ length: 30 }).map((item, i) => {
                const endX = createRandom(500, -100);
                const endY = createRandom(550, 450);

                let cX = 0;
                let cY = 0;
                if (endX > 200) {
                    cX = createRandom(endX, 200);
                }

                if (endX < 200) {
                    cX = createRandom(200, endX);
                }

                cY = createRandom(endY) - 50;

                return { x: endX, y: endY, w: 10, h: 10 };
            });

            animation.value(position, 1500);
            await sleep(1600);
            showCanvas.value = false;
        };

        return { ani, startAni, canvas, startCoin, showCanvas };
    },
};

function sleep(time) {
    return new Promise((resolve, reject) => {
        setTimeout(resolve, time);
    });
}

function useCanvas() {
    const canvas = ref(null);
    const showCanvas = ref(false);
    let aniId = null;
    const animation = ref(null);
    onMounted(async () => {
        const dom = canvas.value;
        dom.width = document.documentElement.clientWidth;
        dom.height = document.documentElement.clientHeight;
        const ctx = dom.getContext('2d');

        const coinImg = await loadImg('coin.png');

        // gsap.to(test, {
        //     a: 100,
        //     b: 100,
        //     duration: 2,
        //     onUpdate() {
        //         console.log(test.a);
        //     },
        // });

        // animation.value =

        const coinAnimation = (position, time) => {
            ctx.clearRect(0, 0, dom.width, dom.height);
            ctx.fillStyle = 'red';

            const t1 = gsap.timeline({
                onUpdate() {
                    ctx.clearRect(0, 0, dom.width, dom.height);
                    position.forEach((item) => {
                        ctx.drawImage(coinImg, item.x, item.y, item.w, item.h);
                    });
                },
            });

            position.forEach((item) => {
                t1.to(item, { x: 10, y: 10, w: '+=10', h: '+=10', duration: time / 1000, ease: 'slow.out' }, 0);
            });

            // position.forEach((item) => {
            //     const p = cubic3([200, 200], item.p, ((t - nowTime) / time).toFixed(3));
            //     const p2 = cubic2([200, 200], item.p, ((t - nowTime) / time).toFixed(3), item.c);

            //     // await img.onload();
            //     // ctx.fillRect(p2.x, p2.y, w, h);
            //     ctx.drawImage(coinImg, p2.x, p2.y, w, h);
            // });

            // w += 0.35;
            // h += 0.35;
            // t += 0.0001;

            // console.log(animation.value.bind(this, [nowTime, time]));
            // aniId = requestAnimationFrame(coinAnimation.bind(this, position, nowTime, time, w, h));
        };
        // animation(1000);
        animation.value = coinAnimation;
    });

    return { canvas, animation, showCanvas };
}

/**
 * 3阶贝塞尔曲线
 * @param {number[]} p1 - 起始位置 [x,y]
 * @param {number[]} p2 - 结束位置 [x,y]
 * @param {number} t - 0到1
 * @param {number[]} [c1] - 贝塞尔曲线控制点1 [x,y]
 * @param {number[]} [c2] - 贝塞尔曲线控制点2 [x,y]
 */
function cubic3(p1, p2, t, c1 = [0, 0.12], c2 = [0, -0.44, -0.91]) {
    // cubic-bezier(.67,-1.21,1,-0.33)
    // 贝塞尔曲线数学公式3阶  M = a(1-t)^3 + 3bt(1-t)^2 + 3ct^2(1-t) + dt^3
    // 贝塞尔曲线数学公式2阶  M = a(1-t)^2 + 2bt(1-t) +  dt^2
    const a = p1;
    const b = c1;
    const c = c2;
    const d = p2;

    const l = (i) => {
        const p1 = a[i] * Math.pow(1 - t, 3);
        const p2 = 3 * b[i] * t * Math.pow(1 - t, 2);
        const p3 = 3 * c[i] * Math.pow(t, 2) * (1 - t);
        const p4 = d[i] * Math.pow(t, 3);
        return p1 + p2 + p3 + p4;
    };
    return {
        x: l(0),
        y: l(1),
    };
}

/**
 * 2阶贝塞尔曲线
 * @param {number[]} p1 - 起始位置 [x,y]
 * @param {number[]} p2 - 结束位置 [x,y]
 * @param {number} t - 0到1
 * @param {number[]} [c1] - 贝塞尔曲线控制点1 [x,y]
 */
function cubic2(p1, p2, t, c1 = [1, -0.99]) {
    // cubic-bezier(.67,-1.21,1,-0.33)
    //
    // 贝塞尔曲线数学公式2阶  M = a(1-t)^2 + 2bt(1-t) +  ct^2
    const a = p1; // 起始点
    const b = c1;
    const c = p2; // 结束点

    const l = (i) => {
        const r1 = a[i] * Math.pow(1 - t, 2);
        const r2 = 2 * b[i] * t * (1 - t);
        const r3 = c[i] * Math.pow(t, 2);
        return r1 + r2 + r3;
    };
    return {
        x: l(0),
        y: l(1),
    };
}

/**
 * 生成指定数字之间的随机数
 */
function createRandom(end, start = 0) {
    return Math.ceil(start + Math.random() * (end - start));
}

/**
 * 加载图片
 */
function loadImg(url) {
    return new Promise((resolve, reject) => {
        const img = new Image();

        img.onload = function () {
            console.log(img);
            resolve(img);
        };
        img.onerror = function (e) {
            reject(e);
        };
        img.src = logoImg;
    });
}
</script>
<style lang="less">
body {
    background: url(/src/assets/menu-icon.png) left top no-repeat;
}
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
    background: url('/src/assets/logo.png') center no-repeat;
    background-size: 12px auto;
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
        transform: rotateX(720deg);
    }
}

@keyframes coinZ {
    0% {
        // perspective: 1000px;
        perspective: 0px;
        transform: translateY(0) translateZ(-100px);
    }
    100% {
        // perspective: -50px;
        perspective: 100px;
        transform: translateY(-200px) translateZ(100px);
    }
}

.demo-test {
    margin: 100px auto;
    position: relative;
    perspective: 100px;
    width: 100px;
    height: 100px;
    transform-style: preserve-3d;
    perspective-origin: center center;
    // backface-visibility: hidden;
    // transform-origin: center center -50px;
    // animation: coinZ 4s infinite linear;
    // transform: rotateX(-180deg);
    // transform: translateZ(-10000px);
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

@keyframes coinBig {
    0% {
        width: 30px;
        height: 30px;
        // transform: rotate3d(1, 1, 1, 0deg);
    }

    100% {
        width: 130px;
        height: 130px;
        // transform: rotate3d(1, 1, 1, 145deg);
    }
}

@keyframes coinY {
    0% {
        transform: translateY(0);
    }

    100% {
        transform: translateY(580px);
    }
}

.coin-box {
    position: relative;
    // margin: 100px;
    width: 100px;
    height: 100px;
}
.coin {
    position: absolute;
    left: 0;
    top: 0;
    background: #000;
    width: 30px;
    height: 30px;
    perspective: 0;
    transform-style: preserve-3d;
    // transition: transform ;
    // animation: coinY 2s infinite cubic-bezier(0.67, -1.21, 1, -0.33), coinBig 2s infinite;
}
.canvas {
    position: absolute;
    left: 0;
    top: 0;
    // border: 1px solid #000;
}
</style>
