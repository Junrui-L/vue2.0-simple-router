<template>
    <main-layout>
        <div class="box">
            <ul class="puzzle-wrap">
                <li :class="{'puzzle':true, 'puzzle-empty': !puzzle}"
                    v-for="puzzle in puzzles" v-text="puzzle"
                    @click="moveFn($index)"
                >
                    
                </li>
            </ul>
            <button class="btn" @click = "render">重置游戏</button>
            <a href="/">回首页吧</a>
        </div>
    </main-layout>
</template>
<script>
    import MainLayout from '../layouts/Main.vue'
    export default {
        data() {
            return {
                puzzles: []
            }
        },
        methods: {
            //重置渲染
            render () {
                let puzzleArr = [], i = 1;

                //生成包含1-15的数组
                for(i; i < 16; i++) {
                    puzzleArr.push(i);
                }

                //随机打乱数组
                puzzleArr = puzzleArr.sort(() => {
                    return Math.random() - 0.5;
                });

                //页面显示
                this.puzzles = puzzleArr;
                this.puzzles.push('');
            },
            //点击方块
           moveFn (index) {
                //获取点击位置及上下左右的值
                console.log(index);
                alert(this.puzzles[index])
                let curNum = this.puzzles[index],
                    leftNum = this.puzzles[index - 1],
                    rightNum = this.puzzles[index + 1],
                    topNum = this.puzzles[ index - 4],
                    bottomNum = this.puzzles[index + 4];
                //和为空的位置交换数值
                if (leftNum === '' && index % 4) {
                    this.puzzles.$set(index - 1, curNum);
                    this.puzzles.$set(index, '');
                } else if(rightNum === '' && 3 !== index % 4) {
                    this.puzzles.$set(index + 1, curNum);
                    this.puzzles.$set(index, '');
                } else if(topNum == '') {
                    this.puzzles.$set(index - 4, curNum);
                    this.puzzles.$set(index, '');
                } else if(bottomNum == '') {
                    this.puzzles.$set(index + 4, curNum);
                    this.puzzles.$set(index, '');
                }

                this.passFn()
            },
            //校验是否通通关

            passFn () {
                if(this.puzzles[15] == '') {
                    const newPuzzles = this.puzzles.slice(0, 15);
                    const isPass = newPuzzles.every((e, i) => e === i + 1);
                    if(isPass) {
                        alert('恭喜，通关成功')
                    }
                }
            }
        },

        ready () {
            this.render()
        },
        components: {
            MainLayout
        }
    }
</script>
<style>
    body {
        font-family: Arial, "Microsoft YaHei";
    }

    .box {
        width: 400px;
        margin: 50px auto 0;
    }
    
    .puzzle-wrap {
        width: 400px;
        height: 400px;
        margin-bottom: 40px;
        padding: 0;
        background: #ccc;
        list-style: none;
    }

    .puzzle {
        float: left;
        width: 100px;
        font-size: 20px;
        background: #f99;
        text-align: center;
        line-height: 100px;

    }   

    .puzzle-empty {
        background: #ccc;
        box-shadow: inset 2px 2px 2px;
    }
    
    .btn-reset {
        box-shadow: inset 2px 2px 18px;
    }

    .btn {
        width: 400px;
        height: 60px;
        text-align: center;
        line-height: 60px;
        background: #fc6;
        box-shadow: inset 3px -2px -5px;
    }
</style>