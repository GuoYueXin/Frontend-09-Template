## 学习笔记

### UI生成

地图整体为 ```100 * 100``` 的方格组成，这里使用一维数组对其进行抽象，定义 ```map = new Array(100).fill(0)``` 。依然可以利用 ```100 * x + y = index(数组下标)``` 来对其使用二维数组的形式来表示，一维数组在一定程度上效率比二维数组更高。

使用双层循环来生成方格并插入到dom中，样式布局采用 ```flex``` 布局， 每个方格宽度为 1% ，并设置 ```box-sizing: border-box;``` 。

为每个方格设置 ```mousemove``` 监听，当鼠标左键移动时添加路径；当鼠标右键移动时清除路径。

将数据存储在 ```localStore``` 中，方便在下次打开时路径依然存在。

### 广度优先搜索

利用数组的 ```push``` 和 ```shift``` 来实现队列，从起始节点开始，依次搜索每个节点的 **相邻可到达** 节点，直到找到目标节点位置。

### 寻路过程可视化

利用异步```（async/await）```为寻路过程添加延迟来实现寻路过程的可视化。

### 启发式搜索

实现 ```Sorted``` 数据结构，该数据结构中的 ```take``` 操作支持自定义规则，取出集合中的某个值，在这里我们可以计算当前节点与目标节点的距离来进行比较，从而取出距离目标节点最近的点。依次进行此操作，从而优化寻路性能。

```Javascript
class Sorted {
    constructor(data, compare) {
        this.data = data.slice();
        this.compare = compare || ((a, b) => a - b);
    }

    take() {
        if (!this.data.length)
            return;
        let min = this.data[0];
        let minIndex = 0;

        for (let i = 1; i < this.data.length; i++) {
            if (this.compare(this.data[i], min) < 0) {
                min = this.data[i];
                minIndex = i;
            }
        }
        this.data[minIndex] = this.data[this.data.length - 1];
        this.data.pop();
        return min;
    }
    give(val) {
        this.data.push(val);
    }

    get length() {
        return this.data.length;
    }
}

```