<script>
export default {
    props: ['pagestatus','userName'],
    emits: ['user-name',],
    data(){
        return{
            easy: [
                {name: 'APPLE', image: 'APPLE.png'},
                {name: 'BANANA', image: 'BANANA.png'},
                {name: 'CHERRY', image: 'CHERRY.png'},
                {name: 'GRAPES', image: 'GRAPES.png'},
                {name: 'LEMON', image: 'LEMON.png'},
                {name: 'ORANGE', image: 'ORANGE.png'},
                {name: 'PEACH', image: 'PEACH.png'},
                {name: 'WATERMELON', image: 'WATERMELON.png'},
                {name: 'BIRD', image: 'BIRD.png'},
                {name: 'BUTTERFLY', image: 'BUTTERFLY.png'},
                {name: 'CAT', image: 'CAT.png'},
                {name: 'DINASOUR', image: 'DINASOUR.png'},
                {name: 'DOG', image: 'DOG.png'},
                {name: 'DOLPHIN', image: 'DOLPHIN.png'},
                {name: 'DONKEY', image: 'DONKEY.png'},
                {name: 'DUCK', image: 'DUCK.png'},
                {name: 'EAGLE', image: 'EAGLE.png'},
                {name: 'FISH', image: 'FISH.png'},
                {name: 'FOX', image: 'FOX.png'},
                {name: 'GIRAFFE', image: 'GIRAFFE.png'},
                {name: 'HORSE', image: 'HORSE.png'},
                {name: 'LION', image: 'LION.png'},
                {name: 'PANDA', image: 'PANDA.png'},
                {name: 'PARROT', image: 'PARROT.png'},
                {name: 'PENGUIN', image: 'PENGUIN.png'},
                {name: 'TOUCAN', image: 'TOUCAN.png'}
            ],
            uname: "",
            gameStarted: false,
            randomItems: [],
            shuffleItems: [],
            inPlay: false,
            rePlay: null,
            highScore: [],
        }
    },
    created() {
        this.setViewport();
    },
    methods: {
        setHighScore(x){
            this.highScore = x;
            this.$emit('mainhighscore', this.highScore);
        },        
        playStatusAgain(x){
            this.rePlay = x;
            if(this.rePlay == true){
                this.setDifficulty(2, 2, 'easy');
            }
        },
        setDifficulty(row, col, difficulty){
            this.gameStarted = true;
            this.generateRandom(row, col, difficulty);
        },
        generateRandom(row, col, difficulty){
            let tempArray = [...this[difficulty]]
            this.size = (row * col) / 2;    
            for (let i = 0; i < this.size; i++){
                const randomIndex = Math.floor(Math.random() * tempArray.length)
                this.randomItems.push(tempArray[randomIndex])
                tempArray.splice(randomIndex, 1)
            }
            this.shuffleItems = [...this.randomItems, ...this.randomItems]
            this.shuffleItems.sort(() => Math.random() -0.5)
            return this.shuffleItems
        },
        setUsername(x){
            if(x != ""){
                this.$emit('user-name', x);
                document.getElementById("uInput").value = "";
            }else{
                alert("Invalid user input");
            }
        },
        setViewport: function() {
            let changeWindowSize = 375
            let viewportContent = "width=device-width,initial-scale=1.0,minimum-scale=1.0,maximum-scale=1.0,user-scalable=no"
            if (window.innerWidth < changeWindowSize ) {
                viewportContent = "width=375,user-scalable=no,viewport-fit=cover"
            }
            document.querySelector("meta[name='viewport']").setAttribute("content", viewportContent)
        },
        gameExit(){
            this.gameStarted = false;
            this.inPlay = false;
            this.randomItems= [];
            this.shuffleItems= [];
        },
        custom(){
            let foo = prompt('PLAY custom numbers of pairs');
            if((foo != "") || (foo != null)){
                if (isNaN(foo)){
                    alert("Must be a whole number!");
                }else{
                    if(foo < 0){
                        alert("Must be a positive number!");
                    } else if(foo > this.easy.length){
                        alert("Max Pairs is: "+this.easy.length);
                    }else{
                        this.setDifficulty(2, foo, 'easy');
                    }
                }
            }
        }
    }
}
</script>

<template>
    <div id="mWrapper">
        <div id="MainMenu">
            <span id="menuBtn" v-if="gameStarted">
                <img @click="gameExit()" id="closeBtn" src="../assets/web/playisclicked/close.png">
            </span>
            <div id="mContainer" v-if="!gameStarted">
                <div id="titleWrapper">
                    <div class="mTitleContainer">
                        <img class="mTitle" src="../assets/web/firstpage/MEMORY.png">
                    </div>
                    <div class="mTitleContainer">
                        <img class="mTitle2" src="../assets/web/firstpage/GAME.png">
                    </div>
                </div>

                <div v-if="(userName == '')" id="uInputWrapper">
                    <div id="uInputContainer">
                        <h2>Username:</h2><input id="uInput" v-model="uname" type="text" autofocus required autocomplete="off" placeholder="username"/>
                    </div>
                    <div id="uInputButton">
                        <img src="../assets/web/firstpage/JOINBUTTON.png" id="join" @click="setUsername(uname)">
                    </div>
                </div>
                <div v-else class="greeting">
                    <h1>Hi {{ userName }}!</h1><br>
                    <div style="display: flex; justify-content: center;">
                        <div id="btnWrapper">
                            <!-- <img class="btn" src="../assets/web/home/PLAY.png" @click="$emit('pageS',1)"><br> -->
                            <img class="btn" src="../assets/web/home/PLAY.png" @click="setDifficulty(4, 4, 'easy')">
                            <img class="btn custom" src="../assets/web/levelcomplete/STAR.png" @click="custom()"><br>
                            <img class="btn" src="../assets/web/home/SCOREBOARD.png" @click="$emit('pageS',2)"><br>
                            <img class="btn" src="../assets/web/home/EXIT.png" @click="$emit('user-name','')">
                        </div>
                    </div>
                </div>
            </div>

            <Difficulty v-else :items="shuffleItems" @gStatus = "gameExit" :uName="userName" @highscore="setHighScore"/>
        </div>
    </div>
</template>