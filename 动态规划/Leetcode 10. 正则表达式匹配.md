### Leetcode 10. 正则表达式匹配

解法：动态规划。

```cpp
//定义状态dp[i][j]表示s中以下标i结尾的字符串和p中以下标j结尾的字符串能否匹配成功。
//状态转移方程为:
	//1.如果s[i]==p[j] or p[j]=='.',则p[j]一定能和s[i]进行匹配，所以dp[i][j]=dp[i-1][j-1]
	//2.如果p[i]=='*',分两种情况讨论：
			//(1)s[i]!=p[j-1] and p[j-1]!='.',则p[j-1]与s[i]匹配不成功，此时应将p[j-1]和p[j]舍去，所以dp[i][j]=dp[i][j-2]
			//(2)除了(1)以外的其他情况，p[j-1]与s[i]匹配成功。此时要看p[j-1]和p[j]匹配几个。
					dp[i][j]=dp[i][j-1]//代表匹配了一个
                    dp[i][j]=dp[i-1][j]//代表匹配了2个及其以上
                    dp[i][j]=dp[i][j-2]//代表匹配了0个
//边界条件:
      //p串出现a*b*之类的形式。由于dp[0][0]=true，所以每隔2个字母碰到一个*便可以设置为true
```




```cpp
bool isMatch(string s, string p) {
        int N=s.size(),M=p.size();
        bool dp[N+1][M+1];//根据状态转移方程可知，要多加一行和一列，避免复杂的边界条件
        memset(dp,0,sizeof dp);
        dp[0][0]=true;
        for(int i=1;i<M;i+=2)
        {//出现a*b*之类的。由于dp[0][0]=true，所以每隔2个字母碰到一个*便可以设置为true
            if(p[i]=='*')   dp[0][i+1]=true;
            else break;
        }
        for(int i=1;i<=N;i++)
        {
            for(int j=1;j<=M;j++)
            {
                if((s[i-1]==p[j-1] || p[j-1]=='.') && dp[i-1][j-1])
                    dp[i][j]=true;  //匹配单个字符
                if(p[j-1]=='*')
                {
                    if(j>=2 && s[i-1]!=p[j-2] && p[j-2]!='.' )
                    {//p中当前字符为*,前一个字符和s中前一个字符无法匹配。视_*匹配了0个字符
                        dp[i][j]=dp[i][j-2];
                    }
                    else
                        dp[i][j]=dp[i][j-1] || dp[i-1][j] || (j>=2 && dp[i][j-2]);
                    //dp[i][j-1]表示_*代表匹配1个
                    //dp[i-1][j]代表_*匹配多个字符
                    //dp[i][j-2]表示跳过_*这两个字符,不参与匹配
                }
            }
        }
        return dp[N][M];
    }
```

