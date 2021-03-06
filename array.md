# array

## 零、基础知识

## 一、Leetcode 刷题

### 950. 按递增顺序显示卡牌 deckRevealedIncreasing

- 难度：medium
- 题意解析：目标构建一套牌组，使之实现"抽一张显示并丢弃，然后把下一张放到牌组底部, 最终能够以**递增顺序**显示牌组"的效果.
- 初始思路：先排序数组使之实现**递增顺序**，然后逐步反推牌组构造。
  - 思路推导：
    - 牌组-》结果：抽一张显示并丢弃，把下一张放到牌组底部
    - 结果-》牌组：把最后一张放到牌组顶部，再在顶部加入目标卡牌
  - 实现：

        ``` js
        var deckRevealedIncreasing = function(deck) {
            // 求解目标：一副牌，如何排序才能在题目规则的抽卡方式下，以递增顺序显示卡牌牌组顺序
            let len = deck.length;
            if (len < 2) return deck;
            deck.sort((a,b)=>b-a);  // 1. 先排列成目标顺序(然后由第二步推出反序更好)
            // 2、 分析
            // 抽牌过程：牌组顶部抽一张，将下一张放到牌组底部
            // 恢复过程：将牌组底部的牌弹出并放到牌组顶部，再在牌组顶部放一张牌
            // 顺序分析：由于answer的第一张即牌组顶部，故应该从最大的牌开始处理，故牌组处理为反序
            let resultArr = [deck[1], deck[0]];
            for (let i=2; i<len; i++) {
                resultArr.unshift(deck[i], resultArr.pop());
            }
            return resultArr;
        };
        ```

### 78. 子集 subsets

- 难度：medium
- 题意解析：给定一个**不包含重复元素**的整数数组 nums ，返回该数组所有可能的子集。不包含重复元素说明无需考虑排除组合后相同的情况，只需要遍历所有自由组合的结果即可。
- 初始思路：**迭代**. 当前数组有n个元素，想要得到数组元素的组合结果，可以先求得(n-1)个元素的组合结果，再遍历(n-1)个元素的组合结果并为每个元素数组加上最后一个元素。
  - TODO: 《之美》-迭代篇
  - 实现：

        ``` js
        var subsets = function (nums) {
            let numsLen = nums.length;
            if (numsLen === 0) return [[]];             //  0. 边界设定
            let lastNum = nums.splice(numsLen-1, 1);    //  1. 移除并取得最后一位
            let resultArr = subsets(nums);              //  2. 将裁减掉最后一位的数组用于迭代
            for (let i=0, len=resultArr.length; i<len; i++) {
                // 3. 循环取得每个元素数组并为之加上最后一个元素
                let tempItemArr = resultArr[i].slice();     // 复制得到”新元素数组“
                tempItemArr.push(lastNum);                  // 将最后一个元素推入”新元素数组“
                resultArr.push(tempItemArr);                // 将”新元素数组“推入结果
            }
            return resultArr;
        }
        ```

- 其他思路：深度优先（TODO）
- 大神思路：**位操作法**. 首先给定数组有n个元素, 在给定数组**不包含重复元素**的情况下必有 2^n个结果, 于是遍历从 0-》2^n-1, 并且在每次遍历中通过位运算确定将哪些元素组合为临时数组加入最终结果。
  - 实现：

        ``` js
        var subsets = function(nums) {
            let len = nums.length;
            if (len === 0) return [[]];
            let resultArr = [];
            let resultLen = Math.pow(2, len);
            for (let i=0; i<resultLen; i++) {
                let tempArr = [];
                let count = 0;
                while (count < len) {
                    if (i & Math.pow(2, count)) {
                        tempArr.push(nums[count]);
                    }
                    count++;
                }
                resultArr.push(tempArr);
            }
            return resultArr;
        };
        ```

### 90. 子集II subsetsWithDup

- 难度: medium
- 题意解析：78题的扩展题，区别自傲与给定的整数数组 nums **可能包含重复元素**, 且**解集不能包含重复的子集**。 上面的**位操作法**在这里是失效的。
- 初始思路: 无
- 大神思路: 深度优先

### 283.移动零

- 刷题进度:
  - [x] 一次遍历暴力法.
  - [ ] xxx
  - [ ] xxx
- 难度: easy.
- 题意解析: 给定数组，将 0 全部移到数组末位，不得影响其他数字顺序.
  - Tip: 要求原地排序.
- 输入处理: 无.
- 初始思路: 一次遍历暴力法.
  - 思路: 遍历，如果是 0 则移除再插入末尾.
  - 复杂度分析:
    - 时间: O(n^2). 循环 O(n), 数组剪切方法复杂度 O(n) & 推入数组复杂度 O(1)， 故 O(n*(n+1)) => O(n^2).
      - 最好: 不剪切 O(n).
      - 最坏: 前一半为 0, O(n/2 * n) => O(n^2).
    - 空间: O(1). 原地排序故 O(1).
  - Leetcode 结果:
    - 执行用时: 76 ms, 在所有 JavaScript 提交中击败了 70.6 %的用户
    - 内存消耗: 35.7 MB, 在所有 JavaScript 提交中击败 36.8 %的用户
  - 实现:

        ``` js
        var moveZeroes = function(nums) {
            for (let i=0, len=nums.length; i<len; i++) {
                if (nums[i] === 0) {
                    nums.splice(i, 1);
                    nums.push(0);
                    i--;
                    len--;
                }
            }
            return nums;
        };
        ```

- 第二思路:
  - 思路:
  - 复杂度分析:
    - 时间:
    - 空间:
  - Leetcode 结果:
    - 执行用时: ms, 在所有 JavaScript 提交中击败了  %的用户
    - 内存消耗: MB, 在所有 JavaScript 提交中击败  %的用户
  - 实现:

        ``` js
        ```

### 27. 移除元素

- 刷题进度:
  - [x] 一次遍历暴力法.
  - [ ] xxx
  - [ ] xxx
- 难度: easy.
- 题意解析: 给定数组并移除所有值为 val 的元素.
- 输入处理: 无需.
- 初始思路: 一次遍历暴力法.
  - 思路: 遍历对比，匹配成功则移除.
  - 复杂度分析:
    - 时间: O(n^2). 循环 O(n), 数组剪切方法复杂度 O(n)， 故 O(n^2).
      - 最好: 不剪切 O(n).
      - 最坏: 前一半为 0, O(n/2 * n) => O(n^2).
    - 空间: O(1).
  - Leetcode 结果:
    - 执行用时: 72 ms, 在所有 JavaScript 提交中击败了 45 %的用户
    - 内存消耗: 34 MB, 在所有 JavaScript 提交中击败 11.8 %的用户
  - 实现:

        ``` js
        var removeElement = function(nums, val) {
            for (let i=0, len=nums.length; i < len; i++) {
                if (nums[i] === val) {
                    nums.splice(i, 1);
                    i--;
                    len--;
                }
            }
            return nums.length;
        };
        ```

- 第二思路:
  - 思路:
  - 复杂度分析:
    - 时间:
    - 空间:
  - Leetcode 结果:
    - 执行用时: ms, 在所有 JavaScript 提交中击败了  %的用户
    - 内存消耗: MB, 在所有 JavaScript 提交中击败  %的用户
  - 实现:

        ``` js
        ```

### 26.删除排序数组中的重复项

- 刷题进度:
  - [x] 两次遍历暴力法.
  - [ ] xxx
  - [ ] xxx
- 难度: easy.
- 题意解析: 如题.
  - Tip1: 数组为已排序数组.
  - Tip2: 原地排序.
- 输入处理: 无需.
- 初始思路: 两次遍历暴力法.
  - 思路: 两次遍历推进，重复就删后一个.
  - 复杂度分析:
    - 时间: O(n^3). 遍历 O(n^2), 删除 O(n).
    - 空间: O(1).
  - Leetcode 结果:
    - 执行用时: 108 ms, 在所有 JavaScript 提交中击败了 40 %的用户
    - 内存消耗: 38 MB, 在所有 JavaScript 提交中击败 7.7 %的用户
  - 实现:

        ``` js
        var removeDuplicates = function(nums) {
            for (let i=0, len=nums.length; i<len; i++) {
                for (let j=i+1; j<len; j++) {
                    if (nums[i] !== nums[j]) break;
                    nums.splice(j, 1);
                    j--;
                    len--;
                }
            }
            return nums.length;
        };
        ```

- 第二思路:
  - 思路:
  - 复杂度分析:
    - 时间:
    - 空间:
  - Leetcode 结果:
    - 执行用时: ms, 在所有 JavaScript 提交中击败了  %的用户
    - 内存消耗: MB, 在所有 JavaScript 提交中击败  %的用户
  - 实现:

        ``` js
        ```

### 80.删除排序数组中的重复项 II

- 刷题进度:
  - [ ] xxx
  - [ ] xxx
  - [ ] xxx
- 难度:
- 题意解析:
- 输入处理:
- 初始思路:
  - 思路:
  - 复杂度分析:
    - 时间:
    - 空间:
  - Leetcode 结果:
    - 执行用时:  ms, 在所有 JavaScript 提交中击败了  %的用户
    - 内存消耗:  MB, 在所有 JavaScript 提交中击败  %的用户
  - 实现:

        ``` js

        ```

- 第二思路:
  - 思路:
  - 复杂度分析:
    - 时间:
    - 空间:
  - Leetcode 结果:
    - 执行用时: ms, 在所有 JavaScript 提交中击败了  %的用户
    - 内存消耗: MB, 在所有 JavaScript 提交中击败  %的用户
  - 实现:

        ``` js
        ```    

### 832. 翻转图像

- 刷题进度:
  - [x] 对撞指针翻转+三目反转.
  - [ ] xxx
  - [ ] xxx
- 难度: easy.
- 题意解析: 给定二维数组作为矩阵，先翻转每行，再反转数值.
- 输入处理: 无.
- 初始思路: 对撞指针翻转+三目反转
  - 思路: 边翻转行边翻转值.
  - 复杂度分析:
    - 时间: O(n). 全部遍历过一次顺便完成替换.
    - 空间: O(1). 全程原地.
  - Leetcode 结果:
    - 执行用时: 80 ms, 在所有 JavaScript 提交中击败了 63 %的用户
    - 内存消耗: 36.8 MB, 在所有 JavaScript 提交中击败 5.3 %的用户
  - 实现:

        ``` js
        var flipAndInvertImage = function(A) {
            for (let i=0, lenA=A.length; i<lenA; i++) {
                let [a, b] = [0, A[i].length-1];
                while (a <= b) {
                    [A[i][a], A[i][b]] = [A[i][b]?0:1, A[i][a]?0:1];
                    [a, b] = [a+1, b-1];
                }
            }
            return A;
        };
        ```

- 第二思路:
  - 思路:
  - 复杂度分析:
    - 时间:
    - 空间:
  - Leetcode 结果:
    - 执行用时: ms, 在所有 JavaScript 提交中击败了  %的用户
    - 内存消耗: MB, 在所有 JavaScript 提交中击败  %的用户
  - 实现:

        ``` js
        ```

### 1051. 高度检查器

- 刷题进度:
  - [x] 排序后对比.
  - [ ] xxx
  - [ ] xxx
- 难度: easy.
- 题意解析: 给定数组，要求获得"能够使其以非递减方式排列"的必要移动数量.
- 输入处理: 无.
- 初始思路: 排序后对比.
  - 思路: 记得数组的 sort 会修改原数组，需要提前复制.
  - 复杂度分析:
    - 时间: O(nlogn). 复制循环 O(n) + 排序O(nlogn) + 对比循环 O(n).
    - 空间: O(n). 复制的数组长度.
  - Leetcode 结果:
    - 执行用时: 60 ms, 在所有 JavaScript 提交中击败了 97.8 %的用户
    - 内存消耗: 35 MB, 在所有 JavaScript 提交中击败 100 %的用户
  - 实现:

        ``` js
        var heightChecker = function(heights) {
            let res = 0;
            let arr = [...heights];
            heights.sort((a, b) => a-b);
            for (let i=0, len=heights.length; i<len; i++) {
                if (heights[i] !== arr[i]) res++;
            }
            return res;
        };
        ```

- 第二思路:
  - 思路:
  - 复杂度分析:
    - 时间:
    - 空间:
  - Leetcode 结果:
    - 执行用时: ms, 在所有 JavaScript 提交中击败了  %的用户
    - 内存消耗: MB, 在所有 JavaScript 提交中击败  %的用户
  - 实现:

        ``` js
        ```

### 1266. 访问所有点的最小时间

- 刷题进度:
  - [ ] 计算点间距.
  - [ ] xxx
  - [ ] xxx
- 难度: easy.
- 题意解析: 从数组第一个点依次走到最后一个.
- 输入处理: 无.
- 初始思路: 计算点间距.
  - 思路: 计算每段点间距，即x,y互减后的最大值
  - 复杂度分析:
    - 时间: O(n).
    - 空间: O(1).
  - Leetcode 结果:
    - 执行用时: 68 ms, 在所有 JavaScript 提交中击败了 76 %的用户
    - 内存消耗: 34.6 MB, 在所有 JavaScript 提交中击败 100 %的用户
  - 实现:

        ``` js
        var minTimeToVisitAllPoints = function(points) {
            let totalTime = 0;
            for (let i=0, len=points.length-1; i<len; i++) {
                let absX = Math.abs(points[i][0] - points[i+1][0]);
                let absY = Math.abs(points[i][1] - points[i+1][1]);
                totalTime += Math.max(absX, absY);
            }
            return totalTime;
        };
        ```

- 第二思路:
  - 思路:
  - 复杂度分析:
    - 时间:
    - 空间:
  - Leetcode 结果:
    - 执行用时: ms, 在所有 JavaScript 提交中击败了  %的用户
    - 内存消耗: MB, 在所有 JavaScript 提交中击败  %的用户
  - 实现:

        ``` js
        ```

### 724. 寻找数组的中心索引

- 刷题进度:
  - [ ] xxx
  - [ ] xxx
  - [ ] xxx
- 难度:
- 题意解析:
- 输入处理:
- 初始思路:
  - 思路:
  - 复杂度分析:
    - 时间:
    - 空间:
  - Leetcode 结果:
    - 执行用时: ms, 在所有 JavaScript 提交中击败了  %的用户
    - 内存消耗: MB, 在所有 JavaScript 提交中击败  %的用户
  - 实现:

        ``` js
        var pivotIndex = function(nums) {
            if (nums.length < 1) return -1;
            for (let i=0, len=nums.length; i<len; i++) {
                let [left, right] = [0, 0];
                for (let j=0; j<i; j++) left+=nums[j];
                for (let j=i+1; j<len; j++) right+=nums[j];
                if (left === right) return i;
            }
            return -1;
        };
        ```

- 第二思路:
  - 思路:
  - 复杂度分析:
    - 时间:
    - 空间:
  - Leetcode 结果:
    - 执行用时: ms, 在所有 JavaScript 提交中击败了  %的用户
    - 内存消耗: MB, 在所有 JavaScript 提交中击败  %的用户
  - 实现:

        ``` js
        ```

### 747. 至少是其他数字两倍的最大数

- 刷题进度:
  - [ ] 获取最大数和次大数比较.
  - [ ] xxx
  - [ ] xxx
- 难度:
- 题意解析:
- 输入处理:
- 初始思路:
  - 思路:
  - 复杂度分析:
    - 时间:
    - 空间:
  - Leetcode 结果:
    - 执行用时: ms, 在所有 JavaScript 提交中击败了  %的用户
    - 内存消耗: MB, 在所有 JavaScript 提交中击败  %的用户
  - 实现:

        ``` js
        var dominantIndex = function(nums) {
            let max = 0;
            let maxIdx = -1;
            for (let i=0, len=nums.length; i<len; i++) {
                if (nums[i] > max) [max, maxIdx] = [nums[i], i];
            }
            nums.splice(maxIdx, 1);
            let sndMax = Math.max.apply(null, nums);
            return sndMax * 2 <= max ? maxIdx : -1;
        };
        ```

- 第二思路:
  - 思路:
  - 复杂度分析:
    - 时间:
    - 空间:
  - Leetcode 结果:
    - 执行用时: ms, 在所有 JavaScript 提交中击败了  %的用户
    - 内存消耗: MB, 在所有 JavaScript 提交中击败  %的用户
  - 实现:

        ``` js
        ```

### 66. 加一

- 刷题进度:
  - [x] 倒转推进 + 进阶检测
  - [ ] xxx
  - [ ] xxx
- 难度:
- 题意解析:
- 输入处理:
- 初始思路:
  - 思路:
  - 复杂度分析:
    - 时间:
    - 空间:
  - Leetcode 结果:
    - 执行用时: ms, 在所有 JavaScript 提交中击败了  %的用户
    - 内存消耗: MB, 在所有 JavaScript 提交中击败  %的用户
  - 实现:

        ``` js
        var plusOne = function(digits) {
            let carry = 0;
            let len = digits.length;
            for (let i=len-1; i>=0; i--) {
                let tmp = digits[i];
                if (i === len-1) digits[i] += 1;
                digits[i] += carry;
                carry = digits[i] > 9 ? 1: 0;
                digits[i] %= 10;
                if (tmp === digits[i]) break;
            }
            if (carry) digits.unshift(1);
            return digits;
        };
        ```

- 第二思路:
  - 思路:
  - 复杂度分析:
    - 时间:
    - 空间:
  - Leetcode 结果:
    - 执行用时: ms, 在所有 JavaScript 提交中击败了  %的用户
    - 内存消耗: MB, 在所有 JavaScript 提交中击败  %的用户
  - 实现:

        ``` js
        ```

### 498. 对角线遍历

- 刷题进度:
  - [ ] 一次遍历.
  - [ ] xxx
  - [ ] xxx
- 难度: medium.
- 题意解析:
- 输入处理:
- 初始思路: 一次遍历.
  - 思路: 观察规律，总来回数为(m+n+1)，偶数回y大x小，奇数回y小x大.
  - 复杂度分析:
    - 时间:
    - 空间:
  - Leetcode 结果:
    - 执行用时: ms, 在所有 JavaScript 提交中击败了  %的用户
    - 内存消耗: MB, 在所有 JavaScript 提交中击败  %的用户
  - 实现:

        ``` js
        var findDiagonalOrder = function(matrix) {
            if (matrix.length === 0) return [];
            let [n, m] = [matrix.length, matrix[0].length];
            let res = [];
            for (let i=0, len=n+m-1; i<len; i++) {
                if (i % 2 === 0) { // 反向, x 小 y 大
                    let [x, y] = [0, i];
                    while (y >= n) [x, y] = [x+1, y-1];
                    while (y >= 0 && x < m) {
                        res.push(matrix[y][x]);
                        [x, y] = [x+1, y-1];
                    }
                } else { // 正向，x 大 y 小
                    let [x, y] = [i, 0];
                    while (x >= m) [x, y] = [x-1, y+1];
                    while (x >= 0 && y < n) {
                        res.push(matrix[y][x]);
                        [x, y] = [x-1, y+1];
                    }
                }
            }
            return res;
        };
        ```

- 第二思路:
  - 思路:
  - 复杂度分析:
    - 时间:
    - 空间:
  - Leetcode 结果:
    - 执行用时: ms, 在所有 JavaScript 提交中击败了  %的用户
    - 内存消耗: MB, 在所有 JavaScript 提交中击败  %的用户
  - 实现:

        ``` js
        ```

### 54. 螺旋矩阵

- 刷题进度:
  - [x] 暴力法：xy轴双指针对撞
  - [ ] xxx
  - [ ] xxx
- 难度: medium.
- 题意解析:
- 输入处理: 由于需要获取 x 轴长度 m, 故首先要处理 matrix 为 0 的情况.
- 初始思路: 暴力法：xy轴双指针对撞
  - 思路: 一轮四个方向都跑一次，如果未全部撞针则继续下拉否则停止.
  - 复杂度分析:
    - 时间: O(n). 单轮遍历.
    - 空间: O(1). 常量级额外空间.
  - Leetcode 结果:
    - 执行用时: ms, 在所有 JavaScript 提交中击败了  %的用户
    - 内存消耗: MB, 在所有 JavaScript 提交中击败  %的用户
  - 实现:

        ``` js
        var spiralOrder = function(matrix) {
            if (matrix.length === 0) return [];
            let res = [];
            let [n, m] = [matrix.length, matrix[0].length];
            let [minY, maxY, minX, maxX] = [0, n-1, 0, m-1];
            while (true) {
                for (let i=minX; i<=maxX; i++) res.push(matrix[minY][i]);
                if (++minY > maxY) break;
                for (let i=minY; i<=maxY; i++) res.push(matrix[i][maxX]);
                if (--maxX < minX) break;
                for (let i=maxX; i>=minX; i--) res.push(matrix[maxY][i]);
                if (--maxY < minY) break;
                for (let i=maxY; i>=minY; i--) res.push(matrix[i][minX]);
                if (++minX > maxX) break;
            }
            return res;
        };
        ```

- 第二思路:
  - 思路:
  - 复杂度分析:
    - 时间:
    - 空间:
  - Leetcode 结果:
    - 执行用时: ms, 在所有 JavaScript 提交中击败了  %的用户
    - 内存消耗: MB, 在所有 JavaScript 提交中击败  %的用户
  - 实现:

        ``` js
        ```

### 118. 杨辉三角

- 刷题进度:
  - [ ] 双遍历构建法.
  - [ ] xxx
  - [ ] xxx
- 难度: easy.
- 题意解析:
- 输入处理:
- 初始思路: 双遍历构建法.
  - 思路: 第一层为层数遍历，第二层遍历用于每层构建.
  - 复杂度分析:
    - 时间: O(n^2). n 为层数. 第二层遍历长度从 1 -> n ， 共 n(n+1)/2 次, 故 O(n^2).
    - 空间: O(1). 常量级额外空间.
  - Leetcode 结果:
    - 执行用时: ms, 在所有 JavaScript 提交中击败了  %的用户
    - 内存消耗: MB, 在所有 JavaScript 提交中击败  %的用户
  - 实现:

        ``` js
        var generate = function(numRows) {
            let res = Array.from({length: numRows}, () => [1]);
            for (let i=0; i<numRows; i++) {
                for (let j=1; j<=i; j++) {
                    if (j<i) res[i][j] = res[i-1][j-1] + res[i-1][j];
                    else if (j === i) res[i][j] = 1;
                }
            }
            return res;
        };
        ```

- 第二思路:
  - 思路:
  - 复杂度分析:
    - 时间:
    - 空间:
  - Leetcode 结果:
    - 执行用时: ms, 在所有 JavaScript 提交中击败了  %的用户
    - 内存消耗: MB, 在所有 JavaScript 提交中击败  %的用户
  - 实现:

        ``` js
        ```

### 344. 反转字符串

- 刷题进度:
  - [x] 双指针.
  - [x] 递归.
  - [ ] xxx
- 难度: easy.
- 题意解析:
- 输入处理:
- 初始思路: 双指针.
  - 思路: 如上.
  - 复杂度分析:
    - 时间: O(n).
    - 空间: O(1).
  - Leetcode 结果:
    - 执行用时: ms, 在所有 JavaScript 提交中击败了  %的用户
    - 内存消耗: MB, 在所有 JavaScript 提交中击败  %的用户
  - 实现:

        ``` js
        var reverseString = function(s) {
            if (s.length === 0) return s;
            let [i, j] = [0, s.length-1];
            while (i < j) {
                [s[i], s[j], i, j] = [s[j], s[i], i+1, j-1];
            }
        };
        ```

- 第二思路: 递归.
  - 思路:
  - 复杂度分析:
    - 时间: O(n).
    - 空间: O(n). 递归空间.
  - Leetcode 结果:
    - 执行用时: ms, 在所有 JavaScript 提交中击败了  %的用户
    - 内存消耗: MB, 在所有 JavaScript 提交中击败  %的用户
  - 实现:

        ``` js
        var reverseString = function(s) {
            helper(0, s.length-1, s);
        };

        function helper (i, j, arr) {
            if (i < j) {
                [arr[i], arr[j]] = [arr[j], arr[i]];
                helper(++i, --j, arr);
            }
        }
        ```

### 561. 数组拆分 I

- 刷题进度:
  - [x] 排序 + 取偶数下标.
  - [ ] xxx
  - [ ] xxx
- 难度: easy.
- 题意解析:
- 输入处理:
- 初始思路: 排序 + 取偶数下标.
  - 思路:
  - 复杂度分析:
    - 时间: O(n). 砍半遍历推进共 O(n/2).
    - 空间: O(1). 常量级额外空间.
  - Leetcode 结果:
    - 执行用时: ms, 在所有 JavaScript 提交中击败了  %的用户
    - 内存消耗: MB, 在所有 JavaScript 提交中击败  %的用户
  - 实现:

        ``` js
        var arrayPairSum = function(nums) {
            nums.sort((a, b) => a-b);
            let res = 0;
            for (let i=0, len=nums.length; i<len; i+=2) {
                res += nums[i];
            }
            return res;
        };
        ```

- 第二思路:
  - 思路:
  - 复杂度分析:
    - 时间:
    - 空间:
  - Leetcode 结果:
    - 执行用时: ms, 在所有 JavaScript 提交中击败了  %的用户
    - 内存消耗: MB, 在所有 JavaScript 提交中击败  %的用户
  - 实现:

        ``` js
        ```

### 485. 最大连续1的个数

- 刷题进度:
  - [x] 双指针法.
  - [ ] xxx
  - [ ] xxx
- 难度: easy.
- 题意解析:
- 输入处理:
- 初始思路: 双指针法.
  - 思路: i 指针逐位推进，如果 i 位为 1，则开启 j指针的推进直到 非1 或 到达数组末尾.
  - 复杂度分析:
    - 时间: O(n^2).
    - 空间: O(1). 常量级额外空间.
  - Leetcode 结果:
    - 执行用时: ms, 在所有 JavaScript 提交中击败了  %的用户
    - 内存消耗: MB, 在所有 JavaScript 提交中击败  %的用户
  - 实现:

        ``` js
        var findMaxConsecutiveOnes = function(nums) {
            let max = 0;
            for (let i=0, len=nums.length; i<len; i++) {
                if (nums[i] === 1) {
                    let count = 1;
                    for (let j=i+1; j<len; j++) {
                        if (nums[j] === 1) count++;
                        else break;
                    }
                    max = Math.max(max, count);
                }
            }
            return max;  
        };
        ```

- 第二思路:
  - 思路:
  - 复杂度分析:
    - 时间:
    - 空间:
  - Leetcode 结果:
    - 执行用时: ms, 在所有 JavaScript 提交中击败了  %的用户
    - 内存消耗: MB, 在所有 JavaScript 提交中击败  %的用户
  - 实现:

        ``` js
        ```

### 209. 长度最小的子数组

- 刷题进度:
  - [x] 快慢指针两次遍历.
  - [ ] xxx
  - [ ] xxx
- 难度: easy.
- 题意解析:
- 输入处理:
- 初始思路: 快慢指针两次遍历.
  - 思路:
  - 复杂度分析:
    - 时间: O(n^2). 两次遍历.
    - 空间: O(1).
  - Leetcode 结果:
    - 执行用时: ms, 在所有 JavaScript 提交中击败了  %的用户
    - 内存消耗: MB, 在所有 JavaScript 提交中击败  %的用户
  - 实现:

        ``` js
        var minSubArrayLen = function(s, nums) {
            let res = Number.POSITIVE_INFINITY;
            for (let i=0, len = nums.length; i<len; i++) {
                if (nums[i] >= s) {
                    return 1; // 特殊化：一个数就满足，必须最短，加速方法.
                } else {
                    let count = 0;
                    let sum = 0;
                    for (let j=i; j<len; j++) {
                        sum += nums[j];
                        count++;
                        if (sum >= s) res = Math.min(res, count);
                    }
                }
            }
            return res === Number.POSITIVE_INFINITY ? 0 : res;
        };
        ```

- 第二思路:
  - 思路:
  - 复杂度分析:
    - 时间:
    - 空间:
  - Leetcode 结果:
    - 执行用时: ms, 在所有 JavaScript 提交中击败了  %的用户
    - 内存消耗: MB, 在所有 JavaScript 提交中击败  %的用户
  - 实现:

        ``` js
        ```

### 189 旋转数组

- 刷题进度:
  - [x] 弹出+头插.
  - [ ] xxx
  - [ ] xxx
- 难度: easy.
- 题意解析:
- 输入处理: k有可能极大，所以先用数组长度 len 将 k 取余.
- 初始思路: 弹出+头插.
  - 思路: 一次循环.
  - 复杂度分析:
    - 时间: O(k * n). 循环处理后的k次O(k)， 弹出和头插复杂度均为 O(n).
    - 空间: O(1).
  - Leetcode 结果:
    - 执行用时: ms, 在所有 JavaScript 提交中击败了  %的用户
    - 内存消耗: MB, 在所有 JavaScript 提交中击败  %的用户
  - 实现:

        ``` js
        var rotate = function(nums, k) {
            if (nums.length === 0) return nums;
            let len = nums.length;
            k = k % len;
            for (let i=0; i<k; i++) {
                nums.unshift(nums.pop());
            }
            return nums;
        };
        ```

- 第二思路:
  - 思路:
  - 复杂度分析:
    - 时间:
    - 空间:
  - Leetcode 结果:
    - 执行用时: ms, 在所有 JavaScript 提交中击败了  %的用户
    - 内存消耗: MB, 在所有 JavaScript 提交中击败  %的用户
  - 实现:

        ``` js
        ```

### 119. 杨辉三角 II

- 刷题进度:
  - [x] 全部生成.
  - [ ] TODO：生成器.
  - [ ] xxx
- 难度: easy.
- 题意解析:
- 输入处理:
- 初始思路: 全部生成.
  - 思路: 全部生成再取对应下标的行数.
  - 复杂度分析:
    - 时间: O(n^2). 生成 1->n 的杨辉三角，故耗时为 O(n^2).
    - 空间: O(n^2). 与同等级别的额外空间.
  - Leetcode 结果:
    - 执行用时: ms, 在所有 JavaScript 提交中击败了  %的用户
    - 内存消耗: MB, 在所有 JavaScript 提交中击败  %的用户
  - 实现:

        ``` js
        var getRow = function(rowIndex) {
            let arr = Array.from({length: rowIndex+1}, ()=>[1]);
            for (let i=1; i<=rowIndex; i++) {
                let j = 1;
                while (arr[i].length !== i+1) {
                    arr[i][j] = arr[i-1][j] ? arr[i-1][j-1] + arr[i-1][j] : 1;
                    j++;
                }
            }
            return arr[rowIndex];
        };
        ```

- 第二思路:
  - 思路:
  - 复杂度分析:
    - 时间:
    - 空间:
  - Leetcode 结果:
    - 执行用时: ms, 在所有 JavaScript 提交中击败了  %的用户
    - 内存消耗: MB, 在所有 JavaScript 提交中击败  %的用户
  - 实现:

        ``` js
        ```

### 217. 存在重复

- 刷题进度:
  - [x] Set.
  - [ ] xxx
  - [ ] xxx
- 难度: easy.
- 题意解析:
- 输入处理:
- 初始思路: Set.
  - 思路:
  - 复杂度分析:
    - 时间: O(1).
    - 空间: O(n). Set 空间.
  - Leetcode 结果:
    - 执行用时: ms, 在所有 JavaScript 提交中击败了  %的用户
    - 内存消耗: MB, 在所有 JavaScript 提交中击败  %的用户
  - 实现:

        ``` js
        var containsDuplicate = function(nums) {
            let set = new Set(nums);
            return set.size !== nums.length;
        };
        ```

- 第二思路:
  - 思路:
  - 复杂度分析:
    - 时间:
    - 空间:
  - Leetcode 结果:
    - 执行用时: ms, 在所有 JavaScript 提交中击败了  %的用户
    - 内存消耗: MB, 在所有 JavaScript 提交中击败  %的用户
  - 实现:

        ``` js
        ```

### 136. 只出现一次的数字

- 刷题进度:
  - [x] Map.
  - [x] 双重遍历原地去重法.
  - [x] 排序.
  - [x] 异或.
- 难度: easy.
- 题意解析:
- 输入处理:
- 初始思路: Map.
  - 思路:
  - 复杂度分析:
    - 时间: O(n).
    - 空间: O(n). Map空间.
  - Leetcode 结果:
    - 执行用时: ms, 在所有 JavaScript 提交中击败了  %的用户
    - 内存消耗: MB, 在所有 JavaScript 提交中击败  %的用户
  - 实现:

        ``` js
        var singleNumber = function(nums) {
            let map = new Map();
            for (let i=0, len=nums.length; i<len; i++) {
                map.set(nums[i], map.has(nums[i]) ? map.get(nums[i])+1 : 1);
            }
            for (let [k,v] of map) {
                if (v===1) return k;
            }
        };
        ```

- 第二思路: 双重遍历原地去重法.
  - 思路:
  - 复杂度分析:
    - 时间: O(n^2).
    - 空间: O(1).
  - Leetcode 结果:
    - 执行用时: ms, 在所有 JavaScript 提交中击败了  %的用户
    - 内存消耗: MB, 在所有 JavaScript 提交中击败  %的用户
  - 实现:

        ``` js
        var singleNumber = function(nums) {
            for (let i=0, len=nums.length; i<len; i++) {
                for (let j=i+1; j<len; j++) {
                    if (nums[i] === nums[j]) {
                        nums.splice(j, 1);
                        nums.splice(i, 1);
                        i--;
                        len-=2;
                        break;
                    }
                }
            }
            return nums[0];
        };
        ```

- 第三思路: 排序.
  - 思路:
  - 复杂度分析:
    - 时间: O(nlogn). 排序耗时.
    - 空间: O(1).
  - Leetcode 结果:
    - 执行用时: ms, 在所有 JavaScript 提交中击败了  %的用户
    - 内存消耗: MB, 在所有 JavaScript 提交中击败  %的用户
  - 实现:

        ``` js
        var singleNumber = function(nums) {
            nums.sort((a,b) => a-b);
            for (let i=0, len=nums.length; i<len; i++) {
                if (typeof nums[i+1] !== 'undefined') {
                    if (nums[i] === nums[i+1]) {
                        i++;
                    } else {
                        return nums[i]
                    }
                } else {
                    return nums[i];
                }
            }
        };
        ```

- 第四思路: 异或.
  - 思路: 相同数都被异或为 0，剩下的就是结果.
  - 复杂度分析:
    - 时间: O(n).
    - 空间: O(1).
  - Leetcode 结果:
    - 执行用时: 64 ms, 在所有 JavaScript 提交中击败了 90 %的用户
    - 内存消耗: 35.6 MB, 在所有 JavaScript 提交中击败 51.7 %的用户
  - 实现:

        ``` js
        var singleNumber = function(nums) {
            let ans = nums[0];
            for (let i=1, len=nums.length; i<len; i++) {
                ans ^= nums[i];
            }
            return ans;
        };
        ```

### 349. 两个数组的交集

- 刷题进度:
  - [x] 双 Set.
  - [ ] xxx
  - [ ] xxx
- 难度: easy.
- 题意解析:
- 输入处理:
- 初始思路: 双 Set.
  - 思路:
  - 复杂度分析:
    - 时间:
    - 空间:
  - Leetcode 结果:
    - 执行用时: ms, 在所有 JavaScript 提交中击败了  %的用户
    - 内存消耗: MB, 在所有 JavaScript 提交中击败  %的用户
  - 实现:

        ``` js
        var intersection = function(nums1, nums2) {
            let [set1, set2] = [new Set(nums1), new Set(nums2)];
            let res = [];
            for (let s of set2) {
                if (set1.has(s)) res.push(s);
            }
            return res;
        };
        ```

- 第二思路:
  - 思路:
  - 复杂度分析:
    - 时间:
    - 空间:
  - Leetcode 结果:
    - 执行用时: ms, 在所有 JavaScript 提交中击败了  %的用户
    - 内存消耗: MB, 在所有 JavaScript 提交中击败  %的用户
  - 实现:

        ``` js
        ```

### 350. 两个数组的交集 II

- 刷题进度:
  - [ ] 双 map 暴力.
  - [ ] xxx
  - [ ] xxx
- 难度:
- 题意解析:
- 输入处理:
- 初始思路:
  - 思路:
  - 复杂度分析:
    - 时间:
    - 空间:
  - Leetcode 结果:
    - 执行用时: ms, 在所有 JavaScript 提交中击败了  %的用户
    - 内存消耗: MB, 在所有 JavaScript 提交中击败  %的用户
  - 实现:

        ``` js
        var intersect = function(nums1, nums2) {
            let [map1, map2] = [new Map(), new Map()];
            for (let i=0, len=nums1.length; i<len; i++) {
                map1.set(nums1[i], map1.has(nums1[i]) ? map1.get(nums1[i])+1 : 1);
            }
            for (let i=0, len=nums2.length; i<len; i++) {
                map2.set(nums2[i], map2.has(nums2[i]) ? map2.get(nums2[i])+1 : 1);
            }
            let res = [];
            for (let [k1, v1] of map1) {
                for (let [k2, v2] of map2) {
                    if (k1 === k2) {
                        let min = Math.min(v1, v2);
                        for (let i=0; i<min; i++) res.push(k1);
                    }
                }
            }
            return res;
        };
        ```

- 第二思路:
  - 思路:
  - 复杂度分析:
    - 时间:
    - 空间:
  - Leetcode 结果:
    - 执行用时: ms, 在所有 JavaScript 提交中击败了  %的用户
    - 内存消耗: MB, 在所有 JavaScript 提交中击败  %的用户
  - 实现:

        ``` js
        ```

### 15.三数之和

- 刷题进度:
  - [x] for + twoSum.
  - [ ] xxx
  - [ ] xxx
- 难度: medium.
- 题意解析:
- 输入处理:
- 初始思路: for + twoSum.
  - 思路:
  - 复杂度分析:
    - 时间: O(n^2). 排序 O(nLogn) + for(n) * twoSum O(n).
    - 空间:
  - Leetcode 结果:
    - 执行用时: 628 ms, 在所有 JavaScript 提交中击败了 5 %的用户
    - 内存消耗: 56.7 MB, 在所有 JavaScript 提交中击败 12.3 %的用户
  - 实现:

        ``` js
        var threeSum = function(nums) {
            let len = nums.length;
            nums.sort((a,b) => a-b);
            if (nums.length < 3) return [];
            if (nums[0] > 0 && nums[len-1] > 0) return [];
            if (nums[0] < 0 && nums[len-1] < 0) return [];
            if (nums[0] === 0 && nums[len-1] === 0) return [[0,0,0]];
            let map = new Map();
            for (let i=0, limitLen=len-2; i<limitLen; i++) {
                let tmp = nums[i];
                let matchArr = twoSum (nums.slice(i+1), -tmp);
                matchArr.forEach(item => {
                    item.unshift(tmp);
                    map.set(item.join(''), item);
                });
            }
            return Array.from(map.values());
        };

        function twoSum (arr, target) {
            let map = new Map();
            let res = [];
            for (let i=0, len=arr.length; i<len; i++) {
                if (map.has(target - arr[i])) {
                    res.push([target - arr[i], arr[i]]);
                } else {
                    map.set(arr[i], i);
                }
            }
            return res;
        }
        ```

- 第二思路:
  - 思路:
  - 复杂度分析:
    - 时间:
    - 空间:
  - Leetcode 结果:
    - 执行用时: ms, 在所有 JavaScript 提交中击败了  %的用户
    - 内存消耗: MB, 在所有 JavaScript 提交中击败  %的用户
  - 实现:

        ``` js
        ```

### 18. 四数之和

- 刷题进度:
  - [x] 双for + twosum.（合并写法）
  - [ ] xxx
  - [ ] xxx
- 难度: medium.
- 题意解析:
- 输入处理:
- 初始思路: 双for + twosum.（合并写法）
  - 思路:
  - 复杂度分析:
    - 时间: O(n^3). 排序 O(nLogn) + 双 for(n^2) * twoSum O(n).
    - 空间:
  - Leetcode 结果:
    - 执行用时: ms, 在所有 JavaScript 提交中击败了  %的用户
    - 内存消耗: MB, 在所有 JavaScript 提交中击败  %的用户
  - 实现:

        ``` js
        var fourSum = function(nums, target) {
            let len = nums.length;
            if (len < 4) return [];
            nums = nums.sort((a, b) => a-b);
            if (nums[0]>0 && nums[len-1]>0) return [];
            if (nums[0]<0 && nums[len-1]<0) return [];
            if (nums[0] === nums[len-1]) return target === 0 ? [[0, 0, 0, 0]] : [];
            let map = new Map();
            for (let m=0, mLen=len-3; m<mLen; m++) {
                for (let i=m+1, iLen=len-2; i<iLen; i++) {
                    let tmp = target - nums[m] - nums[i];
                    let tmpMap = new Map();
                    let matchArr = [];
                    for (let j=i+1; j<len; j++) {
                        if (tmpMap.has(nums[j])) {
                            matchArr.push([tmp-nums[j], nums[j]]);
                        } else {
                            tmpMap.set(tmp-nums[j], j);
                        }
                    }
                    matchArr.forEach(item => {
                        item.unshift(nums[i]);
                        item.unshift(nums[m]);
                        map.set(item.join(''), item);
                    });
                }
            }
            return Array.from(map.values());
        };
        ```

- 第二思路:
  - 思路:
  - 复杂度分析:
    - 时间:
    - 空间:
  - Leetcode 结果:
    - 执行用时: ms, 在所有 JavaScript 提交中击败了  %的用户
    - 内存消耗: MB, 在所有 JavaScript 提交中击败  %的用户
  - 实现:

        ``` js
        ```

### 454.四数相加 II

- 刷题进度:
  - [x] 砍半放map，剩余双遍历对比.
  - [ ] xxx
  - [ ] xxx
- 难度: medium.
- 题意解析:
- 输入处理:
- 初始思路: 砍半放map，剩余双遍历对比.
  - 思路:
  - 复杂度分析:
    - 时间: O(n^2). 砍半放入 Map O(n^2) + 剩余双遍历 O(n^2).
    - 空间: O(n^2). map 最多存在 n^2 个键值对.
  - Leetcode 结果:
    - 执行用时: ms, 在所有 JavaScript 提交中击败了  %的用户
    - 内存消耗: MB, 在所有 JavaScript 提交中击败  %的用户
  - 实现:

        ``` js
        var fourSumCount = function(A, B, C, D) {
            let len = A.length;
            let count = 0;
            let map = new Map();
            for (let i=0; i<len; i++) {
                for (let j=0; j<len; j++) {
                    let tmp = A[i] + B[j];
                    map.set(tmp, map.has(tmp) ? map.get(tmp)+1: 1);
                }
            }
            for (let i=0; i<len; i++) {
                for (let j=0; j<len; j++) {
                    let tmp = 0 - C[i] - D[j];
                    if (map.has(tmp)) count+=map.get(tmp);
                }
            }
            return count;
        };
        ```

- 第二思路:
  - 思路:
  - 复杂度分析:
    - 时间:
    - 空间:
  - Leetcode 结果:
    - 执行用时: ms, 在所有 JavaScript 提交中击败了  %的用户
    - 内存消耗: MB, 在所有 JavaScript 提交中击败  %的用户
  - 实现:

        ``` js
        ```

### 219 存在重复元素 II

- 刷题进度:
  - [x] 双指针推进.
  - [ ] xxx
  - [ ] xxx
- 难度: easy.
- 题意解析:
- 输入处理:
- 初始思路: 双指针推进.
  - 思路:
  - 复杂度分析:
    - 时间: O(n*k).
    - 空间: O(1).
  - Leetcode 结果:
    - 执行用时: ms, 在所有 JavaScript 提交中击败了  %的用户
    - 内存消耗: MB, 在所有 JavaScript 提交中击败  %的用户
  - 实现:

        ``` js
        var containsNearbyDuplicate = function(nums, k) {
            let len = nums.length;
            for (let m=k; m>0; m--) {
                for (let i=0; i<len-m; i++) {
                    let j = i+m;
                    if (nums[i] === nums[j]) return true;
                }
            }
            return false;
        };
        ```

- 第二思路:
  - 思路:
  - 复杂度分析:
    - 时间:
    - 空间:
  - Leetcode 结果:
    - 执行用时: ms, 在所有 JavaScript 提交中击败了  %的用户
    - 内存消耗: MB, 在所有 JavaScript 提交中击败  %的用户
  - 实现:

        ``` js
        ```

### 220 存在重复元素 III

- 刷题进度:
  - [x] 双指针推进.
  - [ ] xxx
  - [ ] xxx
- 难度: medium.
- 题意解析:
- 输入处理:
- 初始思路: 双指针推进.
  - 思路:
  - 复杂度分析:
    - 时间: O(n*k).
    - 空间: O(1).
  - Leetcode 结果:
    - 执行用时: ms, 在所有 JavaScript 提交中击败了  %的用户
    - 内存消耗: MB, 在所有 JavaScript 提交中击败  %的用户
  - 实现:

        ``` js
        var containsNearbyAlmostDuplicate = function(nums, k, t) {
            for (let m=k; m>0; m--) {
                for (let i=0, len=nums.length; i<len-m; i++) {
                    let j = i + m;
                    if (Math.abs(nums[i] - nums[j]) <= t) return true;
                }
            }
            return false;
        };
        ```

- 第二思路:
  - 思路:
  - 复杂度分析:
    - 时间:
    - 空间:
  - Leetcode 结果:
    - 执行用时: ms, 在所有 JavaScript 提交中击败了  %的用户
    - 内存消耗: MB, 在所有 JavaScript 提交中击败  %的用户
  - 实现:

        ``` js
        ```

### 36. 有效的数独

- 刷题进度:
  - [x] 暴力三段法
  - [ ] xxx
  - [ ] xxx
- 难度: medium.
- 题意解析:
- 输入处理:
- 初始思路: 暴力三段法.
  - 思路:
  - 复杂度分析:
    - 时间: O(n). 遍历耗时.
    - 空间: O(n). 额外Set 及 数组空间消耗.
  - Leetcode 结果:
    - 执行用时: 100 ms, 在所有 JavaScript 提交中击败了 30 %的用户
    - 内存消耗: 38 MB, 在所有 JavaScript 提交中击败 27 %的用户
  - 实现:

        ``` js
        var isValidSudoku = function(board) {
            // 1. 横排比较
            for (let i=0, len=board.length; i<len; i++) {
                let tmpArr = board[i].filter(x => x > 0);
                if (tmpArr.length !== new Set(tmpArr).size) return false;
            }
            // 2. 纵列比较
            for (let i=0, len=board.length; i<len; i++) {
                let tmpArr = [
                    board[0][i], board[1][i], board[2][i], board[3][i], board[4][i],
                    board[5][i],board[6][i], board[7][i], board[8][i]
                ];
                tmpArr = tmpArr.filter(x => x > 0);
                if (tmpArr.length !== new Set(tmpArr).size) return false;
            }
            // 3. 3*3内比较
            for (let i=0; i<3; i++) {
                for (let j=0; j<3; j++) {
                    let tmpArr = [
                        board[3*i][3*j], board[3*i][3*j+1], board[3*i][3*j+2],
                        board[3*i+1][3*j], board[3*i+1][3*j+1], board[3*i+1][3*j+2],
                        board[3*i+2][3*j], board[3*i+2][3*j+1], board[3*i+2][3*j+2]
                    ];
                    tmpArr = tmpArr.filter(x => x > 0);
                    if (tmpArr.length !== new Set(tmpArr).size) return false;
                }
            }

            return true;
        };
        ```

- 第二思路:
  - 思路:
  - 复杂度分析:
    - 时间:
    - 空间:
  - Leetcode 结果:
    - 执行用时: ms, 在所有 JavaScript 提交中击败了  %的用户
    - 内存消耗: MB, 在所有 JavaScript 提交中击败  %的用户
  - 实现:

        ``` js
        ```

### 48. 旋转图像

- 刷题进度:
  - [x] 对角线翻转+每行反转.
  - [ ] xxx
  - [ ] xxx
- 难度: medium.
- 题意解析:
- 输入处理: 空数组处理.
- 初始思路: 对角线翻转+每行反转.
  - 思路:
  - 复杂度分析:
    - 时间: O(n^2).
    - 空间: O(1).
  - Leetcode 结果:
    - 执行用时: ms, 在所有 JavaScript 提交中击败了  %的用户
    - 内存消耗: MB, 在所有 JavaScript 提交中击败  %的用户
  - 实现:

        ``` js
        var rotate = function(matrix) {
            if (matrix.length === 0 || matrix[0].length === 0) return [];
            let len = matrix.length;
            for (let i=0; i<len; i++) {
                for (let j=i; j<len; j++) {
                    if (i!==j) [matrix[i][j], matrix[j][i]] = [matrix[j][i], matrix[i][j]];
                }
            }
            for (let i=0; i<len; i++) {
                let [a, b] = [0, len-1];
                while (a < b) {
                    [matrix[i][a], matrix[i][b]] = [matrix[i][b], matrix[i][a]];
                    [a, b] = [a+1, b-1];
                }
            }
            return matrix;
        };
        ```

- 第二思路:
  - 思路:
  - 复杂度分析:
    - 时间:
    - 空间:
  - Leetcode 结果:
    - 执行用时: ms, 在所有 JavaScript 提交中击败了  %的用户
    - 内存消耗: MB, 在所有 JavaScript 提交中击败  %的用户
  - 实现:

        ``` js
        ```

### 54. 螺旋矩阵 II

- 刷题进度:
  - [x] 暴力法.
  - [ ] xxx
  - [ ] xxx
- 难度: medium.
- 题意解析:
- 输入处理:
- 初始思路: 暴力法.
  - 思路: 设定 xy 的上下限，暴力循环缩进.
  - 复杂度分析:
    - 时间: O(n^2).
    - 空间: O(1).
  - Leetcode 结果:
    - 执行用时: 68 ms, 在所有 JavaScript 提交中击败了 51 %的用户.
    - 内存消耗: 34.5 MB, 在所有 JavaScript 提交中击败 15 %的用户.
  - 实现:

        ``` js
        var generateMatrix = function(n) {
            let res = Array.from({ length: n }, () => []);
            let [minX, maxX, minY, maxY] = [0, n-1, 0, n-1];
            let count = 1;
            while (true) {
                for (let i=minX; i<=maxX; i++) [res[minY][i], count] = [count, count+1];
                minY++;
                if (minY > maxY) break;
                for (let i=minY; i<=maxY; i++) [res[i][maxX], count] = [count, count+1];
                maxX--;
                if (minX > maxX) break;
                for (let i=maxX; i>=minX; i--) [res[maxY][i], count] = [count, count+1];
                maxY--;
                if (minY > maxY) break;
                for (let i=maxY; i>=minY; i--) [res[i][minX], count] = [count, count+1];
                minX++;
                if (minX > maxX) break;
            }
            return res;
        };
        ```

- 第二思路:
  - 思路:
  - 复杂度分析:
    - 时间:
    - 空间:
  - Leetcode 结果:
    - 执行用时: ms, 在所有 JavaScript 提交中击败了  %的用户
    - 内存消耗: MB, 在所有 JavaScript 提交中击败  %的用户
  - 实现:

        ``` js
        ```

### 5295. 和为零的N个唯一整数

- 刷题进度:
  - [x] 判断奇偶 + 折半推进.
  - [ ] xxx
  - [ ] xxx
- 难度: easy.
- 题意解析:
- 输入处理:
- 初始思路: 判断奇偶 + 折半推进.
  - 思路:
  - 复杂度分析:
    - 时间: O(n).
    - 空间: O(1).
  - Leetcode 结果:
    - 执行用时: 56 ms, 在所有 JavaScript 提交中击败了 100 %的用户
    - 内存消耗: 35.2 MB, 在所有 JavaScript 提交中击败 100 %的用户
  - 实现:

        ``` js
        var sumZero = function(n) {
            if (n <= 0) return [];
            let res = [];
            if (n % 2 === 1) {
                res.push(0);
                n -= 1;
            }
            for (let i=1, len=n/2+1; i<len; i++) res.push(i, -i);
            return res;
        };
        ```

- 第二思路:
  - 思路:
  - 复杂度分析:
    - 时间:
    - 空间:
  - Leetcode 结果:
    - 执行用时: ms, 在所有 JavaScript 提交中击败了  %的用户
    - 内存消耗: MB, 在所有 JavaScript 提交中击败  %的用户
  - 实现:

        ``` js
        ```
