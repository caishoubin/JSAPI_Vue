<template>
    <div class="home_div">
        <div>
            地图： <input v-model="lat" />,<input v-model="lng" />
            <div id="container"></div>
            <div id="myPageTop">
                <table>
                    <tr>
                        <td>
                            <label>请输入关键字：</label>
                        </td>
                    </tr>
                    <tr>
                        <td>
                            <input id="tipinput" v-model="autoOptions"/>
                        </td>
                    </tr>
                </table>
            </div>
        </div>
    </div>
</template>
<script>
import AMapLoader from "@amap/amap-jsapi-loader"
import { shallowRef } from '@vue/reactivity'
export default {
    name:"mapcomtaint",
    setup(){
        const map = shallowRef(null);
        return{
            map,
        }

    },
    data (){
        return {
            lat: '',
            lng: '',
            autoOptions: ''
        }
    },
    create(){

    },
    methods:{

        ininMap(){
            AMapLoader.load({
                key:'8c6ad19ec3ac954244d16cf85536981f',  //设置您的key
                version:"2.0",
                plugins:['AMap.ToolBar','AMap.Driving','AMap.PlaceSearch','AMap.AutoComplete'],
                AMapUI:{
                    version:"1.1",
                    plugins:[],

                },
                Loca:{
                    version:"2.0.0"
                },
            }).then((AMap)=>{
                this.map = new AMap.Map('container', {
                    viewMode:"3D",
                    zoom:5,
                    zooms:[2,22],
                    center:[105.602725,37.076636],
                }).on('click', (e) => {
                    var text = '您在[' + e.lnglat + ',' + e.lnglat + ']';
                    console.log(text);
                    this.lat = e.lnglat.getLat();
                    this.lng = e.lnglat.getLng();
                    let marker = new AMap.Marker({
                        position: e.lnglat,
                    });
                    this.map.clearMap();
                    this.map.add(marker);
                });
                // var auto = new AMap.AutoComplete(this.autoOptions);
                // var placeSearch = new AMap.PlaceSearch({
                //     map: map
                // });  //构造地点查询类
                // auto.on("select", select);//注册监听，当选中某条记录时会触发
                // function select(e) {
                //     placeSearch.setCity(e.poi.adcode);
                //     placeSearch.search(e.poi.name);  //关键字查询查询
                // }

                let positionArr = [
                    [113.357224,34.977186],
                    [114.555528,37.727903],
                    [112.106257,36.962733],
                    [109.830097,31.859027],
                    [116.449181,39.98614],
                ];
                for(let item of positionArr){
                    let marker = new AMap.Marker({
                        position:[item[0],item[1]],
                    });
                    this.map.add(marker);
                }


            }).catch(e=>{
                console.log(e);
            })
        },
        addMarker(){

        }
    },
    mounted(){
        //DOM初始化完成进行地图初始化
        this.ininMap();
    }
}
</script>
<style scope>
    .home_div{
        height: 100%;
        width: 100%;
        padding: 0px;
        margin: 0px;
        position: relative;
    }
    #container{
        height: 100%;
        width: 100%;
        padding: 0px;
        margin: 0px;
    }
    .map-title{
         position:absolute;
         z-index: 1;
         width: 100%;
         height: 50px;
         background-color: rgba(27, 25, 27, 0.884);

    }
    h3{
        position:absolute;
        left: 10px;
        z-index: 2;
        color: white;
    }

    html, body {
        margin: 0;
        height: 100%;
        width: 100%;
        position: absolute;
    }

    #container {
        position: absolute;
        top: 0;
        left: 0;
        right: 0;
        bottom: 0;
        width: 100%;
        height: 100%;
    }

    .button-group {
        position: absolute;
        bottom: 20px;
        right: 20px;
        font-size: 12px;
        padding: 10px;
    }

    .button-group .button {
        height: 28px;
        line-height: 28px;
        background-color: #0D9BF2;
        color: #FFF;
        border: 0;
        outline: none;
        padding-left: 5px;
        padding-right: 5px;
        border-radius: 3px;
        margin-bottom: 4px;
        cursor: pointer;
    }
    .button-group .inputtext {
        height: 26px;
        line-height: 26px;
        border: 1px;
        outline: none;
        padding-left: 5px;
        padding-right: 5px;
        border-radius: 3px;
        margin-bottom: 4px;
        cursor: pointer;
    }
    /*
   .tip {
       position: absolute;
       bottom: 30px;
       right: 10px;
       background-color: #FFF;
       text-align: center;
       border: 1px solid #ccc;
       line-height: 30px;
       border-radius: 3px;
       padding: 0 5px;
       font-size: 12px;
   }
   */
    #tip {
        background-color: #fff;
        padding-left: 10px;
        padding-right: 10px;
        position: absolute;
        font-size: 12px;
        right: 10px;
        top: 20px;
        border-radius: 3px;
        border: 1px solid #ccc;
        line-height: 30px;
    }

    /*
    #tip input[type='button'] {
        margin-top: 10px;
        margin-bottom: 10px;
        background-color: #0D9BF2;
        height: 30px;
        text-align: center;
        line-height: 30px;
        color: #fff;
        font-size: 12px;
        border-radius: 3px;
        outline: none;
        border: 0;
    }
    */
    .amap-info-content {
        font-size: 12px;
    }

    #myPageTop {
        position: absolute;
        top: 5px;
        right: 10px;
        background: #fff none repeat scroll 0 0;
        border: 1px solid #ccc;
        margin: 10px auto;
        padding:6px;
        font-family: "Microsoft Yahei", "微软雅黑", "Pinghei";
        font-size: 14px;
    }
    #myPageTop label {
        margin: 0 20px 0 0;
        color: #666666;
        font-weight: normal;
    }
    #myPageTop input {
        width: 170px;
    }
    #myPageTop .column2{
        padding-left: 25px;
    }


</style>
