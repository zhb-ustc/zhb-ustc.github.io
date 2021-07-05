# 回溯算法

## 回溯算法与深度优先遍历
### 回溯算法  
**回溯法**采用试错的思想，它尝试分步的去解决一个问题。在分步解决问题的过程中，当它通过尝试发现现有的分步答案不能得到有效的正确的解答的时候，它将取消上一步甚至是上几步的计算，再通过其它的可能的分步解答再次尝试寻找问题的答案。回溯法通常用最简单的递归方法来实现，在反复重复上述的步骤后可能出现两种情况：
     * 找到一个可能存在的正确的答案；  
     * 在尝试了所有可能的分步方法后宣告该问题没有答案。  
### 深度优先遍历算法  
**深度优先搜索算法**（英语：Depth-First-Search，DFS）是一种用于遍历或搜索树或图的算法。这个算法会尽可能深的搜索树的分支。当结点v的所在边都己被探寻过，搜索将回溯到发现结点v的那条边的起始结点。这一过程一直进行到已发现从源结点可达的所有结点为止。如果还存在未被发现的结点，则选择其中一个作为源结点并重复以上过程，整个进程反复进行直到所有结点都被访问为止。
**回溯算法**强调了**深度优先遍历**思想的用途，用一个**不断变化**的变量，在尝试各种可能的过程中（也就是深度优先遍历）的过程中搜索并记录想要的结果，强调了**回退**操作对于搜索的合理性。

## 回溯算法与动态规划算法
### 共同点
都用于求解多阶段决策问题，多阶段决策问题即：    
* 求解一个问题氛围很多步骤（阶段）  
* 每一个步骤（阶段）可以有多种选择  
### 不同点  
* 动态规划只要求我们评估最优解是多少，最优解对应的具体解是什么并不要求，因此很适合应用于评估一个方案的效果  
* 回溯算法可以搜索的得到所有的方案（当然包括最优解），但是本质上是一种暴力搜索的遍历算法，时间复杂度是很高的  

### 回溯法框架（以全排列为例）
```markdown
void backtrack(const vector<int>& nums, vector<int> path, vector<bool> visited) {
    // 触发结束条件
    if (path.size() == nums.size()) {
        result.push_back(path);
        return;
    }
    // 遍历可选择的列表
    for (int i = 0; i < nums.size(); i++) {                        
        // 排除不合法选择
        if (visited[i] == true) {
            continue; 
        }                        
        // 做选择
        path.push_back(nums[i]);
        visited[i] = true;                       
        // 进入下一层决策树
        backtrack(nums, path, visited);                        
        // 取消选择
        path.pop_back();
        visited[i] = false;
    }
}

```
### 理解回溯  
在执行深度优先遍历的过程中，从较深的节点返回到较浅的结点的时候，需要将状态重置，只有撤销上一次的选择，重置现场，才能够回到完全一样的过去，再开始新的尝试才会是有效的。
当然也可以不回溯，每一次尝试都**复制**，在每一个非叶子结点的分支的尝试，都创建新的变量表示状态向下一层节点传递，这样就可以不需要回溯了，但是这种做法会有一定的空间和时间消耗
```markdown
void backtrack(const vector<int>& nums, vector<int> path, vector<bool> visited) {
    if (path.size() == nums.size()) {
        result.push_back(path);
        return;
    }
    for (int i = 0; i < nums.size(); i++) {
        if (visited[i] == true) {
            continue; 
        }
        vector<int> newPath = path;
        vector<bool> newVisited = visited;
        newPath.push_back(nums[i]);
        newVisited[i] = true;
        backtrack(nums, newPath, newVisited);
        //path.pop_back();
        //visited[i] = false;
    }
}

```
这就好比我们在实验室里做「对比实验」，每一个步骤的尝试都要保证使用的材料是一样的。我们有两种办法：  
  * 每做完一种尝试，都把实验材料恢复成做上一个实验之前的样子，只有这样做出的对比才有意义；  
  * 每一次尝试都使用同样的新的材料做实验。  

### 剪枝
  * 回溯算法应会应用**剪枝**技巧达到以加快搜索速度。有些时候，比如需要对待搜索的数据做一些预处理工作（如排序）才能达到剪枝的目的
  * 回溯问题本身时间复杂度很高，所以尽量用空间换时间

### 总结
做回溯问题算法题的时候，应先画树形图，在画图过程中应该思考清楚：
  * 分支如何产生；
  * 题目需要的解在哪里？在叶子结点还是非叶子结点？还是从根节结点到叶子结点的路径
  * 哪些搜索会产生不需要的解？产生重复的原因是什么？如果在树形图的浅层就知道这个分支不能产生需要的结果，应该提前剪枝

## 回溯题目
#### 排列、组合、子集相关问题
先绘图，再编码。思考可以剪枝的条件，哪些需要visited数组，哪些有需要设置搜索起点begin变量
  * [46全排列](https://leetcode-cn.com/problems/permutations/)
  * [47全排列(元素可重复)](https://leetcode-cn.com/problems/permutations-ii/)
  ```markdown
      void backtrack(vector<int>&nums, vector<int> path, vector<bool> visited) {
        if (path.size() == nums.size()) {
            result.push_back(path);
            return;
        }
        for (int i = 0; i < nums.size(); i++) {
            if (visited[i] == true) {
                continue;
            }
            // 绘制出决策树可以得知，当前面的元素与当前元素相等，并且前一个元素又没有被选中时，那么决策树中选当前元素和前一元素的形成的树的分支是完全一样的，因此需要剪枝
            if (i > 0 && nums[i-1] == nums[i] && visited[i-1] == false) {
                continue;
            }
            path.push_back(nums[i]);
            visited[i] = true;
            backtrack(nums, path, visited);
            path.pop_back();
            visited[i] = false;
        }
    }
  ```
  * [39组合总和](https://leetcode-cn.com/problems/combination-sum/)  
  不同于排列问题（讲究顺序），组合问题（不讲究顺序）认为[1,2,3],[3,2,1]的是同一组解。如果按照排列的方式设置visited数组，绘制出决策树会发现解中存在[1,2,3],[3,2,1]重复解，
  因此对于这一类不计算顺序的问题时，我们在搜索时就需要**按照某种顺序搜索**，具体的做法是：每一次搜索的时候设置**下一轮搜索的起点begin**，这样就避免了[3,2,1]这种重复解决，并且
  不会遗漏，因为[1,2,3]这组解必包含在以1为根的子决策树上。
  ```markdown
  // 方法1：剪枝位置在选择之后
      void backtrack(vector<int>& candidates, vector<int>& path, int index, int target) {
        if (target < 0) {// 剪枝
            return;
        }
        if (target == 0) {
            result.push_back(path);
            return;
        }
        for (int i = index; i < candidates.size(); i++) {
            if (target < candidates[i]) {
                continue;
            }
            path.push_back(candidates[i]);
            backtrack(candidates, path, i, target - candidates[i]);
            path.pop_back();
        }
    }
  // 方法2：剪枝位置在选择时
     void backtrack(vector<int>& candidates, vector<int>& path, int index, int target) {
        if (target == 0) {
            result.push_back(path);
            return;
        }
        for (int i = index; i < candidates.size(); i++) {
            if (target < candidates[i]) {//剪枝
                continue;
            }
            path.push_back(candidates[i]);
            backtrack(candidates, path, i, target - candidates[i]);
            path.pop_back();
        }
    }
  // 方法3：根据上面画树形图的经验，如果target减去一个数得到负数，那么减去一个更大的树依然是负数，同样搜索不到结果。
  // 基于这个想法，我们可以对输入数组进行排序，添加相关逻辑达到进一步剪枝的目的  
    sort(candidates.begin(), candidates.end());
    void backtrack(vector<int>& candidates, vector<int>& path, int index, int target) {
        if (target == 0) {
            result.push_back(path);
            return;
        }
        for (int i = index; i < candidates.size(); i++) {
            if (target < candidates[i]) {
                break;            //剪枝提速
            }
            path.push_back(candidates[i]);
            backtrack(candidates, path, i, target - candidates[i]);
            path.pop_back();
        }
    }
  ```
  * [40组合总和(含重复元素)](https://leetcode-cn.com/problems/combination-sum-ii/)   
  这里组合类问题剪枝的思路，先对数组升序排列，减枝发生在**同一层数值相同的结点的第2，3...个结点，因为数值相同的第一个结点已经搜索出了包含这个数值的全部结果**，同一层的其他结点，候选数更少，
  搜索出的结果一定不会比第1个结点更多，并且是第1个结点的子集。  
  ![](https://github.com/zhb-ustc/zhb-ustc.github.io/blob/main/1599718525-iXEiiy-image.png)  
  ```markdown
      void backtrack(vector<int>& candidates, vector<int> path, int begin, int target) {
        if (target == 0) {
            result.push_back(path);
            return;
        }

        for (int i = begin; i < candidates.size(); i++) {
            if (target < candidates[i]) {
                break;  //减枝
            }
            if (i > begin && candidates[i - 1] == candidates[i]) {  //剪枝：去除决策树同一层相同结点的非第一个结点
                continue;
            }
            path.push_back(candidates[i]);
            backtrack(candidates, path, i + 1, target - candidates[i]);
            path.pop_back();
        }
    }
    vector<vector<int>> result;
    
    解释：if (i > begin && candidates[i - 1] == candidates[i])
    这个方法最重要的作用是，可以让同一层级，不出现相同的元素。即
                      1
                     / \
                    2   2  这种情况不会发生 但是却允许了不同层级之间的重复即：
                   /     \
                  5       5
                    例2
                      1
                     /
                    2      这种情况确是允许的
                   /
                  2  

    为何会有这种神奇的效果呢？
    首先 i - 1 == i 是用于判定当前元素是否和之前元素相同的语句。这个语句就能砍掉例1。
    可是问题来了，如果把所有当前与之前一个元素相同的都砍掉，那么例二的情况也会消失。 
    因为当第二个2出现的时候，他就和前一个2相同了。

    那么如何保留例2呢？
    那么就用i > begin 来避免这种情况，你发现例1中的两个2是处在同一个层级上的，
    例2的两个2是处在不同层级上的。
    在一个for循环中，所有被遍历到的数都是属于一个层级的。我们要让一个层级中，
    必须出现且只出现一个2，那么就放过第一个出现重复的2，但不放过后面出现的2。
    第一个出现的2的特点就是 i == begin. 第二个出现的2 特点是i > begin.
  ```
  
```markdown
* [77组合](https://leetcode-cn.com/problems/combinations/)
* [78子集](https://leetcode-cn.com/problems/subsets/)
Syntax highlighted code block

# Header 1
## Header 2
### Header 3

- Bulleted
- List

1. Numbered
2. List

**Bold** and _Italic_ and `Code` text

[Link](url) and ![Image](src)
```

For more details see [GitHub Flavored Markdown](https://guides.github.com/features/mastering-markdown/).

### Jekyll Themes

Your Pages site will use the layout and styles from the Jekyll theme you have selected in your [repository settings](https://github.com/zhb-ustc/zhb-ustc.github.io/settings). The name of this theme is saved in the Jekyll `_config.yml` configuration file.

### Support or Contact

Having trouble with Pages? Check out our [documentation](https://docs.github.com/categories/github-pages-basics/) or [contact support](https://support.github.com/contact) and we’ll help you sort it out.
