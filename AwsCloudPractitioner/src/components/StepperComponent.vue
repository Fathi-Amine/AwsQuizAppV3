<script>
    export default {
        data: ()=>({
            step: 1
        }),
        computed: {
            stepperProgress(){
                return (100 / 3) * ( this.step - 1) + '%'
            }
        }
    }
</script>
<template>
    <div class="stepper-container">
        <div class="stepper">
            <div class="stepper-progress">
                <div class="stepper-progress-bar" :style="'width:' + stepperProgress">

                </div>
            </div>
            <div class="stepper-item" :class="{'current': step == item, 'success': step > item}" v-for="item in 4" :key="item">
                <div class="stepper-item-counter">
                    <img class="icon-success" src="@/assets/images/checked.png" alt="">
                    <span class="number">
                        {{ item }}
                    </span>
                </div>
                <span class="stepper-item-title">
                    step {{ item }}
                </span>
            </div>
        </div>
        <div class="stepper-content" v-for="item in 4" :key="item">
            <div class="stepper-sheet" v-if="step == item">
                <p>Lorem ipsum dolor sit amet consectetur. {{ item }}</p>
                <button v-if="step == 4">start</button>
            </div>
        </div>
        <div class="controls">
            <button class="btn" @click="step--" :disabled="step == 1">
                back
            </button>
            <button class="btn btn-purple-1" @click="step++" :disabled="step == 4">
                next
            </button>
        </div>
    </div>
</template>
<style>
.stepper-container{
    background-color: #fff;
    padding: 60px;
    border-radius: 32px;
    box-shadow: rgba(#000, #000, #000, 0.9);
}
.stepper{
    display: flex;
    align-items: center;
    justify-content: space-between;
    width: 660px;
    position: relative;
    z-index: 0;
    margin-bottom: 24px;
}
.stepper .stepper-progress{
    position: absolute;
    background-color: #D3D3D3;
    height: 2px;
    z-index: -1;
    left: 0;
    right: 0;
    margin-top: 0 auto;
}
.stepper-progress .stepper-progress-bar{
    position: absolute;
    height: 100%;
    width: 0%;
    background-color: #ab3cfc;
    transition: all 500ms linear;
}
.stepper-item{
    display: flex;
    flex-direction: column;
    align-items: center;
    color: #D3D3D3;
    transition: all 500ms linear;
}

.stepper-item-counter{
    height: 68px;
    width: 68px;
    display: grid;
    place-items: center;
    background-color: #fff;
    border-radius: 100%;
    border: 2px solid #D3D3D3;
    position: relative;
}
.stepper-item-counter .icon-success{
    position: absolute;
    opacity: 0;
    transform: scale(0);
    width: 24px;
    transition: all 500ms linear;
}

.stepper-item-counter .number{
    font-size: 22px;
    transition: all 500ms linear;
}

.stepper-item .stepper-item-title{
    font-size: 14px;
    position: absolute;
    bottom: -24px;
    color: #D3D3D3;
}

.stepper-item.success .stepper-item-counter{
    border-color: #ab3cfc;
    background-color: #533b7c;
    color: #fff;
    font-weight: 600;
}

.stepper-item.success .stepper-item-counter .icon-success{
    opacity: 1;
    transform: scale(1);
}
.stepper-item.success .stepper-item-counter .number{
    opacity: 0;
    transform: scale(0);
}

.stepper-item.success .stepper-item-title{
    color: #533b7c;
}

.stepper-item.current .stepper-item-counter{
    border-color: #533b7c;
    background-color: #533b7c;
    color: #fff;
    font-weight: 600;
}

.stepper-item.current .stepper-item-title{
    color: #D3D3D3 ;
}

.stepper-sheet{
    color: #3e394c;
    text-align: center;
    padding: 120px 60px;
    margin-top: 50px;
    box-shadow: 0px -1px 35px 1px rgba(0,0,0,0.43);
-webkit-box-shadow: 0px -1px 35px 1px rgba(0,0,0,0.43);
-moz-box-shadow: 0px -1px 35px 1px rgba(0,0,0,0.43);;
}

.controls{
    display: flex;
}
 .btn{
    display: flex;
    justify-content: center;
    align-items: center;
    padding: 6px 16px;
    border: 1px solid;
    text-align: center;
    vertical-align: middle;
    cursor: pointer;
    line-height: 1.6;
    transition: all 150ms;
    border-radius: 4px;
    width: fit-content;
    font-size: 0.75rem;
    color: #3e394c;
    background-color: red;
    border-color: #D3D3D3;
    margin-top: 20px;
 }

 .btn:disabled{
    opacity: 0.5;
    pointer-events: none;
 }

 .btn.btn-purple-1{
    background-color: #ab3cfc;
    border-color: #ab3cfc;
    color: #fff;
    margin-left: auto;
 }
</style>