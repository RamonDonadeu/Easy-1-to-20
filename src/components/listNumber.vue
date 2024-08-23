<template>
    <div class="listNumber">
        <p>{{number}}.-</p>
        <div class="numberValue" :canBeClicked="canBeClicked" @click="() => {
            if(!canBeClicked) return
           $emit('setValue')
        }">
                <div v-show="value!==-1" >
                {{ value }}

                </div>
        </div>
    </div>
</template>
<script setup lang="ts">
withDefaults(defineProps < {
    number: number,
    value: number,
    canBeClicked: boolean
} > (),
    {
    number: 0,
    value: 0,
    canBeClicked: false
})

</script>
<style>

.listNumber{
    display: flex;
    flex-direction: row;
    justify-content: space-between;
    align-items: center;
    width: 100%;
    gap: 10px;
}

.numberValue{    
    width: 100%;
    height: 25px;
    background-color: #333333;
    flex: 1;
    text-align: center;
}

.numberValue[canBeClicked="true"]{
    background-color: #555555;
}

.numberValue[canBeClicked="true"]:hover{
    background-color: #444444;
    cursor: pointer;
}
</style>