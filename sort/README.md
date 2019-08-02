# 排序算法

## 排序算法说明

### 1、排序的定义：对一序列对象根据某个关键字进行排序；

输入：n个数：a1,a2,a3,...,an 输出：n个数的排列:a1',a2',a3',...,an'，使得a1'<=a2'<=a3'<=...<=an'。

再讲的形象点就是排排坐，调座位，高的站在后面，矮的站在前面咯。

### 2、对于评述算法优劣术语的说明

稳定：如果a原本在b前面，而a=b，排序之后a仍然在b的前面；
不稳定：如果a原本在b的前面，而a=b，排序之后a可能会出现在b的后面；
内排序：所有排序操作都在内存中完成；
外排序：由于数据太大，因此把数据放在磁盘中，而排序通过磁盘和内存的数据传输才能进行；
时间复杂度: 一个算法执行所耗费的时间。
空间复杂度: 运行完一个程序需内存的大小。

### 3、排序算法图片总结:

排序对比：

![markdown](https://user-gold-cdn.xitu.io/2016/11/29/4abde1748817d7f35f2bf8b6a058aa40?imageView2/0/w/1280/h/960/format/webp/ignore-error/1 "markdown")

图片名词解释： n: 数据规模 k:“桶”的个数 In-place: 占用常数内存，不占用额外内存 Out-place: 占用额外内存

排序分类：

![markdown](https://user-gold-cdn.xitu.io/2016/11/29/b93d9288a682f5538667fb1fa4c65220?imageView2/0/w/1280/h/960/format/webp/ignore-error/1 "markdown")

### 4、




**基本排序算法**


### 1.冒泡排序 ###

冒泡排序是最慢的排序算法之一, 也是最容易实现的排序算法.使用这种算法进行排序时,数据值会像气泡一样从数组的一端漂浮到另一端,所以称之为冒泡排序.假设要对数组按照升序排列,较大的值会浮动到数组的右侧,较小值会浮到左侧.

#### 原理: ####

简单来说就是相邻两个元素进行对比，按照你需要的排序方式（升序or降序）进行位置替换，替换时需要额外一个变量当作中间变量去暂存值。

冒泡排序动图演示:

![markdown](https://images2018.cnblogs.com/blog/1055753/201804/1055753-20180417094344056-474474328.gif "markdown")

**代码:**

    function bubbleSort(arr) {
        console.time('改进前冒泡排序耗时');
        var len = arr.length;
        for (var i = 0; i < len; i++) {
            for (var j = 0; j < len - 1 - i; j++) {
                if (arr[j] > arr[j+1]) {        //相邻元素两两对比
                    var temp = arr[j+1];        //元素交换
                    arr[j+1] = arr[j];
                    arr[j] = temp;
                }
            }
        }
        console.timeEnd('改进前冒泡排序耗时');
        return arr;
    }
    var arr=[3,44,38,5,47,15,36,26,27,2,46,4,19,50,48];
    console.log(bubbleSort(arr));//[2, 3, 4, 5, 15, 19, 26, 27, 36, 38, 44, 46, 47, 48, 50]

改进冒泡排序： 设置一标志性变量pos,用于记录每趟排序中最后一次进行交换的位置。由于pos位置之后的记录均已交换到位,故在进行下一趟排序时只要扫描到pos位置即可。

改进后算法如下:

    function bubbleSort2(arr) {
        console.time('改进后冒泡排序耗时');
        var i = arr.length-1;  //初始时,最后位置保持不变
        while ( i> 0) {
            var pos= 0; //每趟开始时,无记录交换
            for (var j= 0; j< i; j++){
                if (arr[j]> arr[j+1]) {
                    pos= j; //记录交换的位置
                    var tmp = arr[j]; arr[j]=arr[j+1];arr[j+1]=tmp;
                }
            }
            i= pos; //为下一趟排序作准备
        }
        console.timeEnd('改进后冒泡排序耗时');
        return arr;
    }
    var arr=[3,44,38,5,47,15,36,26,27,2,46,4,19,50,48];
    console.log(bubbleSort2(arr));//[2, 3, 4, 5, 15, 19, 26, 27, 36, 38, 44, 46, 47, 48, 50]


### 2、选择排序 ###

表现最稳定的排序算法之一(这个稳定不是指算法层面上的稳定哈，相信聪明的你能明白我说的意思2333)，因为无论什么数据进去都是O(n²)的时间复杂度.....所以用到它的时候，数据规模越小越好。唯一的好处可能就是不占用额外的内存空间了吧。理论上讲，选择排序可能也是平时排序一般人想到的最多的排序方法了吧。


##### 原理: #####
　　首先从原始数组中找到最小的元素，并把该元素放在数组的最前面，然后再从剩下的元素中寻找最小的元素，放在之前最小元素的后面，直到排序完毕

示意图:

![markdown](https://images2018.cnblogs.com/blog/1055753/201804/1055753-20180417094725917-1229969107.gif "markdown")

**代码:** 

    function selectionSort(arr) {
        console.time('选择排序耗时');
        var len = arr.length;
        var minIndex, temp;
        for (var i = 0; i < len - 1; i++) {
          minIndex = i;
          for (var j = i + 1; j < len; j++) {
            if (arr[j] < arr[minIndex]) {
              minIndex = j;
            }
          }
          temp = arr[i];
          arr[i] = arr[minIndex];
          arr[minIndex] = temp;
        }
        console.timeEnd('选择排序耗时');
        return arr;
    }
    var arr=[3,44,38,5,47,15,36,26,27,2,46,4,19,50,48];
    console.log(selectionSort(arr));//[2, 3, 4, 5, 15, 19, 26, 27, 36, 38, 44, 46, 47, 48, 50]复制代码


### 3、插入排序 ###

    插入排序的代码实现虽然没有冒泡排序和选择排序那么简单粗暴，但它的原理应该是最容易理解的了，因为只要打过扑克牌的人都应该能够秒懂。当然，如果你说你打扑克牌摸牌的时候从来不按牌的大小整理牌，那估计这辈子你对插入排序的算法都不会产生任何兴趣了.....哈哈😄


#### 原理: ####

　　从第二个元素开始(假定第一个元素已经排序了),取出这个元素,在已经排序的元素中从后向前进行比较,如果该元素大于这个元素,就将该元素移动到下一个位置,然后继续向前进行比较,直到找到小于或者等于该元素的位置,将该元素插入到这个位置后.重复这个步骤直到排序完成.

**示意图:**


![markdown](https://images2018.cnblogs.com/blog/1055753/201804/1055753-20180417095013077-1931405500.gif "markdown")

**代码:** 

	function insertionSort(arr) {
        var len = arr.length;
        var preIndex, current;
        for (var i = 1; i < len; i++) {
          preIndex = i - 1;
          current = arr[i];
          while (preIndex >= 0 && arr[preIndex] > current) {
            arr[preIndex + 1] = arr[preIndex];
            preIndex--;
          }
          arr[preIndex + 1] = current;
        }
        return arr;
      }



### 快速排序 ###

快速排序的名字起的是简单粗暴，因为一听到这个名字你就知道它存在的意义，就是快，而且效率高! 它是处理大数据最快的排序算法之一了。

#### 算法简介 ####

快速排序的基本思想：通过一趟排序将待排记录分隔成独立的两部分，其中一部分记录的关键字均比另一部分的关键字小，则可分别对这两部分记录继续进行排序，以达到整个序列有序

#### 法描述和实现 ####

选择一个基准，将比基准小的放左边，比基准小的放在右边（基准处在中间位置）

    function quickSort2(arr) {
        //如果数组<=1,则直接返回
        if (arr.length <= 1) { return arr; }
        var pivotIndex = Math.floor(arr.length / 2);
        //找基准，并把基准从原数组删除
        var pivot = arr.splice(pivotIndex, 1)[0];
        //定义左右数组
        var left = [];
        var right = [];

        //比基准小的放在left，比基准大的放在right
        for (var i = 0; i < arr.length; i++) {
            if (arr[i] <= pivot) {
                left.push(arr[i]);
            }
            else {
                right.push(arr[i]);
            }
        }
        //递归
        return quickSort2(left).concat([pivot], quickSort2(right));
    }

快速排序动图演示：

![markdown](https://user-gold-cdn.xitu.io/2016/11/29/dd9dc195a7331351671fe9ac4f7d5aa4?imageView2/0/w/1280/h/960/format/webp/ignore-error/1 "markdown")


### 希尔排序 ###

1959年Shell发明； 第一个突破O(n^2)的排序算法；是简单插入排序的改进版；它与插入排序的不同之处在于，它会优先比较距离较远的元素。希尔排序又叫缩小增量排序

#### 原理： ####

利用步长来进行两两元素比较，然后缩减步长在进行排序。

　　详细的说明：希尔排序的实质是分组插入排序，该方法又称缩小增量排序。该方法的基本思想是：先将整个待排元素序列分割为若干个子序列（由相隔某个‘增量’的元素组成的）分别进行直接插入排序，然后依次缩减增量再进行排序，带这个序列中的元素基本有序（增量足够小）时，再对全体元素进行一次直接插入排序。因为直接插入排序在元素基本有序的情况下（接近最好情况）效率是很高的，因此希尔排序在时间效率上有较大的提高。 

与插入排序的不同之处：它会优先比较距离较远的元素

希尔排序图示：

![markdown](https://user-gold-cdn.xitu.io/2016/11/29/86dc5422e18bd545afef8898b1bb1315?imageView2/0/w/1280/h/960/format/webp/ignore-error/1 "markdown")

    function shellSort(arr) {
      let temp,
        gap = 1;
      while (gap < arr.length / 3) {
        gap = gap * 3 + 1//动态定义间隔序列
      }
      for (gap; gap > 0; gap = Math.floor(gap / 3)) {//控制步长（间隔）并不断缩小
        for (var i = gap; i < arr.length; i++) {//按照增量个数对序列进行排序
          temp = arr[i]
          for (var j = i - gap; j >= 0 && arr[j] > temp; j -= gap) {//例：j=0  arr[1]>arr[5]
            arr[j + gap] = arr[j]
          }
          arr[j + gap] = temp
        }
      }
      return arr
    }


### 堆排序 ###

堆排序可以说是一种利用堆的概念来排序的选择排序。

#### 算法简介 ####

堆排序（Heapsort）是指利用堆这种数据结构所设计的一种排序算法。堆积是一个近似完全二叉树的结构，并同时满足堆积的性质：即子结点的键值或索引总是小于（或者大于）它的父节点。

#### 算法描述和实现 ####

具体算法描述如下：

<1>.将初始待排序关键字序列(R1,R2....Rn)构建成大顶堆，此堆为初始的无序区；
<2>.将堆顶元素R[1]与最后一个元素R[n]交换，此时得到新的无序区(R1,R2,......Rn-1)和新的有序区(Rn),且满足R[1,2...n-1]<=R[n]；
<3>.由于交换后新的堆顶R[1]可能违反堆的性质，因此需要对当前无序区(R1,R2,......Rn-1)调整为新堆，然后再次将R[1]与无序区最后一个元素交换，得到新的无序区(R1,R2....Rn-2)和新的有序区(Rn-1,Rn)。不断重复此过程直到有序区的元素个数为n-1，则整个排序过程完成。

代码：

    /*方法说明：堆排序
    @param  array 待排序数组*/
    function heapSort(array) {
        console.time('堆排序耗时');
        if (Object.prototype.toString.call(array).slice(8, -1) === 'Array') {
            //建堆
            var heapSize = array.length, temp;
            for (var i = Math.floor(heapSize / 2) - 1; i >= 0; i--) {
                heapify(array, i, heapSize);
            }

            //堆排序
            for (var j = heapSize - 1; j >= 1; j--) {
                temp = array[0];
                array[0] = array[j];
                array[j] = temp;
                heapify(array, 0, --heapSize);
            }
            console.timeEnd('堆排序耗时');
            return array;
        } else {
            return 'array is not an Array!';
        }
    }
    /*方法说明：维护堆的性质
    @param  arr 数组
    @param  x   数组下标
    @param  len 堆大小*/
    function heapify(arr, x, len) {
        if (Object.prototype.toString.call(arr).slice(8, -1) === 'Array' && typeof x === 'number') {
            var l = 2 * x + 1, r = 2 * x + 2, largest = x, temp;
            if (l < len && arr[l] > arr[largest]) {
                largest = l;
            }
            if (r < len && arr[r] > arr[largest]) {
                largest = r;
            }
            if (largest != x) {
                temp = arr[x];
                arr[x] = arr[largest];
                arr[largest] = temp;
                heapify(arr, largest, len);
            }
        } else {
            return 'arr is not an Array or x is not a number!';
        }
    }
    var arr=[91,60,96,13,35,65,46,65,10,30,20,31,77,81,22];
    console.log(heapSort(arr));//[10, 13, 20, 22, 30, 31, 35, 46, 60, 65, 65, 77, 81, 91, 96]


### 归并排序 ###

和选择排序一样，归并排序的性能不受输入数据的影响，但表现比选择排序好的多，因为始终都是O(n log n）的时间复杂度。代价是需要额外的内存空间。

#### 原理： ####

　归并排序是建立在归并操作上的一种有效的排序算法。该算法是采用分治法（Divide and Conquer）的一个非常典型的应用。归并排序是一种稳定的排序方法。将已有序的子序列合并，得到完全有序的序列；即先使每个子序列有序，再使子序列段间有序。若将两个有序表合并成一个有序表，称为2-路归并。


    function mergeSort(arr) {  //采用自上而下的递归方法
    var len = arr.length;
    if(len < 2) {
        return arr;
    }
    var middle = Math.floor(len / 2),
        left = arr.slice(0, middle),
        right = arr.slice(middle);
    return merge(mergeSort(left), mergeSort(right));
}

function merge(left, right)
{
    var result = [];
    console.time('归并排序耗时');
    while (left.length && right.length) {
        if (left[0] <= right[0]) {
            result.push(left.shift());
        } else {
            result.push(right.shift());
        }
    }

    while (left.length)
        result.push(left.shift());

    while (right.length)
        result.push(right.shift());
    console.timeEnd('归并排序耗时');
    return result;
}
var arr=[3,44,38,5,47,15,36,26,27,2,46,4,19,50,48];
console.log(mergeSort(arr));


![markdown](https://user-gold-cdn.xitu.io/2016/11/29/33d105e7e7e9c60221c445f5684ccfb6?imageView2/0/w/1280/h/960/format/webp/ignore-error/1 "markdown")

