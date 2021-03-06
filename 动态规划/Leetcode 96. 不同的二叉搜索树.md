### Leetcode 96. 不同的二叉搜索树

解法：动态规划

```cpp
dp[i]表示节点为[1,i]可以构成的二叉搜索树的数量.
则状态转移方程为:
	dp[i]=dp[0]*dp[i-1]+dp[i]*dp[i-2]+...dp[i-1]*dp[0];

因为根节点是不固定的，所以[1..i]中任意一个数均可作为根节点。
    
然后，根节点左边的数作为左子树上的值，根节点右边的数作为右子树上的值。
    
由于左右子树上根节点又是随机的，所以两者是可以任意组合的，所以相乘。
    
边界条件:dp[0]=1,dp[1]=1;
```

```cpp
int numTrees(int n) {
        int dp[n+1];
        memset(dp,0,sizeof dp);
        dp[0]=1,dp[1]=1;
        for(int i=2;i<=n;i++)
        {
            for(int j=1;j<=i;j++)
            {
                dp[i] += dp[j-1]*dp[i-j];
            }
        }
        return dp[n];
    }
```

