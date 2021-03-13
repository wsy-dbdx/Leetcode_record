首先kmp算法就是找子串的位置，普通做法其实不能用滑动窗口来做，因为一个子串比较不是在一个字符串中找，而是两个字符串，比较失败需要退回，所以每个字符串失败之后还要退回到原来的位置，这种做法效率很低，kmp就是利用有限状态机来指定这个字符串往哪返回而不是返回原来第一个位置的下一个（其实类似动态规划）。所以对于每一个模式字符串p，会创造一个属于它的dp数组，它决定了这次比较字符该往哪走。dp[i][j]表示从p的i位置的字符如果碰到了j这个字符（256长度的ascii码）该往哪走。所以在比较的时候，原字符串不会回退，永远向前走，退后的只有p模式串。比较到p到了最后则返回位置，如果到最后都没返回则说明无法匹配。
那么如何得到p的dp数组，就是两个for循环i和j遍历每一个p位置可能遇到的所有字符，创建一个影子位置x，如果此时的字符就等于p此时的字符，则直接往前移动等于i+1，如果不等于，则等于dp[x][j],那x怎么得到x=dp[x][i位置字符]。

```java
class Solution {
    int[][] dp;
    public int strStr(String haystack, String needle) {
        int h=haystack.length();
        int n=needle.length();
        if(n==0)return 0;
        if(h==0)return -1;
        dp=new int[n][26];
        kmp(needle);
        int j=0;
        for(int i=0;i<h;i++){
            j=dp[j][haystack.charAt(i)-'a'];
            if(j==n)return i-j+1;
        }
        return -1;
    }
    public void kmp(String str){
        int l=str.length();
        dp[0][str.charAt(0)-'a']=1;
        int x=0;
        for(int i=1;i<l;i++){
            for(int j=0;j<26;j++){
                if(j==str.charAt(i)-'a'){
                    dp[i][j]=i+1;
                }else{
                    dp[i][j]=dp[x][j];
                }
            }
            x=dp[x][str.charAt(i)-'a'];
        }
    }
}
```
