<template>
    <ul class="list">
        <li 
        class="list-item" 
        v-for="item of letters" 
        :key="item"
        :ref="item"
        @click="handleLetterClick"
        @touchstart="handleTouchStart"
        @touchmove="handleTouchMove"
        @touchend="handleTouchEnd">
        {{item}}
        </li>
    </ul>  
</template>

<script>

export default {
    name:'CityAlphabet',
    props: {
        cities: Object
    },
    computed: {
        letters () {
            const letters = []
            for (let i in this.cities) {
                letters.push(i)
            }
            return letters
        }
    },
    data () {
        return {
            touchStaus:false,
            startY: 0,
            timer: null
        }
    },
    updated () {
        this.statrY = this.$refs['A'][0].offsetTop //获取字母A到头部的底边的距离
    },
    methods: {
        handleLetterClick (e) {
            this.$emit('change', e.target.innerText)
        },
        handleTouchStart () {
            this.touchStaus = true
        },
        handleTouchMove (e) {
            if (this.touchStaus) {
                if (this.timer) {
                    clearTimeout(this.timer)
                }
                this.timer = setTimeout(() => {
                    const touchY = e.touches[0].clientY - 79  //获取手指触摸位置到头部底边的距离
                    const index = Math.floor((touchY - this.startY) / 20)
                    if (index >= 0 && index < this.letters.length )
                    this.$emit('change', this.letters[index])    
                }, 16);     
            }
        },
        handleTouchEnd () {
            this.touchStaus = false
        }

    }
}
</script>

<style lang="stylus" scoped>
 @import '~styles/varibles.styl'
.list
    position absolute
    display flex
    flex-direction  column
    justify-content center
    right 0
    top 1.58rem
    bottom 0
    width .4rem
    .list-item
        text-align center
        color $bgColor
        line-height .4rem
    

           

</style>

