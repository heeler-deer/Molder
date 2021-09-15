```
#include <iostream>
#include <cstdio>
#define mod 1000000007
#define maxn 10000000
using namespace std;
using LL=long long int;
LL n,m;
LL cnt=0;
LL sumx[maxn],sumy[maxn],sumz[maxn];
LL lson[maxn],rson[maxn];
LL lazyx[maxn],lazyy[maxn],lazyz[maxn],lazymul[maxn],lazyro[maxn];
void swp(LL& a,LL& b){
    LL temp;
    temp=a;
    a=b;
    b=temp;
}
void pushdown(LL p,LL start,LL end){
    if(start==end){
    
        lazyx[p]=lazyy[p]=lazyz[p]=lazymul[p]=lazyro[p]=0;
        return;
    }
    LL& lchild=lson[p];
    LL& rchild=rson[p];
    if(!lchild)lchild=++cnt;
    if(!rchild)rchild=++cnt;
    LL mid=(start+end)>>1;
    if(!lazymul[p])lazymul[p]=1;
    sumx[lchild]=(sumx[lchild]*(lazymul[p]%mod)%mod+((mid-start+1)%mod)*(lazyx[p]%mod)%mod)%mod;
    sumy[lchild]=(sumy[lchild]*(lazymul[p]%mod)%mod+((mid-start+1)%mod)*(lazyy[p]%mod)%mod)%mod;
    sumz[lchild]=(sumz[lchild]*(lazymul[p]%mod)%mod+((mid-start+1)%mod)*(lazyz[p]%mod)%mod)%mod;
    sumx[rchild]=(sumx[rchild]*(lazymul[p]%mod)%mod+((end-mid)%mod)*(lazyx[p]%mod)%mod)%mod;
    sumy[rchild]=(sumy[rchild]*(lazymul[p]%mod)%mod+((end-mid)%mod)*(lazyy[p]%mod)%mod)%mod;
    sumz[rchild]=(sumz[rchild]*(lazymul[p]%mod)%mod+((end-mid)%mod)*(lazyz[p]%mod)%mod)%mod;
    for(LL i=0;i<lazyro[p];i++){
        swp(sumx[lchild],sumy[lchild]);
        swp(sumy[lchild],sumz[lchild]);
        swp(sumx[rchild],sumy[rchild]);
        swp(sumy[rchild],sumz[rchild]);

    }
    LL u,v,w;
    u=lazyx[p];
    v=lazyy[p];
    w=lazyz[p];
    for(LL i=0;i<lazyro[lchild];i++){
        swp(v,w);
        swp(u,v);
    }
    lazyx[lchild]=(lazyx[lchild]*(lazymul[p]%mod)%mod+u%mod)%mod;
    lazyy[lchild]=(lazyy[lchild]*(lazymul[p]%mod)%mod+v%mod)%mod;
    lazyz[lchild]=(lazyz[lchild]*(lazymul[p]%mod)%mod+w%mod)%mod;
    u=lazyx[p];
    v=lazyy[p];
    w=lazyz[p];
    for(LL i=0;i<lazyro[rchild];i++){
        swp(v,w);
        swp(u,v);
    }
    lazyx[rchild]=(lazyx[rchild]*(lazymul[p]%mod)%mod+u%mod)%mod;
    lazyy[rchild]=(lazyy[rchild]*(lazymul[p]%mod)%mod+v%mod)%mod;
    lazyz[rchild]=(lazyz[rchild]*(lazymul[p]%mod)%mod+w%mod)%mod;
    if(lazymul[p]!=1){
        if(!lazymul[lchild])lazymul[lchild]=1;
        lazymul[lchild]=(lazymul[lchild]*(lazymul[p]%mod))%mod;
        if(!lazymul[rchild])lazymul[rchild]=1;
        lazymul[rchild]=(lazymul[rchild]*(lazymul[p]%mod))%mod;

    }
    lazyro[lchild]=(lazyro[lchild]+lazyro[p])%3;
    lazyro[rchild]=(lazyro[rchild]+lazyro[p])%3;
    lazyx[p]=0;
    lazyy[p]=0;
    lazyz[p]=0;
    lazymul[p]=0;
    lazyro[p]=0;
}
void add(LL &p,LL start,LL end,LL l,LL r,LL a,LL b,LL c){
    if(!p)p=cnt++;
    if(end<l||start>r)return;
    if(start>=l&&end<=r){
        sumx[p]=(sumx[p]+(end-start+1)%mod*(a%mod)%mod)%mod;
        sumy[p]=(sumy[p]+(end-start+1)%mod*(b%mod)%mod)%mod;
        sumz[p]=(sumz[p]+(end-start+1)%mod*(c%mod)%mod)%mod;
        for(LL i=0;i<lazyro[p];i++){
            swp(b,c);
            swp(a,b);
        }
        lazyx[p]=(lazyx[p]+a%mod)%mod;
        lazyy[p]=(lazyy[p]+b%mod)%mod;
        lazyz[p]=(lazyz[p]+c%mod)%mod;
        return;
    }
    if(lazyx[p]||lazyy[p]||lazyz[p]||lazymul[p]||lazyro[p])pushdown(p,start,end);
    LL mid=(start+end)>>1;
    if(mid>=1)add(lson[p],start,mid,l,r,a,b,c);
    if(mid<r)
    add(rson[p],mid+1,end,l,r,a,b,c);
    sumx[p]=(sumx[lson[p]]%mod+sumx[rson[p]%mod])%mod;
    sumy[p]=(sumy[lson[p]]%mod+sumy[rson[p]%mod])%mod;
    sumz[p]=(sumz[lson[p]]%mod+sumz[rson[p]%mod])%mod;

}
void mul(LL &p,LL start,LL end,LL l,LL r,LL k){
    if(!p)p=++cnt;
    if(end<l||start>r)return;
    if(start>=l&&end<=r){
        if(!lazymul[p])lazymul[p]=1;
        sumx[p]=(sumx[p]*(k%mod))%mod;
        sumy[p]=(sumy[p]*(k%mod))%mod;
        sumz[p]=(sumz[p]*(k%mod))%mod;
        lazyx[p]=(lazyx[p]*(k%mod))%mod;
        lazyy[p]=(lazyy[p]*(k%mod))%mod;
        lazyz[p]=(lazyz[p]*(k%mod))%mod;
        lazymul[p]=(lazymul[p]*(k%mod))%mod;
        return;
    }
    if (lazyx[p] || lazyy[p] || lazyz[p] || lazymul[p] || lazyro[p])
		pushdown(p, start, end);
	LL mid = (start + end) >> 1;
	if (mid >= l)
		mul(lson[p], start, mid, l, r, k);
	if (mid < r)
		mul(rson[p], mid + 1, end, l, r, k);
	sumx[p] = (sumx[lson[p]] % mod + sumx[rson[p] % mod]) % mod;
	sumy[p] = (sumy[lson[p]] % mod + sumy[rson[p] % mod]) % mod;
	sumz[p] = (sumz[lson[p]] % mod + sumz[rson[p] % mod]) % mod;
}
void rot(LL &p,LL start,LL end,LL l,LL r){
    if(!p)p=++cnt;
    if(end<l||start>r)return;
    if(start>=l&&end<=r){
        swp(sumx[p],sumy[p]);
        swp(sumy[p],sumz[p]);
        lazyro[p]++;
        lazyro[p]=lazyro[p]%3;
        return;
    }
    if(lazyx[p]||lazyy[p]||lazyz[p]||lazymul[p]||lazyro[p])pushdown(p,start,end);
    LL mid=(start+end)>>1;
    if(mid>=l)rot(lson[p],start,mid,l,r);
    if(mid<r)rot(rson[p],mid+1,end,l,r);
    sumx[p]=(sumx[lson[p]]%mod+sumx[rson[p]%mod])%mod;
    sumy[p]=(sumy[lson[p]]%mod+sumy[rson[p]%mod])%mod;
    sumz[p]=(sumz[lson[p]]%mod+sumz[rson[p]%mod])%mod;

}
void query(LL &p,LL start,LL end,LL l,LL r,LL &a,LL &b,LL &c){
    if(!p)p=cnt++;
    if(end<l||start>r)return;
    if(start>=l&&end<=r){
        a=(a+sumx[p]%mod)%mod;
        b=(b+sumy[p]%mod)%mod;
        c=(c+sumz[p]%mod)%mod;
        return;
    }
    if (lazyx[p] || lazyy[p] || lazyz[p] || lazymul[p] || lazyro[p]) pushdown(p, start, end);
	LL mid = (start + end) >> 1;
	if (mid >= l)
		query(lson[p], start, mid, l, r, a, b, c);
	if (mid < r)
		query(rson[p], mid + 1, end, l, r, a, b, c);
}
int main(){
    LL i,j;
    cin>>n>>m;
    LL start=1,end=n,root;
    root=++cnt;
    for(i=1;i<=m;i++){
        LL type;
        cin>>type;
        LL l,r;
        cin>>l>>r;
        if(type==1){
            LL a,b,c;
            cin>>a>>b>>c;
            add(root,start,end,l,r,a,b,c);
        }
        else if(type==2){
            LL k;
            cin>>k;
            mul(root,start,end,l,r,k);

        }
        else if(type==3){
            rot(root,start,end,l,r);
        }
        else if(type==4){
            LL f=0,d=0,e=0;
            query(root,start,end,l,r,f,d,e);
            cout<<((f%mod)*(f%mod)%mod+(d%mod)*(d%mod)%mod+(e%mod)*(e%mod)%mod)<<endl;

        }
    }
    return 0;
}


```