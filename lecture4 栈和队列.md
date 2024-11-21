### 栈

#### 进制转换

```c++
//十进制转八进制
int main(){
    int num;
    std::cin>>num;
    
    std::stack<int> temp;
    while(num){
        temp.push(num%8);
        num/=8;
    }
    
    while(!temp.isEmpty()){
        cout<<temp.pop();
    }
    
    return 0;
}
```

#### 括号匹配

```c++
bool bracketMatching(char* s) {
    int len = strlen(s);
    std::stack<char> LBracket;
    std::unordered_map<char, char> pair = {{'{', '}'}, {'[', ']'}, {'(', ')'}};

    for (int i = 0; i < len; i++) {
        char ch = s[i];
        
        // 如果是左括号，则入栈
        if (pair.find(ch) != pair.end()) {
            LBracket.push(ch);
        }
        // 如果是右括号，则进行匹配
        else if (ch == '}' || ch == ']' || ch == ')') {
            if (LBracket.empty()) return false; // 右括号多余
            char top = LBracket.top();
            LBracket.pop();
            if (pair[top] != ch) return false; // 括号不匹配
        }
    }

    // 最终栈应为空，表示所有括号都有匹配
    return LBracket.empty();
}
```

#### 表达式求值（中缀、后缀表达式）

##### 后缀表达式

```c++
int calculate(char* s){
    int len=strlen(s);
    stack<int> numTemp;
    
    for(int i=0;i<len;i++){
        //操作数入栈
        if(s[i]>='0'&&s[i]<='9')
            numTemp.push(s[i]-48);
        
        //遇到操作符，从栈中弹出两个数，运算后，压回栈
        else{
            int a=numTemp.top();
            numTemp.pop();
            int b=numTemp.top();
            numtemp.pop();
            
            int res;
            if(s[i]=='+')	res=a+b;
            else if(s[i]=='-') res=a-b;
            else if(s[i]=='*') res=a*b;
            else res=a/b;
            numTemp.push(res);
        }
    }
    
    return numTemp.top();
}
```

##### 中缀表达式（先转换为后缀表达式，再求值）

```c++
// 判断操作符的优先级
int precedence(char op) {
    if (op == '+' || op == '-') return 1;
    if (op == '*' || op == '/') return 2;
    return 0;  // 对于其他操作符（如括号），不需要考虑优先级
}

// 中缀转后缀
std::string infixToPostfix(const std::string& infix) {
    std::stack<char> operators;  // 用栈存储操作符
    std::string postfix;          // 存储后缀表达式
    for (size_t i = 0; i < infix.length(); i++) {
        char c = infix[i];

        // 如果是数字或字母，直接添加到后缀表达式中
        if (isdigit(c)) {
            postfix += c;
        }
        // 如果是左括号，入栈
        else if (c == '(') {
            operators.push(c);
        }
        // 如果是右括号，弹出操作符直到遇到左括号
        else if (c == ')') {
            while (!operators.empty() && operators.top() != '(') {
                postfix += operators.top();
                operators.pop();
            }
            operators.pop();  // 弹出左括号
        }
        // 如果是操作符，处理优先级
        else if (c == '+' || c == '-' || c == '*' || c == '/') {
            // 弹出栈中优先级大于等于当前操作符的操作符
            while (!operators.empty() && precedence(operators.top()) >= precedence(c)) {
                postfix += operators.top();
                operators.pop();
            }
            // 当前操作符入栈
            operators.push(c);
        }
    }

    // 将栈中剩余的操作符弹出并添加到后缀表达式
    while (!operators.empty()) {
        postfix += operators.top();
        operators.pop();
    }

    return postfix;
}
```

##### 栈扩容

###### 容量倍增策略

###### 容量递增策略（从初始容量为0的栈，采取容量递增策略，每次容量增加d）

##### 栈混洗

```c++
bool check(int a[],int b[],int n){//a:入栈序列	b:出栈序列
    stack<int> temp;
    int pa=0;
    
    for(int pb=0;pb<n;pb++){
        while(temp.empty()||temp.top()!=b[pb]){
            if(pa<n) temp.push(a[pa++]);
            else return false;
        }
        temp.pop();
    }
    
    return true;
}
```

##### 快速幂(递归，内存中实际上是栈的CAL和RET)

```c++
double pow(double a,long long n){
    if(a==1||n==0) return 1;
    
    double b=pow(a,n/2);
    double res=b*b;
    
    return (n%2)?res*a:res;
}
```

#### 队列

##### 循环队列

```c++
初始	front=0,rear=0
队空	front==rear
队满	(rear+1)%MaxSize==front
队中元素个数	(rear-front+MaxSize)%MaxSize
    
```



