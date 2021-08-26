# 最长回文子串

## 问题说明
给你一个字符串 s，找到 s 中最长的回文子串。  

## 问题解析
其实想要找到这个问题的解答，经过思考，至少要做到两点：  
1. 找到一堆回文子串
2. 在这些回文子串中找到最长的
那么，我们一起来思考一下第一个问题，如何辨别回文串？  
### 回文串分析
回文串是一种文字排列上特别有意思的一种组合，其呈现一种空间上的轴对称，如前段时间大火的电影`tenet`，即第[start + n]位和第[end - n]位的字符相同，有一种对称的美感。  
如何去构建一个回文？有一个最简单的方法，从递推来看，`tenet`是在对称轴在n的位置开始，不断的在n的左右添加相同的字符，即`n->ene->tenet`，没添加一次，就形成了一个新的回文串，如果开始的时候对称轴什么都没有就可以视为在什么都没有的左右添加字符串，如`''->(e''e) = (ee)->teet`。
那么我们反过来看，每一个回文串，其实都是前一个回文子串的左右添加一组相同的字符，那么，只要这个目标串的回文子串都是由回文子串组成的，那么他就是一个回文串。  
即状态转移方程为：  
`P(i, j) = P(i + 1, j - 1) & (S[i] == S[j])`

## c语言实现
```c
char * longestPalindrome(char * s){
    int length = strlen(s);
    int maxSize = 0, start, end, i, j;
    for(i = 0; i < length; i++) {
        for(j = length - 1; j > i; j--) { // 查找从i开始的第一个回文串
            if(checkPalindrome(s, i, j)) {
                if((j - i + 1) > maxSize) {
                    maxSize = (j - i + 1);
                    start = i;
                    end = j;
                    break;
                }
            }
        }
    }
    s[end + 1]='\0';    // end结尾

    return &s[start];   // 从start开始
}

// 递归查询是否回文串
int checkPalindrome(char *s , int i, int j) {
    if(i < j || i == j) {
        return 1;
    }
    if(s[i] != s[j]) {
        return 0;
    }
    checkPalindrome(s, (i + 1), (j - 1));
    return 0;
}

// 检测字符串长度
int strLength(char *s) {
    int i = 0;
    while(true) {
        if(s[i] == '\0') {
            break;
        }
        i++;
    }
    return i;
}
```