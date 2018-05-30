# Design_pattern

## 常用的设计模式

> 1、模版模式

<hr>

    var Fruit = function(param) {

    var buy = param.buy || function() {
        console.log('买水果！');
    }
    var sell = param.sell || function() {
        console.log('卖水果！');
    }
    var F = function() {}
    F.prototype.init = function() {
        buy();
        sell();
    }
    return F;

}

    var Apple = Fruit({

    buy: function() {
        console.log('买苹果');
    },
    sell: function() {
        console.log('卖苹果');
    }

})
var a = new Apple();
a.init();

<hr>

> 2、代理模式

```java
int a;
int b;
```

<hr>

> 3、工厂模式

> 4、中介者模式

<!DOCTYPE html>

    <html lang="en">

    <head>
        <meta charset="UTF-8">
        <meta name="viewport" content="width=device-width, initial-scale=1.0">
        <meta http-equiv="X-UA-Compatible" content="ie=edge">
        <title></title>
    </head>

    <body>
        <!-- 门店 品牌 内存 颜色 数量....=》 有货 -->
        选择颜色
        <select id="colorSelect">
            <option value="">请选择</option>
            <option value="red">红色</option>
            <option value="blue">蓝色</option>
        </select>
        <br/> 选择内存
        <select id="memorySelect">
            <option value="">请选择</option>
            <option value="16G">16G</option>
            <option value="32G">32G</option>
        </select>
        <br/> 选择品牌
        <select id="boardSelect">
            <option value="">请选择</option>
            <option value="iphone">iphone</option>
            <option value="xiaomi">xiaomi</option>
        </select>
        <br> 输入购买数量 <input id="numberInput" type="text" />
        <br/> 您选择了颜色:
        <div id="colorInfo"></div>
        您选择的内存:
        <div id="memoryInfo"></div>
        您选择了品牌:
        <div id="boardInfo"></div>
        您选择了数量:
        <div id="numberInfo"></div>

        <button id="nextBtn" disabled="true">请选择手机颜色和购买数量</button>
        <script>
            const goods = {
                "red|32G|iphone": 3,
                "red|16G|iphone": 0,
                "blue|32G|xiaomi": 1,
                "blue|16G|xiaomi": 6
            };

            const mediator = (function() {
                const colorSelect = document.querySelector('#colorSelect'),
                    memorySelect = document.querySelector('#memorySelect'),
                    numInput = document.querySelector('#numberInput'),
                    boardSelect = document.querySelector('#boardSelect'),
                    colorInfo = document.querySelector('#colorInfo'),
                    memeoryInfo = document.querySelector('#memoryInfo'),
                    numberInfo = document.querySelector('#numberInfo'),
                    boardInfo = document.querySelector('#boardInfo'),
                    nextBtn = document.querySelector('#nextBtn');
                colorSelect.onchange = function() {
                    mediator.changed(this);
                }
                memorySelect.onchange = function() {
                    mediator.changed(this);
                }
                numInput.oninput = function() {
                    mediator.changed(this);
                }
                boardSelect.oninput = function() {
                    mediator.changed(this);
                }

                return {
                    changed: function(obj) {
                        //中介者模式，让我们将复杂的情况收纳至一个中介者对象，之后，新的情况，新的处理，都将集中在changed一处
                        console.log(this);
                        let color = colorSelect.value,
                            memory = memorySelect.value,
                            number = numInput.value,
                            board = boardSelect.value;
                        if (obj === colorSelect) {
                            colorInfo.innerHTML = color;
                        } else if (obj === memorySelect) {
                            memoryInfo.innerHTML = memory;
                        } else if (obj === numInput) {
                            numberInfo.innerHTML = number;
                        } else if (obj === boardSelect) {
                            boardInfo.innerHTML = board;
                        }
                        if (!color) {
                            nextBtn.disabled = true;
                            nextBtn.innerHTML = '请选择手机颜色';
                            return;
                        }

                        if (!memory) {
                            nextBtn.disabled = true;
                            nextBtn.innerHTML = '请选择内存大小';
                            return;
                        }
                        if (!board) {
                            nextBtn.disabled = true;
                            nextBtn.innerHTML = '请选择手机品牌';
                            return;
                        }

                        if (!Number.isInteger(parseInt(number)) || parseInt(number) <= 0) {
                            nextBtn.disabled = true;
                            nextBtn.innerHTML = '请输入正确的数量';
                            return;
                        }


                        let stock = goods[`${color}|${memory}|${board}`];
                        if (number > stock) {
                            nextBtn.disabled = true;
                            nextBtn.innerHTML = '库存不足';
                            return;
                        }

                        nextBtn.disabled = false;
                        nextBtn.innerHTML = '放入购物车';
                    }
                }
            })();
        </script>

    </body>

    </html>
