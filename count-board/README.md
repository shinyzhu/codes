# Count Board

为训练孩子专注力设计的一个数字小游戏。

进入游戏会展示一个数字面板，同时开始计时，孩子应该按照自然数的顺序从1开始来点击数字，正确点击的数字会变成绿色，不正确的点击会变成红色，后续正确了会变成绿色，在所有数字按照顺序点击完成后结束计时。

## 关键代码

该游戏程序包含 `BoardStopwatch` 和 `NumberBoard` 两个对象，分别负责计时和展示数字面板。

首先初始化一个计时器，然后根据选择的最大数字个数初始化数字面板，最后显示在界面上。

```javascript
//init a stopwatch
window.boardWatch = BoardStopwatch.createNew("timer");

//初始化一个默认的数字面板
var gridMax = $('grid_count_select').value;
window.numberBoard = NumberBoard.createNew("numbers_table", window.boardWatch);
window.numberBoard.show(gridMax);
```

详细代码请看 [countboard.html](countboard.html)。

进入游戏在这里：[shinyzhu.com/countboard.html](http://shinyzhu.com/countboard.html)