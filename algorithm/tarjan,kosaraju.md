# tarjan
参考[博客](https://www.cnblogs.com/shadowland/p/5872257.html)    
## 作用：
解决强连通分量，双连通分量，割点和桥，求最近公共祖先的问题
## 概述
>Tarjan 算法是基于深度优先搜索的算法，用于求解图的连通性问题。Tarjan 算法可以在线性时间内求出无向图的割点与桥，进一步地可以求解无向图的双连通分量；同时，也可以求解有向图的强连通分量、必经点与必经边。



割点：从图中删除结点x以及与x有关的所有边后该图分裂，则x为图的割点    
桥：删除边e后，图分裂，则e为桥      
## 定义：
dfn[i]:在dfs该结点被搜索的次序，即时间戳     
low[i]:i或**i的子树**能够被追溯到的最早的栈中结点的次序号     
显然dfn[i]==low[i]时，i或i的子树可以构成一个强连通分量   
```
void Tarjan ( int x ) {
         dfn[ x ] = ++dfs_num ;
         low[ x ] = dfs_num ;
         vis [ x ] = true ;//是否在栈中
         stack [ ++top ] = x ;
         for ( int i=head[ x ] ; i!=0 ; i=e[i].next ){
                  int temp = e[ i ].to ;
                  if ( !dfn[ temp ] ){
                           Tarjan ( temp ) ;
                           low[ x ] = gmin ( low[ x ] , low[ temp ] ) ;
                 }
                 else if ( vis[ temp ])low[ x ] = gmin ( low[ x ] , dfn[ temp ] ) ;
         }
         if ( dfn[ x ]==low[ x ] ) {//构成强连通分量
                  vis[ x ] = false ;
                  color[ x ] = ++col_num ;//染色
                  while ( stack[ top ] != x ) {//清空
                           color [stack[ top ]] = col_num ;
                           vis [ stack[ top-- ] ] = false ;
                 }
                 top -- ;
         }
}
```
# kosaraju
参考[博客](https://www.cnblogs.com/shadowland/p/5876307.html)    
## 概述
>Kosaraju的算法（又称为–Sharir Kosaraju算法）是一个线性时间(linear time)算法找到的有向图的强连通分量。它利用了一个事实，逆图（与各边方向相同的图形反转, transpose graph）有相同的强连通分量的原始图



逆图☞有向图的各边反向     
## 步骤
1. 对原图dfs，盖上时间戳
2. 选择大时间戳，对逆图dfs，能到达的点构成强连通分量。
3. 重复2
## 代码
```
#include "cstdio"
#include "iostream"
#include "algorithm"

using namespace std ;

const int maxN = 10010 , maxM = 50010;

struct Kosaraju { int to , next ; } ;

Kosaraju E[ 2 ][ maxM ] ;
bool vis[ maxN ];
int head[ 2 ][ maxN ] , cnt[ 2 ] , ord[maxN] , size[maxN] ,color[ maxN ];

int tot , dfs_num  , col_num , N , M  ;

void Add_Edge( int x , int y , int _ ){//建图
         E[ _ ][ ++cnt[ _ ] ].to = y ;
         E[ _ ][ cnt[ _ ] ].next = head[ _ ][ x ] ;
         head[ _ ][ x ] = cnt[ _ ] ;
}

void DFS_1 ( int x , int _ ){
         dfs_num ++ ;//发现时间
         vis[ x ] = true ;
         for ( int i = head[ _ ][ x ] ; i ; i = E[ _ ][ i ].next ) {
                 int temp = E[ _ ][ i ].to;
                 if(vis[ temp ] == false) DFS_1 ( temp , _ ) ;
         }
         ord[(N<<1) + 1 - (++dfs_num) ] = x ;//完成时间加入栈
}

void DFS_2 ( int x , int _ ){
         size[ tot ]++ ;// 强连通分量的大小
         vis[ x ] = false ;
         color[ x ] = col_num ;//染色
         for ( int i=head[ _ ][ x ] ; i ; i = E[ _ ][ i ].next ) {
                 int temp = E[ _ ][ i ].to;
                 if(vis[temp] == true) DFS_2(temp , _);
         }
}

int main ( ){
         scanf("%d %d" , &N , &M );
         for ( int i=1 ; i<=M ; ++i ){
                 int _x , _y ;
                 scanf("%d %d" , &_x , &_y ) ;
                 Add_Edge( _x , _y , 0 ) ;//原图的邻接表
                 Add_Edge( _y , _x , 1 ) ;//逆图的邻接表
         }
         for ( int i=1 ; i<=N ; ++i )
                 if ( vis[ i ]==false )
                          DFS_1 ( i , 0 ) ;//原图的DFS

         for ( int i = 1 ; i<=( N << 1) ; ++i ) {
                 if( ord[ i ]!=0 && vis[ ord[ i ] ] ){
                          tot ++ ; //强连通分量的个数
                          col_num ++ ;//染色的颜色
                          DFS_2 ( ord[ i ] , 1 ) ;
                 }
         }

         for ( int i=1 ; i<=tot ; ++i )
                 printf ("%d ",size[ i ]);
         putchar ('\n');
         for ( int i=1 ; i<=N ; ++i )
                 printf ("%d ",color[ i ]);
         return 0;
}
```
