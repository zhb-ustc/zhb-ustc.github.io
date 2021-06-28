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

### 回溯法框架（以全排列）
```markdown
Syntax highlighted code block
void backtrack(const vector<int>& nums, vector<int> path, vector<bool> visited) {
    // 触发结束条件
    if (path.size() == nums.size()) {
        result.push_back(path);
        return;
    }
    
    for (int i = 0; i < nums.size(); i++) {
        // 排除不合法选择
        if (visited[i] == true) {
            continue; 
        }
        
        path.push_back(nums[i]);
        visited[i] = true;
        backtrack(nums, path, visited);
        path.pop_back();
        visited[i] = false;
    }
}




```markdown
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
