# 前两章的内容
## STL巡礼
以一个简化版的UNIX sort为例，它需要三个步骤：从标准输入读入数据并断行；排序；把排序好的写到标准输出。
````
int main(){
    vector<string> V;
    string tmp;
    while(getline(cin,tmp))
    V.push_back(tmp);
    sort(V.begin(),V.end());
    copy(V.begin(),V.end(),ostream_iterator<string>(cout,"\n));
}
````
从中我们可以看出来，几个使用STL最重要的观念：
1. 所谓使用STL，就是去扩充它。
2. STL的算法和容器分离
3. 可扩充，可定制
4. 抽象化不代表效率低
## 几种迭代器
**iterator之所以重要，是因为他是算法与数据结构的接口**
1. input_iterators
   特点：可以进行累加动作，也只能进行累加动作，用来指向某个对象，却不能提供任何更改对象的方法
2. output_iterators
   特点：具有只写性
3. forward_iterators
   特点：正如其名，只能前向移动，同样只能p++,但遵循强烈的同一性，即当且仅当两个指针指向同一个对象才能确保两个f_i相等
4. bidirectional_iterators
   特点：双向移动
5. random access_iterators
   特点：支持如p+=5或p-=5这种随机存取动作