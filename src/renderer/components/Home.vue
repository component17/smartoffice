<template>
    <div>
        <el-row v-if="isLineOne">
            <el-button :type="lineOne ? 'default' : 'success'" style="width: 100%" @click="power(1)">Линия 1</el-button>
        </el-row>

        <el-row style="margin-top: 20px" v-if="isLineTwo">
            <el-button :type="lineTwo ? 'default' : 'success'" style="width: 100%" @click="power(2)">Линия 2</el-button>
        </el-row>

        <el-row style="margin-top: 20px" v-if="isLineOne">
            <el-button type="primary" style="width: 100%" @click="flash(1)">Линия 1 (Помигать)</el-button>
        </el-row>

        <el-row style="margin-top: 20px" v-if="isLineTwo">
            <el-button type="primary" style="width: 100%" @click="flash(2)">Линия 2 (Помигать)</el-button>
        </el-row>

        <el-row style="margin-top: 20px" v-if="isLineOne && isLineTwo">
            <el-button type="danger" style="width: 100%" @click="disco">Дискотека</el-button>
        </el-row>
    </div>
</template>

<script>
    const gpio = require("gpio");


    export default {
        data(){
            return {
                lineOne: false,
                lineTwo: false,

                isLineOne: false,
                isLineTwo: false,

                gpio20: null,
                gpio21: null,
            }
        },
        created(){
            this.init();
        },
        methods: {
            init(){
                this.gpio20 = gpio.export(20, {
                    direction: gpio.DIRECTION.OUT,
                    ready: () => {
                        this.isLineOne = true;

                        this.power(1)
                    }
                });

                this.gpio21 = gpio.export(21, {
                    direction: gpio.DIRECTION.OUT,
                    ready: () => {
                        this.isLineTwo = true;

                        this.power(2)
                    }
                });
            },

            power(line){
                if(line === 1){
                    this.actionLineOne()
                }

                if(line === 2){
                    this.actionLineTwo()
                }
            },

            async flash(line){
                if(line === 1){
                    await this.actionLineOne();
                    await this.timeout(300);

                    await this.actionLineOne();
                    await this.timeout(300);

                    await this.actionLineOne();
                    await this.timeout(500);

                    await this.actionLineOne();
                    await this.timeout(300);

                    await this.actionLineOne();
                    await this.timeout(300);

                    await this.actionLineOne();
                }

                if(line === 2){
                    await this.actionLineTwo();
                    await this.timeout(300);

                    await this.actionLineTwo();
                    await this.timeout(300);

                    await this.actionLineTwo();
                    await this.timeout(500);

                    await this.actionLineTwo();
                    await this.timeout(300);

                    await this.actionLineTwo();
                    await this.timeout(300);

                    await this.actionLineTwo();
                }
            },

            async disco(){
                await this.actionLineOne();
                await this.actionLineTwo();
                await this.timeout(300);

                await this.actionLineOne();
                await this.actionLineTwo();
                await this.timeout(300);

                await this.actionLineOne();
                await this.timeout(300);

                await this.actionLineTwo();
                await this.timeout(300);

                await this.actionLineOne();
                await this.timeout(300);

                await this.actionLineTwo();
                await this.timeout(300);

                await this.actionLineOne();
                await this.timeout(300);

                await this.actionLineTwo();
                await this.timeout(300);

                await this.actionLineOne();
                await this.timeout(300);

                await this.actionLineTwo();
                await this.timeout(300);

                await this.actionLineTwo();
                await this.timeout(300);

                await this.actionLineOne();
                await this.timeout(300);

                await this.actionLineOne();
                await this.actionLineTwo();
            },

            timeout(ms){
                return new Promise((resolve, reject) => {
                    setTimeout(() => {
                        resolve();
                    }, ms)
                })
            },

            actionLineOne(){
                return new Promise((resolve, reject) => {
                    this.gpio20.set(this.lineOne ? 0 : 1, () => {
                        this.lineOne = this.gpio20.value;
                        resolve()
                    });
                })
            },

            actionLineTwo(){
                return new Promise((resolve, reject) => {
                    this.gpio21.set(this.lineTwo ? 0 : 1, () => {
                        this.lineTwo = this.gpio21.value;
                        resolve()
                    });
                })
            }
        }
    }
</script>

<style scoped>

</style>