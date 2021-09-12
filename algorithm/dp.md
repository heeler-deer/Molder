# DP--damp proofing
## 线性dp
状态间有线性关系典型应用：
1. 数字三角形，找出路径使和最大
2. 最长上升子序列
3. 最长公共子序列
## 区间dp
>设有 n 堆石子排成一排，其编号为 n。
每堆石子有一定的质量，可以用一个整数来描述，现在要将这 n 堆石子合并成为一堆。
每次只能合并相邻的两堆，合并的代价为这两堆石子的质量之和，合并后与这两堆石子相邻的石子将和新堆相邻，合并时由于选择的顺序不同，合并的总代价也不相同。
请找出一种合理的方法，使总的代价最小，输出最小代价



待求解问题是给定区间上的离散，带权值的点，需要把原区间划分后合并，通过合并区间上的最优点进而得出大区间的解
## 背包dp
## 树形dp
一棵树用最少的代价或者收益来完成给定的操作。
[二叉树](https://www.luogu.com.cn/problem/P2015)    
```
#include <bits/stdc++.h>
using namespace std;
const int N=150;
int n,q,dp[N][N];
struct node{
    int lson,rson,val;
}tree[20*N];
int dfs(int now,int point){
    if(now==0||point==0){
        return 0;
    }
    if(tree[now].lson==0&&tree[now].rson==0){
        return tree[now].val;

    }
    //////////////////////////////////
    if(dp[now][point]>0)return dp[now][point];
    for(int k=0;k<point;k++){
        dp[now][point]=max(dp[now][point],dfs(tree[now].lson,k)+dfs(tree[now].rson,point-k-1));

    }
    /////////////////////////
    return dp[now][point];
}
int main(){
    cin>>n>>q;
    for(int i=1;i<=n-1;i++){
        int fa,son,v;
        cin>>fa>>son>>v;
        tree[son].val=v;
        if(tree[fa].lson==0)tree[fa].lson=son;
        else tree[fa].rson=son;
    }
    cout<<dfs(1,q+1);
    return 0;
}
```
## 状压dp
## 数位dp
>  数位dp是一种计数用的dp，一般就是要统计一个区间  [A , B ] 内满足一些条件数的个数。




例题比如给定区间n~m，求n~m中没有62或4的数的个数
```
ll dfs(int pos,int pre,int st,……,int lead,int limit)//记搜
{
    if(pos>len) return st;//剪枝
    if((dp[pos][pre][st]……[……]!=-1&&(!limit)&&(!lead))) return dp[pos][pre][st]……[……];//记录当前值
    ll ret=0;//暂时记录当前方案数
    int res=limit?a[len-pos+1]:9;//res当前位能取到的最大值
    for(int i=0;i<=res;i++)
    {
        //有前导0并且当前位也是前导0
        if((!i)&&lead) ret+=dfs(……,……,……,i==res&&limit);
        //有前导0但当前位不是前导0，当前位就是最高位
        else if(i&&lead) ret+=dfs(……,……,……,i==res&&limit); 
        else if(根据题意而定的判断) ret+=dfs(……,……,……,i==res&&limit);
    }
    if(!limit&&!lead) dp[pos][pre][st]……[……]=ret;//当前状态方案数记录
    return ret;
}
ll part(ll x)//把数按位拆分
{
    len=0;
    while(x) a[++len]=x%10,x/=10;
    memset(dp,-1,sizeof dp);//初始化-1（因为有可能某些情况下的方案数是0）
    return dfs(……,……,……,……);//进入记搜
}
int main()
{
    scanf("%d",&T);
    while(T--)
    {
        scanf("%lld%lld",&l,&r);
        if(l) printf("%lld",part(r)-part(l-1));//[l,r](l!=0)
        else printf("%lld",part(r)-part(l));//从0开始要特判
    }
    return 0;
}
```