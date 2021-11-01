# diff程序算法

## diff程序作用
对于两个不同的文本，获取其不同之处

## diff程序核心
diff程序的建立基础是，LCS（最长公共子序列）算法。  
.举例说明吧，一个字符串“FallenOrc”,“FaOrc"是一个subsequence，"Flr"也是一个subsequence,因为他们每个字符在"FallenOrc"都有而且出现的次序一致，不要求字符连续出现，而"FrOc"就不是subsequnece了，因为"O"跑到"r"前面去了。所谓Common Subsequence,就是两个字符串共同的subsequence，比如对字符串“FallenOrc"和“SegmentFault",“Fa"就是一个Common Subsequence。Longest Commone Subsequence，就是两个字符串最大的Commone Subsequence。可以想象，如果获得了两个字符串的LCS，也就可以进一步通过比较知道这两个字符串有哪些差异。