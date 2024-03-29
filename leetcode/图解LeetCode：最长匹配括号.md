给定一个字符串，其中只包含左括号'(' 以及右括号 ')'，要求返回最长的匹配括号长度。

比如给定"(()"，那么我们的函数需要返回2，因为最长的匹配括号是"()";

比如给定")()())"，那么我们的函数需要返回4，因为最长的匹配括号是"()()";

如果在面试的时候遇到这个题目，你该如何解决呢？



#### 我的思考过程

首先我们要明确的就是什么样的字符串才是**匹配的括号**，其实很简单，那就是如果一个括号串中的左括号"("和右括号")"都能相互抵消掉，那么我们就说这是匹配的括号；

比如给定"())"，其中"()"能抵消掉，剩下的")"就没有对应的"("来抵消，这样的字符串就不满足要求；

对于"(()())"，其中中间的两个"()()"能抵消掉，最后剩下一个"()"也能抵消，这样的字符串就满足要求；

从上面的分析过程可以看到这其实可以用**栈**这种数据结构来进行判断，很简单，只要是"("就入栈，遇到右括号")"就去看一下当前的**栈顶**是不是"("，如果是，那么就出栈，否则继续入栈，最后判断一下如果栈为空，那么该字符串就满足要求，如图所示：

![1574475377945](https://mmbiz.qpic.cn/mmbiz_png/8g3rwJPmya1hmjicCIN19j8rbNxILeY40zdRhIvL5rnf6ibVpb0jWZMr7yWRdAzQ1OIDOdibvPCyYy1DKKsgceZ3w/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

这个分析可能有的同学觉得无聊，这不是很显而易见的吗，但就是这个显而易见的分析实际上就是该问题的解决方法。



#### 计算最长匹配括号

什么情况下才可以计算最长的匹配括号，答案很显然，那就是**出栈时**，因为在**出栈时我们能确定此时一定有一个匹配的括号产生**，那么接下来的问题就是如何计算这个匹配括号的长度。

如果你不知道这个问题的答案，那么你需要再仔细的看一看上图，如果你还看不出来，那么下图应该能帮到你：

![1574477947029](https://mmbiz.qpic.cn/mmbiz_png/8g3rwJPmya1hmjicCIN19j8rbNxILeY40Qak6pCk1YPC01q1jTYFicPeHvBiaUdm1M53OfqUsVF59K19KRhWULnLw/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)



从上面的分析我们可以看出**只要每次出栈的时候用当前的位置减去栈顶的位置就是本次形成的匹配字符串长度**，只需要在遍历时记录下最大的那个长度就可以了。

接下来我们用一个示例")((()())"来说明这个过程。



#### 图解计算过程

下图就是以上分析过程的图形化，以字符串为例")((()())"来说明：

![1574478065167](https://mmbiz.qpic.cn/mmbiz_png/8g3rwJPmya1hmjicCIN19j8rbNxILeY40SdSmuJgicvwFsbic2qp9wHnhM4kjfRWdO9FmU4Kj2MqzTrkX984InpVg/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)



有了这些分析，代码不要太简单。



#### 代码实现

```
int longestValidParentheses(string s) {
    int len = s.length();
    if (len == 0)
        return 0;
    int r = 0;
    stack<int>st;
    for(int i=0;i<len;i++){
        if (s[i] == '('){
            st.push(i);
        } else {
            if (st.empty() || s[st.top()] == ')') {
                st.push(i);
            } else {
                int top = st.top();
                st.pop();
                r = max(r, st.empty()? i+1 : i-st.top());
            }
        }
    }
    return r;
}
```

唯一需要注意的是在计算匹配括号长度时需要判断当前栈是否为空，如果为空，那么说明从当前位置到字符串开头都是匹配的。



#### 总结

从这个题目可以看出有时问题的答案就藏在问题本身的定义中，希望这个题目能让你对栈这种数据结构有一种新的认知，栈有十分有趣而且极其重要的性质，在后面的文章中我们还会多次见到栈，并能看到用栈可以使用很多十分精妙的算法。