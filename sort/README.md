# 前端算法-基本排序算法比较

**基本排序算法**

>这里主要介绍的基本排序算法主要包括: 冒泡排序,选择排序,插入排序,之后的文章会介绍希尔排序,快速排序等高级排序算法, 文章后面会对这几个算法进行性能比较.
基本排序算法的核心思想是对一组数据按照一定的顺序重新排列. 重新排列主要就是嵌套的for循环. 外循环会遍历数组每一项,内循环进行元素的比较.

###1.冒泡排序###

冒泡排序是最慢的排序算法之一, 也是最容易实现的排序算法.使用这种算法进行排序时,数据值会像气泡一样从数组的一端漂浮到另一端,所以称之为冒泡排序.假设要对数组按照升序排列,较大的值会浮动到数组的右侧,较小值会浮到左侧.

####原理: ####

简单来说就是相邻两个元素进行对比，按照你需要的排序方式（升序or降序）进行位置替换，替换时需要额外一个变量当作中间变量去暂存值。

![markdown](https://images2018.cnblogs.com/blog/1055753/201804/1055753-20180417094344056-474474328.gif "markdown")

**代码:**

    function bubbleSort(array){
        var len = array.length,i,j, d;
        for(i=len;i--;){
            for(j=0; j<i; j++){
                var z = j+1;
                if(array[j] > array[z]){
                    d = array[j];
                    array[j] = array[z];
                    array[z] = d;
                }
            }
        }
        return array;
    };

###2.选择排序###

#####原理: #####
　　首先找出当前元素中最小的元素，并放到排序序列的起始位置，然后再从剩余的元素中寻找最小的元素，然后放到已排序序列的末尾。以此类推，直到排序完成。

示意图:

![markdown](https://images2018.cnblogs.com/blog/1055753/201804/1055753-20180417094725917-1229969107.gif "markdown")

**代码: ** 

      function selectionSort(arr) {
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
        return arr;
      }

###3.插入排序 ###

####原理: ####

　　从第二个元素开始(假定第一个元素已经排序了),取出这个元素,在已经排序的元素中从后向前进行比较,如果该元素大于这个元素,就将该元素移动到下一个位置,然后继续向前进行比较,直到找到小于或者等于该元素的位置,将该元素插入到这个位置后.重复这个步骤直到排序完成.

**示意图: **


![markdown](https://images2018.cnblogs.com/blog/1055753/201804/1055753-20180417095013077-1931405500.gif "markdown")

**代码: ** 

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