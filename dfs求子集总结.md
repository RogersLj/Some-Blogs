# [LeetCode 78. 子集](https://leetcode-cn.com/problems/subsets/) 
 
 阅读此篇之前建议阅读 [DFS求全排列总结](https://www.acwing.com/file_system/file/content/whole/index/content/572190/) ，求子集相对于求全排列来说简单不少，因为我们在考虑方案时，对于每一个数，我们只有选或不选两种选择，因此如果像全排列那样画出递归搜索树，求子集的递归搜索树正好是一棵满二叉树。所有方案刚好是所有叶子结点，因此对于一个大小为`n`的集合来说，所有子集的个数为$2^n$个。

## dfs + 回溯法

和全排列的做法一样，当我们走到叶子结点时，就把该路径加入方案中。如果还没有走到叶子节点，那么对于枚举的当前数，我们有两种选择，选或不选，做出选择再递归到下一层，同时记得回溯。


### C ++代码
```
class Solution {
public:
    vector<vector<int>> res;
    vector<int> path;

    vector<vector<int>> subsets(vector<int>& nums) {
        dfs(nums, 0);
        return res;    
    }

    void dfs(vector<int>& nums, int u)
    {
        if (u >= nums.size())
        {
            res.push_back(path);
            return;
        }

        dfs(nums, u + 1); //不选当前数，递归下一层
        path.push_back(nums[u]);  //选当前数
        dfs(nums, u + 1); //递归下一层
        path.pop_back(); //回溯
    }
};
```

## 位运算法

在前面的分析中我们知道对于每个数，我们可以选或者不选，两种选择刚好对应两种状态，因此我们可以采用位运算的思路进行求解。对于一个大小为`n`的集合来说，我们需要用一个`n`位二进制数来表示每个数的选择状态，所有方案刚好是所有叶子结点，所有子集的个数为$2^n$个，所以每个叶子结点的状态刚好可以用0 ~ $2^n - 1$中的二进制数表示。

例如对于集合[1, 2, 3]

|  二进制数  | 表示集合  |
|  ----  | ----  |
| 000  | [] |
| 001  | [3] |
| 010  | [2] |
| 011  | [2, 3] |
| 100  | [1] |
| 101  | [1, 3] |
| 110  | [1, 2] |
| 111  | [1, 2, 3] |

### C ++代码

```
class Solution {
public:
    vector<vector<int>> subsets(vector<int>& nums) {
        int n = nums.size();
        int p = 1 << n;
        vector<vector<int>> res(p);

        for (int i = 0; i < p; i ++)
            for (int j = 0; j < n; j ++)
                if ((i >> j) & 1)
                    res[i].push_back(nums[j]);

        return res;    
    }
};
```

# [LeetCode 90. 子集 II ](https://leetcode-cn.com/problems/subsets-ii/) 
 
## 算法思路

在上面的分析中我们知道求子集问题与[求全排列问题](https://www.acwing.com/file_system/file/content/whole/index/content/572190/) 的区别在于我们不需要考虑每个数在的位置或每个位置上是什么数，我们只在乎每一个数选或不选。因此对于有重复元素的子集问题，我们的思路不再像全排列那样关心每个数的先后关系，我们只需要关心相同的数选几个。具体步骤如下：

1. 将原数组排序，目的是将相同元素放在一起，之后好计算每个相同数的个数
2. 计算出当前数相同的个数，然后分别做出不选，选1个，选2个...的选择，然后递归到下一层
3. 注意在每做完一个选择递归到下一层时不需要马上回溯，因为我们选1个，选2个...每一个之后的选择是建立在之前选择上多选一个，所以我们不需要马上回溯
4. 当做出所有选择后，我们才需要将选择的这一段相同的数清空，所以回溯需要在做完所有选择后再进行


 
### C ++代码

```
class Solution {
public:
    vector<vector<int>> res;
    vector<int> path;
    vector<vector<int>> subsetsWithDup(vector<int>& nums) {
        sort(nums.begin(), nums.end());
        dfs(nums, 0);
        return res;
    }

    void dfs(vector<int>& nums, int u)
    {
        int n = nums.size();
        if(u == n)
        {
            res.push_back(path);
            return;
        }

        int k = u;
        while(k < nums.size() && nums[k] == nums[u]) k++; 
        dfs(nums, k); //当前这种数一个都不选
        for(int i = u; i < k; i++) //分别选1个,2个...
        { 
            path.push_back(nums[u]); 
            dfs(nums, k);
        }
        path.erase(path.end() - (k - u), path.end()); //回溯，将选择的这一段相同的数全部清空
    }
};
```

**接下来的两题是通过数组模拟的题目，分析思路与上面的分析一模一样**

# [AcWing 92. 递归实现指数型枚举](https://www.acwing.com/problem/content/94/) 
 
### C ++代码
 
```
#include <iostream>

using namespace std;

const int N = 20;

int n;
bool st[N];

void dfs(int u)
{
    if (u > n)
    {
        for (int i = 1; i <= n; i ++)
            if (st[i])  
                cout << i << ' ';
                
        cout << endl;
        return;
    }
    
    dfs(u + 1);
    st[u] = true;
    dfs(u + 1);
    st[u] = false;
}

int main()
{
    cin >> n;
    dfs(1);
    return 0;
}

```
 
# [AcWing 1572. 递归实现指数型枚举 II](https://www.acwing.com/problem/content/1574/) 
  
### C ++代码
```
#include <iostream>
#include <algorithm>

using namespace std;

const int N = 20;

int n;
int a[N];
bool st[N];

void dfs(int u)
{
    if (u >= n)
    {
        for (int i = 0; i < n; i ++)
            if (st[i])
                cout << a[i] << ' ';
                
        cout << endl;
        return;
    }
    
    int k = u;
    while (k < n && a[k] == a[u]) k ++;
    
    dfs(k);
    for (int i = u; i < k; i ++)
    {
        st[i] = true;
        dfs(k);
    }
    
    for (int i = u; i < k; i ++) st[i] = false;
}

int main()
{
    cin >> n;
    for (int i = 0; i < n; i ++) cin >> a[i];
    sort(a, a + n);
    dfs(0);
    return 0;
}
  
```
