## 1.1canvas基本语法
####1.1.1 基本操作
   
```
var     cvs = document.getElementById('cvs');//获取画布
        var ctx = cvs.getContext('2d');//获取画笔
```
####1.1.2 设置画布大小
> 通过行内或者html，不能通过css去设置，会导致变形
 
```
 <canvas id="cvs" width="500" height="500"></canvas>
```

####1.1.3 非零环绕原则
> 原理：随便找一块封闭的区域，然后在区域随便找一点，向外发一条射线， 然后会继续计数判断，初始值为0， 如果射线遇到边，这条边相当于点是逆时针，那么-1，顺时针+1，最终结果非0，那么就认为这块区域是被环绕起来的。

####1.1.4 基本绘图命令

- 设置开始绘图的位置: `ctx.moveTo( x, y )`.
- 设置直线到的位置: `ctx.lineTo( x, y )`.
- 描边绘制: `ctx.stroke()`.
- 设置填充颜色`ctx.fillStyle = 'yellow'`;
- 填充绘制: `ctx.fill()`.//根据路径进行填充
- 闭合路径: `ctx.closePath()`.
- 用来画矩形路径的: `ctx.rect( x, y, w, h )`.
- 用来绘制描边矩形，不会产生任何路径: `ctx.strokeRect( x, y, w, h )`.
- 用来绘制填充矩形，不会产生任何路径: `ctx.fillRect( x, y, w, h )`.
- 清除矩形区域`ctx.clearRect`
- 设置描边色: `ctx.strokeStyle` = `"颜色单词、rgb、rgba、css支持的方式这里都支持"`
- 清理之前的路径：`ctx.beginPath()`; 为了防止重绘之前的路径，先把他们清除掉
- 当前路径的结束点和起点连一条路径 `ctx.closePath()`;  
- `ctx.lineWidth` 设置线宽.
- `ctx.lineCap` 设置线末端类型.
    - 设置线型末端的样式, 可取值为: 'butt'( 默认 ), 'round', 'square'.
    - 'butt' 表示两端使用方形结束.
    - 'round' 表示两端使用圆角结束.
    - 'square' 表示突出的圆角结束.
- `ctx.lineJoin` 设置相交线的拐点.
    - 设置两条直线的拐点描述方式. 可以取值 'round', 'bevel', 'miter'(默认)
    - 'round' 使用圆角连接.
    - 'bevel' 使用平切连接.
    - 'miter' 使用直角转.
- 如果线帽与交点样式不一致，优先按照交点样式处理。
- `ctx.getLineDash()` 获得线段样式数组.
- `ctx.setLineDash()` 设置线段样式.
- `ctx.lineDashOffset` 绘制线段偏移量.
> `lineDashOffset` 用于设置开始绘制虚线的偏移量. 数字的正负表示左右偏移.
> `getLineDash()` 与 `setLineDash()` 方法使用数组描述实线与虚线的长度.

####1.1.5 基本例子
> 等腰三角形

```
var cvs = document.getElementById('cvs');
    var ctx = cvs.getContext('2d');
    ctx.moveTo(100, 100);
    ctx.lineTo(100 + 50 / 2, 100 + 50);
    ctx.lineTo(100 - 50 / 2, 100 + 50);
    ctx.lineTo(100, 100);
    ctx.strokeStyle = 'skyblue';
    ctx.stroke();
```
> 封装等腰三角形（函数）

```
function strokeSJX( x, y, w, h, style ) {
        ctx.beginPath();   // 为了防止重绘之前的路径，先把他们清除掉
        ctx.moveTo(x, y);
        ctx.lineTo(x + w / 2, y + h);
        ctx.lineTo(x - w / 2, y + h);
        ctx.lineTo(x, y);
        ctx.strokeStyle = style;
        ctx.stroke();
    }

    strokeSJX( 100, 100, 100, 50, 'red' );
```

> 封装渐变矩形

```
    function JX( x, y, w, h, direction ) {
        var i = 0, lineNum;
        // 如果是横方式，线的总数为宽度；
        // 如果是竖方式，线的总数为高度。
        lineNum = direction === 'h'? w : h;
        // 分别画每一条不同颜色的线，共同组成一个渐变矩形
        for ( ; i < lineNum; i++ ) {
            ctx.beginPath();
            if ( direction === 'h' ) {
                ctx.moveTo( x + i, y);
                ctx.lineTo( x + i, y + h);
            }else {
                ctx.moveTo( x, y + i);
                ctx.lineTo( x + w, y + i);
            }
            ctx.strokeStyle = 'rgb(' + Math.ceil(255 / lineNum * i) + ', 0, 0)';
            ctx.stroke();
        }
    }
    // 测试
    JX( 10, 10, 100, 100, 'h' );
    JX( 210, 10, 80, 100 );
```

> 网格

```
        * 画网格，
        * 先画N多横线，
        * 再画N多竖线。
        // 画宽为10，高为10的网格
        var w = 10, h = 10,
            cvsW = cvs.width,
            cvsH = cvs.height;
        // 先画横线
        for ( var i = 1, num = cvsH / h; i < num; i++ ) {
            ctx.moveTo( 0, h * i );
            ctx.lineTo( cvsW, h * i );
        }
        // 再画竖线
        for ( var i = 1, num = cvsW / w; i < num; i++ ) {
            ctx.moveTo( w * i, 0 );
            ctx.lineTo( w * i, cvsH );
        }
        ctx.stroke();
```

> 在自定义的坐标里画点

```
        // 定义坐标轴距离画布的边距
        var paddingLeft = 10;
        var paddingTop = 10;
        var paddingRight = 10;
        var paddingBottom = 10;
        // 箭头的宽高
        var arrow = {
            w: 10,
            h: 20
        };
        // 先画坐标轴中的竖线
        // 竖线的顶点坐标为( paddingLeft, paddingTop );
        // 原点的坐标为( paddingLeft, cvs.height -  paddingBottom );
        // 横线的顶点坐标为( cvs.width - paddingRight, cvs.height -  paddingBottom );
        var s = {
            x: paddingLeft,
            y: paddingTop
        };
        var o = {
            x: paddingLeft,
            y: cvs.height -  paddingBottom
        };
        var h = {
            x: cvs.width - paddingRight,
            y: cvs.height -  paddingBottom
        };
        // 画坐标轴的两条线
        ctx.moveTo( s.x, s.y );
        ctx.lineTo( o.x, o.y );
        ctx.lineTo( h.x, h.y );
        // 先画竖线箭头
        ctx.moveTo( s.x, s.y );
        ctx.lineTo( s.x - arrow.w / 2, s.y + arrow.h );
        ctx.moveTo( s.x, s.y );
        ctx.lineTo( s.x + arrow.w / 2, s.y + arrow.h );
        // 再画横线箭头
        ctx.moveTo( h.x, h.y );
        ctx.lineTo( h.x - arrow.h, h.y - arrow.w / 2 );
        ctx.moveTo( h.x, h.y );
        ctx.lineTo( h.x - arrow.h, h.y + arrow.w / 2 );
        ctx.stroke();
        /*
        * 在10，10位置画一点:
        * 该点坐标的算法：
        * x轴 = 原点的x轴 + 该点和原点的距离
        * y轴 = 原点的y轴 - 该点和原点的距离
        * */
        // ctx.fillRect( o.x + 10, o.y - 10, 1, 1 );
        /*
        * 需求：在自定义的坐标轴中画N多点
        * */
        var points = [ 10, 10, 30, 30, 50, 30, 70, 40, 110, 150 ];
        for ( var i = 0, len = points.length; i < len; i+=2 ) {
            ctx.fillRect( o.x + points[i], o.y - points[i+1], 3, 3 );
        }
```

> 补充上面代码块,在上述的基础上画折线

```
        ctx.beginPath(); // 新开辟路径，现在就没有任何路径，以及路径的起点和结束点了。
        // 再画折线
        for ( var i = 0, len = points.length; i < len; i+=2 ) {
            ctx.lineTo( o.x + points[i], o.y - points[i+1] );
        }
        ctx.stroke();
```

> 等比例的放大折线图

```
    var cvs=document.getElementById("cvs")
        var ctx=cvs.getContext("2d");
        var paddingLeft = 10;
        var paddingTop = 10;
        var paddingRight = 10;
        var paddingBottom = 10;
            // 箭头的宽高
        var arrow = {
            w: 10,
            h: 20
        };
            // 先画坐标轴中的竖线
        // 竖线的顶点坐标为( paddingLeft, paddingTop );
        // 原点的坐标为( paddingLeft, cvs.height -  paddingBottom );
        // 横线的顶点坐标为( cvs.width - paddingRight, cvs.height -  paddingBottom );
        var s = {
            x: paddingLeft,
            y: paddingTop
        };
        var o = {
            x: paddingLeft,
            y: cvs.height -  paddingBottom
        };
        var h = {
            x: cvs.width - paddingRight,
            y: cvs.height -  paddingBottom
        };
        ctx.moveTo(s.x, s.y)
        ctx.lineTo(o.x, o.y)
        ctx.lineTo(h.x, h.y)
        //先画竖箭头
        ctx.moveTo(s.x, s.y);
        ctx.lineTo(s.x-arrow.w/2, s.y+arrow.h);
        ctx.moveTo(s.x, s.y);
        ctx.lineTo(s.x+arrow.w/2, s.y+arrow.h);
        //横箭头
        ctx.moveTo(h.x, h.y);
        ctx.lineTo(h.x-arrow.h, h.y-arrow.w/2);
        ctx.moveTo(h.x, h.y);
        ctx.lineTo(h.x-arrow.h, h.y+arrow.w/2);
        ctx.stroke();
        //折线点的坐标
        var points = [ 10, 10, 30, 30, 50, 30, 70, 40, 110, 150 ];
        //求自定义的画布的最大显示面积
        var maxKD = {
            x: cvs.width - paddingLeft - paddingRight - arrow.h,
            y: cvs.height - paddingTop - paddingBottom - arrow.h
        };
        //对折线点进行区别，x,y轴的数据都提出来，然后放大比例进行处理
        var pointXs=[];
        var pointYs=[];
        for(var i= 0,len=points.length;i<len;i++){
            if(i%2===0){
                pointXs.push(points[i])
            }else{
                pointYs.push(points[i])
            }
        }
    //求折线点的最大x,y值
        var pointsMaxX = Math.max.apply(null, pointXs);
        var pointsMaxY = Math.max.apply(null, pointYs);
    //    求出比例
        var ratioX = maxKD.x / pointsMaxX;
        var ratioY = maxKD.y / pointsMaxY;
    //    画折线的点
        for(var i= 0,len=points.length;i<len;i+=2){
            ctx.fillRect(o.x+points[i]*ratioX, o.y-points[i+1]*ratioY,4,4)
        }
        ctx.beginPath();
        for(var i= 0,len=points.length;i<len;i+=2){
            ctx.lineTo(o.x+points[i]*ratioX+2, o.y-points[i+1]*ratioY+2)
        }
        ctx.stroke();
```