**使用echarts结合百度地图API做迁徙图**

-----

echarts.js原先可以不使用百度地图API，直接实现迁徙图的效果。后来因为一些原因，地理信息的定位需要借助百度地图来实现。

-----

首先需要[echarts](http://echarts.baidu.com/download.html)。   
然后是echarts的插件 [bmap](https://github.com/apache/incubator-echarts/tree/master/extension/bmap)。   
之后是需要在页面上结合百度地图，所以要申请一个百度地图的api key[地图Api申请地址](http://lbsyun.baidu.com/index.php?title=jspopular)
>这里申请一个JS API在网页端 使用就可以了

准备就绪，就可以开始制作了。

* 百度echarts本身的简单使用教程    
    [echarts最简单教程](http://echarts.baidu.com/tutorial.html#5%20%E5%88%86%E9%92%9F%E4%B8%8A%E6%89%8B%20ECharts)    
    这是echarts给出的快速上手教程，关于配置文件中的每个模块的作用在后面简单介绍。文档中也有更详细的介绍和使用方法。

* 搭一个简易的测试页面    
    1. 文件结构
    ```   
        demo/    
            demo.html    
            map.js    
        js/    
          echarts.js    
          bmap.js
    ```
    

    2. demo.html，画一个最简单的div窗口来容纳迁徙图。

    ```
    <html>

    <head>
        <meta charset="utf-8">
        <style>
            .mainwindow{
                height: 500px;
                width: 500px;
            }

            .mapwindow{
                height: 100%;
                width: 500px;
                background-color: #3dd17b;
                float: left;
           }
        </style>
    </head>

    <body>
        <div id="main" class="mainwindow"> 
            <div id="map" class="mapwindow"></div>
        </div>
    </body>

    <script src="http://api.map.baidu.com/api?v=2.0&ak=你申请的API KEY"></script>
    <script src="../js/echarts.js"></script> 
    <script src="../js/bmap.js"></script>
    <script src="./map.js"></script>

    </html>
    ```

    3. map.js 用来完成写入echarts中的图形配置文件和图形的初始化和加载
    ```
    //初始化echarts,并和框体map绑定。
    var myChart = echarts.init(document.getElementById('map'));

    //手工写入的一个迁徙线的数据，正常项目中应该是由AJAX或其他方式来获取数据。
    var linesdata = [{
        fromName: "银泰",
        toName: "路口",
        coords: [
            [120.114271,30.305938],
            [120.118951,30.309134]
        ]
    }];

    //echarts中使用地图的配置参数
    var option = {
    bmap: {
        // 百度地图中心经纬度 坐标拾取器http://api.map.baidu.com/lbsapi/getpoint/index.html
        center: [120.114271,30.305938],
        // 百度地图缩放等级，数字越大，放大越大，地图比例尺越小
        zoom: 16,
        // 是否开启拖拽缩放，可以只设置 'scale' 或者 'move'
        roam: false,
        // mapStyle是百度地图的自定义样式，见 http://developer.baidu.com/map/custom/
        mapStyle: {
            styleJson: [{
                    "featureType": "water",
                    "elementType": "all",
                    "stylers": {
                        "color": "#021019"
                    }
                },
                {
                    "featureType": "highway",
                    "elementType": "geometry.fill",
                    "stylers": {
                        "color": "#000000"
                    }
                },
                {
                    "featureType": "highway",
                    "elementType": "geometry.stroke",
                    "stylers": {
                        "color": "#147a92"
                    }
                },
                {
                    "featureType": "arterial",
                    "elementType": "geometry.fill",
                    "stylers": {
                        "color": "#000000"
                    }
                },
                {
                    "featureType": "arterial",
                    "elementType": "geometry.stroke",
                    "stylers": {
                        "color": "#0b3d51"
                    }
                },
                {
                    "featureType": "local",
                    "elementType": "geometry",
                    "stylers": {
                        "color": "#000000"
                    }
                },
                {
                    "featureType": "land",
                    "elementType": "all",
                    "stylers": {
                        "color": "#08304b"
                    }
                },
                {
                    "featureType": "railway",
                    "elementType": "geometry.fill",
                    "stylers": {
                        "color": "#000000"
                    }
                },
                {
                    "featureType": "railway",
                    "elementType": "geometry.stroke",
                    "stylers": {
                        "color": "#08304b"
                    }
                },
                {
                    "featureType": "subway",
                    "elementType": "geometry",
                    "stylers": {
                        "lightness": -70
                    }
                },
                {
                    "featureType": "building",
                    "elementType": "geometry.fill",
                    "stylers": {
                        "color": "#000000"
                    }
                },
                {
                    "featureType": "all",
                    "elementType": "labels.text.fill",
                    "stylers": {
                        "color": "#857f7f"
                    }
                },
                {
                    "featureType": "all",
                    "elementType": "labels.text.stroke",
                    "stylers": {
                        "color": "#000000"
                    }
                },
                {
                    "featureType": "building",
                    "elementType": "geometry",
                    "stylers": {
                        "color": "#022338"
                    }
                },
                {
                    "featureType": "green",
                    "elementType": "geometry",
                    "stylers": {
                        "color": "#062032"
                    }
                },
                {
                    "featureType": "boundary",
                    "elementType": "all",
                    "stylers": {
                        "color": "#1e1c1c"
                    }
                },
                {
                    "featureType": "manmade",
                    "elementType": "geometry",
                    "stylers": {
                        "color": "#022338"
                    }
                },
                {
                    "featureType": "poi",
                    "elementType": "all",
                    "stylers": {
                        "visibility": "off"
                    }
                },
                {
                    "featureType": "all",
                    "elementType": "labels.icon",
                    "stylers": {
                        "visibility": "off"
                    }
                },
                {
                    "featureType": "all",
                    "elementType": "labels.text.fill",
                    "stylers": {
                        "color": "#2da0c6",
                        "visibility": "on"
                    }
                }
            ]
        }
    },
    //series是在地图上的线条等效果的配置文件，具体可以查阅文档。
    series: [{
       type: 'lines',
          coordinateSystem: 'bmap',
          zlevel: 2,
          effect: {
             show: true,
             period: 6,
             trailLength: 0,
             symbol: 'arrow',
             symbolSize: 10
         },
         lineStyle: {
             normal: {
                 color: "#a6c84c",
                  width: 2,
                 opacity: 0.6,
                  curveness: 0.2
             }
        },
        //将手动做的一个迁徙数据放入线条的数据部分。
        data: linesdata
        }]
    };

    //配置参数传入图形实例中
    myChart.setOption(option);
    //初始化bmap和echarts实例绑定
    var bmap = myChart.getModel().getComponent('bmap').getBMap();
    bmap.addControl(new BMap.MapTypeControl());
    ```

在浏览器中打开demo.html就可以看到一个杭州市城西银泰到一个小路口的箭头指向的线段。简易的迁徙图就制作完成了。