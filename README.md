# Cpp2
## 9.11
> 对6种创建和初始化vector对象的方法，每一种都给出一个实例。解释每个vector包含什么值。
```CPP
vector<int> vec;  // 0
vector<int> vec(10);  // 0
vector<int> vec(10,1); // 1
vector<int> vec{1,2,3,4,5}; //1, 2, 3, 4, 5
vector<int> vec(other_vec); // 与 other_vec 一样
vector<int> vec(other_vec.begin(), other_vec.end());  // 与 other_vec 一样
```
## 9.20
> 编写程序，从一个list<int>拷贝元素到两个deque中。值为偶数的所有元素都拷贝到一个deque中，而奇数值元素都拷贝到另一个deque中。
```CPP
/*Copyright [2019] <Copyright MrM>
*/
#include <iostream>
#include <deque>
#include <list>
int main()
{
std::list<int> list1{1, 2, 3, 4, 5, 6, 7, 8, 9, 10};
std::deque<int> deque_jishu, deque_oushu;
for(auto i : list1)
  if(i % 2 == 0)
      deque_oushu.push_back(i);
  else
      deque_jishu.push_back(i);
for(auto i : deque_oushu)
  std::cout << i << ' ';
std::cout << '\n';
for(auto i : deque_jishu)
  std::cout << i << ' ';
return 0;
}
```
	
## 9.29
> 假定vec包含25个元素，那么vec.resize(100)会做什么？如果接下来调用vec.resize(10)会做什么？
* 将75个值为0的元素添加到vec的末尾
* 从vec的末尾删除90个元素

## 9.43
> 编写一个函数，接受三个string参数是s、oldVal 和newVal。使用迭代器及insert和erase函数将s中所有oldVal替换为newVal。测试你的程序，用它替换通用的简写形式，如，将"tho"替换为"though",将"thru"替换为"through"。
```CPP
/*Copyright [2019] <Copyright MrM>
*/
#include <iostream>
#include <string>
void f(std::string& s, std::string oldVal, std::string newVal)
{
    auto x = s.begin();
    while(x != s.end() - oldVal.size())
    {
        if(std::string(x, x + oldVal.size()) == oldVal)
        {
            s.erase(x, x + oldVal.size());
            s.insert(x, newVal.begin(), newVal.end());
            x += newVal.size();
        }
        else
            x++;
    }
}

int main()
{
    std::string s("tho thru");
    f(s, "tho", "though");
    std::cout << s;
    std::cout << '\n';
    f(s, "thru", "through");
    std::cout << s;
    return 0;
}
```
## 9.52
> 使用stack处理括号化的表达式。当你看到一个左括号，将其记录下来。当你在一个左括号之后看到一个右括号，从stack中pop对象，直至遇到左括号，将左括号也一起弹出栈。然后将一个值（括号内的运算结果）push到栈中，表示一个括号化的（子）表达式已经处理完毕，被其运算结果所替代。
```CPP
/*Copyright [2019] <Copyright MrM>
*/
#include<iostream>
#include<stack>
#include<string>

std::string ji_suan(std::string l, std::string op, std::string r)
{
	std::string s;
	if (op == "-")
		s = std::to_string(stoi(l) - stoi(r));
    else if (op == "+")
		s = std::to_string(stoi(l) + stoi(r));
    else if (op == "*")
		s = std::to_string(stoi(l) * stoi(r));
    else if (op == "/")
		s = std::to_string(stoi(l) / stoi(r));
	return s;
}

int main()
{
    std::string s("1+2+3*(3*3)");
    std::stack<std::string> stack1;
    for(auto i = s.begin(); i != s.end(); )
    {
        if(*i == '(')
        {
            stack1.push(std::string(1, *i));
            i++;
            while(*i != ')')
            {
                stack1.push(std::string(1, *i));
                i++;
            }
        }
        else if(*i == ')')
        {
            auto r = stack1.top();
            stack1.pop();
            auto op = stack1.top();
            stack1.pop();
            auto l = stack1.top();
            stack1.pop();
            stack1.push(ji_suan(l, op, r));
            i++;
        }
        else
            i++;
    }
    std::cout << stack1.top();
    stack1.pop();
    return 0;
}
```
## 10.3
> 用accumulate求一个 vector 中元素之和。
```cpp
/*Copyright [2019] <Copyright MrM>
*/
#include <iostream>
#include <vector>
#include <numeric>
int main() {
    std::vector<int> v = {1, 2, 3, 4, 5, 6, 7, 8, 9, 10};
    std::cout << std::accumulate(v.begin(), v.end(), 0) << std::endl;
    return 0;
}
```
## 10.15
> 编写一个 lambda ，捕获它所在函数的 int，并接受一个 int参数。lambda 应该返回捕获的 int 和 int 参数的和。
```cpp
int a = 1;
auto f = [a](int b) {return a + b;};
```

## 10.34
> 使用 reverse_iterator 逆序打印一个vector。
```cpp
/*Copyright [2019] <Copyright MrM>
*/
#include <iostream>
#include <vector>
int main() {
    std::vector<int> v = {1, 2, 3, 4, 5};
    for(auto p = v.crbegin(); p != v.crend(); p++)
        std::cout << *p << ' ';
    return 0;
}
```

## 10.42
> 使用 list 代替 vector 重新实现10.2.3节中的去除重复单词的程序。
```cpp
/*Copyright [2019] <Copyright MrM>
*/
#include <iostream>
#include <string>
#include <list>
void elimDups(std::list<std::string> &words)
{
    words.sort();
    words.unique();
}
int main() {
    std::list<std::string> words = {"the", "quick", "red", "fox", "jumps", "over", "the", "slow", "red", "turtle"};
    elimDups(words);
    for(auto i : words)
        std::cout << i << ' ';
    return 0;
}
```

## 11.12
> 编写程序，读入string和int的序列，将每个string和int存入一个pair 中，pair保存在一个vector中。
```cpp
/*Copyright [2019] <Copyright MrM>
*/
#include <vector>
#include <utility>
#include <string>
#include <iostream>
int main()
{
	std::vector<std::pair<std::string, int>> v;
	std::string s;
	int x;
	while(std::cin >> s >> x)
        v.push_back(std::pair<std::string, int>(s, x));
    for(auto i : v)
        std::cout << i.first << ":" << i.second << std::endl;
}
```
## 11.17
> 假定 c 是一个string的multiset，v 是一个string 的vector，解释下面的调用。指出每个调用是否合法：
```cpp
copy(v.begin(), v.end(), inserter(c, c.end()));
copy(v.begin(), v.end(), back_inserter(c));
copy(c.begin(), c.end(), inserter(v, v.end()));
copy(c.begin(), c.end(), back_inserter(v));
```
copy(v.begin(), v.end(), back_inserter(c));这个调用不合法,因为multiset 没有 push_back 方法, 所以也不能使用back_inserter。
其他三个都合法。

## 11.38
> 用 unordered_map 重写单词计数程序和单词转换程序。
```cpp
/*Copyright [2019] <Copyright MrM>
*/
#include<iostream>
#include<string>
#include <fstream>
#include<sstream>
#include<unordered_map>

void Count(std::unordered_map<std::string, size_t> word_count)
{
    std::string word;
    while(std::cin >> word)
        word_count[word]++;
    for(auto i : word_count)
        std::cout << i.first << "出现了" << i.second << "次" << std::endl;
}

std::unordered_map<std::string, std::string> buildMap(std::ifstream &map_file)
{
    std::unordered_map<std::string, std::string> trans_map;
    std::string key;
    std::string value;
    while(map_file >> key && getline(map_file, value))//map_file输入以" "为结束标志，会储存到后续输入的value中，所以后续要用value.substr(1)
        if(value.size() > 1)
            trans_map[key] = value.substr(1);
        else
            throw std::runtime_error("no rule for" + key);
    return trans_map;
}

const std::string & transform(const std::string &s, const std::unordered_map<std::string, std::string> &m)
{
    auto map_it = m.find(s);
    if(map_it != m.cend())
        return map_it -> second;
    else
        return s;
}

void trans(std::ifstream &map_file, std::ifstream &input)
{
    auto trans_map = buildMap(map_file);
    std::string text;
    while (getline(input, text))
    {
        std::istringstream stream(text);
        std::string word;
        bool firstword = true;
        while(stream >> word)
        {
            if (firstword)
                firstword = false;
            else
                std::cout << " ";
            std::cout << transform(word, trans_map);
        }
        std::cout << std::endl;
    }
}

int main()
{
    std::unordered_map<std::string, size_t> word_count;
    std::ifstream map_file("E:\\cpp\\1.txt");
    std::ifstream input("E:\\cpp\\2.txt");
    //Count(word_count);
    trans(map_file, input);
    return 0;
}
```

