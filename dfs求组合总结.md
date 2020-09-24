# [LeetCode 77. 组合](https://www.acwing.com/solution/content/14588/) 

## 算法思路

在[DFS求子集总结](https://www.acwing.com/file_system/file/content/whole/index/content/572276/) 中已经详细介绍了求子集的过程，其实这道题本质上也是一个求子集的问题。题目给定两个整数 n 和 k，返回 1 ... n 中所有可能的 k 个数的组合。其实就是求由1 ~ n的数字组成集合的子集，并且子集的个数刚好是k的所有子集方案。因此我们可以在求子集的同时再添加一个参数`s`,来表示当前该子集中的元素个数，当个数刚好为`k`时，即可加入方案，具体代码实现如下：

### C ++代码
```
class Solution {
public:
    vector<vector<int>> res;
    vector<int> path;

    vector<vector<int>> combine(int n, int k) {
        dfs(n, 1, 0, k);
        return res;    
    }

    void dfs(int n, int u, int s, int k)
    {
        if (s == k) //该子集个数恰好为k
        {
            res.push_back(path); //添加方案
            return;
        }

        if (u > n) return; //说明已经遍历完所有数，但是子集大小不足k
        path.push_back(u); //选当前数
        dfs(n, u + 1, s + 1, k); //子集个数s+1
        path.pop_back(); //回溯
        dfs(n, u + 1, s, k); //不选当前数，子集个数s不变
    }
};

```

# [AcWing 93. 递归实现组合型枚举](https://www.acwing.com/problem/content/description/95/) 

**数组模拟，思路与上面一样**

### C ++代码

```
#include<iostream>

using namespace std;

const int N = 30;

int n, m;
bool st[N];

void dfs(int u, int s)
{

    if(s == m)
    {
        for(int i = 1; i <= n; i++)
        {
            if(st[i]) cout<<i<<' ';
        }
        cout<<endl;
        return;
    }
    
    if (u > n) return;
    
    st[u] = true;
    dfs(u + 1, s + 1);
    st[u] = false;
    dfs(u + 1, s);
}

int main()
{
    cin>>n>>m;
    dfs(1, 0);
    return 0;
}
```


#  [AcWing 1573. 递归实现组合型枚举 II](https://www.acwing.com/problem/content/1575/) 

该题是具有重复元素的组合型问题，在[LeetCode 90. 子集 II ](https://www.acwing.com/solution/content/14587/) 中介绍过处理重复元素的子集问题，方法是先将原数组进行排序。每次枚举时先计算该数的相同数个数，然后再枚举选该数的个数，不选，选1个，选2个...

**注意：该题要求字典序输出组合**，因此遇到一段相同的数时，我们先将其全部选上，然后每次的个数逐渐减少，直到当前数一个都不选为止。

### C ++代码

```
#include <iostream>
#include <algorithm>

using namespace std;

const int N = 30;

int n, m;
int a[N];
bool st[N];


void dfs(int u, int s)
{
    if (s == m)
    {
        for (int i = 0; i < n; i ++)
            if (st[i])
                cout << a[i] << ' ';
                
        cout << endl;
        return;
    }
    if (u >= n) return;
    
    int k = u;
    while (k < n && a[k] == a[u]) k ++; //计算该相同数个数
    
    for (int i = u; i < k; i ++) st[i] = true, s ++; //这一段数都选
    dfs(k, s); //递归下一层
    for (int i = u; i < k; i ++)
    {
        st[i] = false; //每次选择减一
        s --; //个数--
        dfs(k, s); //递归下一层
    }
}

int main()
{
    cin >> n >> m;
    for (int i = 0; i < n; i ++) cin >> a[i];
    sort(a, a + n);
    dfs(0, 0);
    return 0;
}
```
