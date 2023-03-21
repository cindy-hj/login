<template>
    <div>
        <span><img :src="item.imgPath" width="100"></span>
        <!-- <span class="img" :style="{backgroundImage: `url(${item.imgPath})`}"></span> -->
        <div class="wow">
            이름: <span>{{ item.name }}&nbsp;</span>
            할인율: <span>{{ item.discountPer }}%</span>
            가격: <span>{{ lib.getNumberFormatted(item.price) }}</span>
        </div>

    </div>
</template>

<script>
import lib from "@/lib/lib"

export default {
    name:"CardInfo",
    props:{
        item: Object
    },
    setup () {
        
        return { 
            lib,
        };
    }
}
</script>

<style lang="css" scoped>
.img{
    /* display: inline-block; */
    width: 100%;
    height: 250px;
    background-size: cover;
    background-position: center;
}

.wow{
    display: inline-block
}
</style>