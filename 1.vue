<template>
    <div unselectable="on" onselectstart="return false;" style="-moz-user-select:none;">
        <div id = "bottom" ></div>
        <div id="trainSelected" @mousemove="move()">aaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaa</div>
    </div>
</template>

<script>
export default {
    data(){
        return{
            pressed: false, 
            pressData:null,
        }
    },
    methods:{
       //取出来：矩形里开始位置的Aid 和 结束位置的Bid
        move(data){
            if (!this.pressed) return
            // console.log("data",data);
            this.pressData.push(data);
        },
        boxSelection(){
            this.pressData = []; //圈中的值，初始化
            var stateBar = document.getElementById("bottom");
            var trainSelected = document.getElementById('trainSelected');
            //当鼠标按下时，按钮事件
            trainSelected.onmousedown = (e) =>{
                this.pressed = true;
                // console.log('开始',e , e.clientY, e.clientY)

            var posx = e.clientX;
            var posy = e.clientY;
            var div = document.createElement("div");
            div.className = "tempDiv";
            div.style.left = e.clientX+"px";
            div.style.top = e.clientY+"px";
            // document.body.appendChild(div);
            trainSelected.appendChild(div);
            //当鼠标指针移出时，
            trainSelected.onmousemove = (ev) =>{
                div.style.left = Math.min(ev.clientX, posx) + "px";
                div.style.top = Math.min(ev.clientY, posy) + "px";
                div.style.width = Math.abs(posx - ev.clientX)+"px";
                div.style.height = Math.abs(posy - ev.clientY)+"px";

                // aid的left=矩形的left; top=top+height
                // bid的left=矩形的left+width; top=top+height

                //Aid 的位置坐标
                // let AId_left = e.clientX;
                // let AId_hight = (e.clientY + Math.abs(posy - ev.clientY));

                //Bid 的位置坐标
                // let BId_left = e.clientX + Math.abs(posx - ev.clientX);
                // let BId_hight = (e.clientY + Math.abs(posy - ev.clientY));
                stateBar.innerHTML = "MouseX: " + ev.clientX + "<br/>MouseY: " + ev.clientY;
                //当松开鼠标按钮时，
                trainSelected.onmouseup = (event) =>{
                    console.log("end",this.pressData, this.pressData[0], this.pressData[this.pressData.length-1]);
                    // AID =  this.pressData[0],
                    // BID = this.pressData[this.pressData.length-1];
                    this.pressed = false;
                    // console.log('结束', event.clientX, event.clientY)
                    div.parentNode.removeChild(div);
                    trainSelected.onmousemove = null;
                    trainSelected.onmouseup = null;
                }
            }
            }
        },
    },
    created(){

    },
    mounted(){
        this.boxSelection();
    }
}
</script>

<style>
*{
    padding:0;
    margin:0;
}
/* /鼠标移动时禁止选中文字/ */
div{
    -moz-user-select: none;
    -khtml-user-select: none;
    user-select: none;
    cursor: pointer;
}
#bottom{
    position:absolute;
    bottom:0px;
    width:100%;
    height:40px;
    border:1px solid #000;
    background:#000;
    color:#fff;
}
.tempDiv{
    border:1px dashed blue;
    background:#5a72f8;
    position:absolute;
    width:0;
    height:0;
    filter:alpha(opacity:10);
    opacity:0.1;
}
#trainSelected {
    height: 200px;
    width: 100%;
}
</style>
