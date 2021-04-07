# coding-experience-for-leetcode

python input EOFError:python3 stipulate the input function just can be called in father process

Bitmap:！！

<--------2021-04-06-------->

洗牌算法：

有一个大小为100的数组，里面的元素是1到100，怎样随机从里面选择50个数。

如果采用暴力算法，那么到后面选出来的随机数可能会在前面就已经出现过，使用set去重也会造成很大的时间复杂度开销。

洗牌算法的核心思想就是，对下标进行随机选择，按照顺序交换当前下标和随机下标里面的数字。这样即使随机到曾经出现的数也不会造成什么本次操作失效。

上述方法每一位只能出现99个数，虽然概率相等但是不够随机。可以将对下标的随机范围包括自己，这样每个数出现在某一位上的概率都是1/100。

在常数时间内完成向数组插入、删除、随机选取一个数的操作。

核心办法：通过hash函数完成元素值到下标的映射，这样在检索就可以通过hash映射在常数时间内找到对应值的索引，完成查找这一动作。

随机选取一个数可以直接对数组操作，通过随机下标的方式选出随机数。

在C++中，unordered_map采用STL标准库提供的hash函数进行map映射，该hash函数只适用于基本数据类型（包括string类型），并不能使用在自定义结构体或者类上面。容器模板中还表明了相等比较规则，改规则仅支持可以使用==符号进行比较的数据类型。

unordered_map类型的初始化为：

std::unordered_map<std::int,std::int> M_map;
成员函数：

begin()和end()都是返回迭代器；

empty()，若容器为空，返回true；否则false；

size()，返回容器存有的键值对的个数；

可以通过value=map[key]直接通过key值访问value值；

find()，查找以key为键的键值对，如果找到返回迭代器；否则==end()；

insert()，插入新的键值对；emplace()，向容器添加新的键值对，效率比前者高;

erase()，删除指定键值对；

clear()，清空容器；

swap()，交换两个容器的键值对，保证容器的类型完全相同。

设计一个支持在平均 时间复杂度 O(1) 下，执行以下操作的数据结构。

insert(val)：当元素 val 不存在时，向集合中插入该项。
remove(val)：元素 val 存在时，从集合中移除该项。
getRandom：随机返回现有集合中的一项。每个元素应该有相同的概率被返回。

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/insert-delete-getrandom-o1

通过代码如下：

/** 执行用时：44 ms, 在所有 C++ 提交中击败了81.33%的用户
    内存消耗：22.1 MB, 在所有 C++ 提交中击败了52.10%的用户 */

#include<unordered_map>
#include<vector>
using namespace std;

class RandomizedSet {
public:
    /** Initialize your data structure here. */
    RandomizedSet() {}
    
    /** Inserts a value to the set. Returns true if the set did not already contain the specified element. */
    bool insert(int val) {
        if(mMap.find(val) == mMap.end()){
            mMap.emplace(val,mVec.size());
            mVec.push_back(val);
            return true;
        }
        return false;
    }
    
    /** Removes a value from the set. Returns true if the set contained the specified element. */
    bool remove(int val) {
        if(mMap.find(val) == mMap.end()){
            return false;
        }
        //可以直接删除映射值，将最后一个值赋给当前pos值，并且更新最后一个值的映射索引
        //原值可以不处理，因为需要删去，内容并不重要！
        int pos = mMap[val];
        //需要考虑本身就是最后一个值的情况
        mVec[pos] = mVec[mVec.size()-1];
        mMap[mVec[pos]] = pos;
        mVec.pop_back();
        mMap.erase(val);
        return true;
    }
    
    /** Get a random element from the set. */
    int getRandom() {
        return mVec[rand()%mVec.size()];
    }

    private:
      unordered_map<int,int> mMap;
      vector<int> mVec;
};



