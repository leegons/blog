# Substring with Concatenation of All Words

> link: [https://leetcode.com/problems/substring-with-concatenation-of-all-words/](https://leetcode.com/problems/substring-with-concatenation-of-all-words/)
>
> You are given a string, **s**, and a list of words, **words**, that are all of the same length. Find all starting indices of substring\(s\) in **s **that is a concatenation of each word in **words **exactly once and without any intervening characters.
>
> For example, given:  
> **s**:`"barfoothefoobarman"`  
> **words**:`["foo", "bar"]`
>
> You should return the indices:`[0,9]`.  
> \(order does not matter\).

题意：从字符串s中找到一个子字符串，包含指定数组words中所有的字符串**一次**，同时不能出现多余的字符。words中所有的字符串都是等长的。


输入：

"bafoothefoobarmanabfooo"

\["foo","bar"\]

输出：

\[8\]

