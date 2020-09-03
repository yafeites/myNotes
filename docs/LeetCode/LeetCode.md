## 刷题记录
####  剑指offer

[二叉搜索树第k个结点](https://leetcode-cn.com/problems/kth-smallest-element-in-a-bst/)
先遍历左子树不存在遍历自己再遍历右子树

[数据流中的中位数](https://leetcode-cn.com/problems/kth-smallest-element-in-a-bst/)
制造一个大根堆和一个小根堆,当前为偶数时向小根堆里插入并从小根堆提一个到大根堆,奇数则相反,当前偶数则为大根堆的顶部元素,奇数为大根和小根的顶部元素平均值
[滑动窗口最大值](https://leetcode-cn.com/problems/sliding-window-maximum/)
制造一个queue存放索引,从first到last递减,并且保证当前的first在当前的窗口中

[矩阵中的路径](https://leetcode-cn.com/problems/ju-zhen-zhong-de-lu-jing-lcof/solution/)
dfs+回溯

[机器人的运动范围](https://leetcode-cn.com/problems/ji-qi-ren-de-yun-dong-fan-wei-lcof/)
bfs|dfs

[剪绳子](https://leetcode-cn.com/problems/jian-sheng-zi-lcof/)
动态规划
[两数之和](https://leetcode-cn.com/problems/two-sum/)
map
[两数相加](https://leetcode-cn.com/problems/add-two-numbers/)
链表直接操作就行

[无重复字符的最长子串](https://leetcode-cn.com/problems/longest-substring-without-repeating-characters/)
map

[最长回文子串](https://leetcode-cn.com/problems/longest-palindromic-substring/)
直接写/动态规划

[z字形变换](https://leetcode-cn.com/problems/zigzag-conversion/)

先按RowNum创造行,设置一个boolean godown 到最后一行反转,每次添加一个元素直到所有元素添加到对应行再一起append

[整数反转](https://leetcode-cn.com/problems/reverse-integer/)
注意溢出即可

[字符串转换整数](https://leetcode-cn.com/problems/string-to-integer-atoi/)
硬写

[盛最多水的容器](https://leetcode-cn.com/problems/container-with-most-water/)
对比左右的边把小的一边内缩

[整数转罗马数字](https://leetcode-cn.com/problems/integer-to-roman/)
把1000 900 500 到1 硬编码即可

[罗马数字转整数](https://leetcode-cn.com/problems/roman-to-integer/)
从头开始遍历,与后一个数字相比较,如果等级比后面的大就加，否则减

[最长公共前缀](https://leetcode-cn.com/problems/longest-common-prefix/)
一个一个比较就行

[三数之和](https://leetcode-cn.com/problems/3sum/)

排序加双指针

[最接近的三数之和](https://leetcode-cn.com/problems/3sum-closest/submissions/)
排序加双指针

[电话号码的字母组合](https://leetcode-cn.com/problems/letter-combinations-of-a-phone-number/)
递归

[四数之和](https://leetcode-cn.com/problems/4sum/)
参考三数

[ 删除链表的倒数第N个节点
](https://leetcode-cn.com/problems/remove-nth-node-from-end-of-list/)
设置哨兵结点,fast结点先走n-1步,n为倒数第几个结点

[有效的括号](https://leetcode-cn.com/problems/valid-parentheses/)
栈

[合并两个有序数组](https://leetcode-cn.com/problems/merge-two-sorted-lists/)
就直接合并?

[括号生成](https://leetcode-cn.com/problems/generate-parentheses/)
递归+计算左括号和右括号的数量,左括号不能小于右括号

[合并K个有序序列](https://leetcode-cn.com/problems/merge-k-sorted-lists/)
归并

[组合总和](https://leetcode-cn.com/problems/combination-sum/)
回溯+剪枝
[两两交换链表中的节点](https://leetcode-cn.com/problems/swap-nodes-in-pairs/)
 正常交换

 [ 删除排序数组中的重复项](https://leetcode-cn.com/problems/remove-duplicates-from-sorted-array/)
 双指针

 [实现 strStr()](https://leetcode-cn.com/problems/implement-strstr/)
 kmp打扰了

[下一个排列](https://leetcode-cn.com/problems/next-permutation/)
从右到左找到第一个顺序对ab,从右边找到比a小的最大的数字与其交换,再将原本a位置后的数字逆序

[搜索旋转排序数组](https://leetcode-cn.com/problems/search-in-rotated-sorted-array/)
比较中间和left和right的关系,如果target处于有序状态内则移动相应的left和right否则相反

[在排序数组中查找元素的第一个和最后一个位置](https://leetcode-cn.com/problems/find-first-and-last-position-of-element-in-sorted-array/)
两次二分

[有效的数独](https://leetcode-cn.com/problems/valid-sudoku/)
建立行列小方格三个map即可

[外观数列](https://leetcode-cn.com/submissions/detail/30423509/)
正常做

[确实的整数](https://leetcode-cn.com/problems/first-missing-positive/)
置换
[接雨水](https://leetcode-cn.com/problems/trapping-rain-water/)
维持左边最大值和右边最大值,每次从小的一侧判断如果比同侧最大值小则增加雨水量
[通配符匹配](https://leetcode-cn.com/problems/wildcard-matching/)
参数 string s pa'ttern p 可以将p作为行 s作为列 动态规划即可

[全排列](https://leetcode-cn.com/problems/permutations/)
递归+回溯

[旋转图像](https://leetcode-cn.com/problems/rotate-image/)
先转置矩阵,再反转

[字母异位词分组](https://leetcode-cn.com/problems/group-anagrams/)
计算字符计数个数

[Pow(x,n)](https://leetcode-cn.com/problems/powx-n/solution/)

快速幂,res=1,n取余等于1时res乘x,n/=2

[最大子序和](https://leetcode-cn.com/problems/maximum-subarray/)
动态规划

[螺旋矩阵](https://leetcode-cn.com/problems/spiral-matrix/)
选择左上角和右下角两点按顺序遍历,遍历完成向聚合方向移动,注意两点的行和列相同的情况

[跳跃游戏](https://leetcode-cn.com/problems/jump-game/)
维持一个max是否大于目前的位置,大于则继续移动

[合并区间](https://leetcode-cn.com/problems/merge-intervals/)
先按前缀sort再遍历 i+1和i比较是否可以合并

[不同路径](https://leetcode-cn.com/problems/unique-paths/)
动态规划

[加一](https://leetcode-cn.com/problems/plus-one/)
从数组最后一位向前开始去加,加到不是0为止,到第一位就再创建一个新的数组
[x的平方根](https://leetcode-cn.com/problems/sqrtx/)
二分

[爬楼梯](https://leetcode-cn.com/problems/climbing-stairs/)
动态规划

[矩阵置0](https://leetcode-cn.com/problems/set-matrix-zeroes/)
将每一行的第一个数字和每一列的第一个数字当作此行和此列的标识符,[0][0]当作行标识符,维持遍历temp当做第一列的标识符

[颜色分类](https://leetcode-cn.com/problems/sort-colors/)
荷兰国旗,维持left,right,cur三个变量

[最小覆盖子串](https://leetcode-cn.com/problems/minimum-window-substring/)
滑动窗口,遍历被覆盖子串每个字母的出现次数,被覆盖串出现不同的字母有n个,维持left,right,遍历覆盖串right++,当被覆盖串每个字母出现次数达到时left++,直到出现有字幕不符合个数要求

[子集](https://leetcode-cn.com/problems/subsets/submissions/)
回溯


[单词搜索](https://leetcode-cn.com/problems/word-search/)
dfs

[柱状图中最大的矩形](https://leetcode-cn.com/problems/word-search/)
  维持一个单调栈，每次一旦发现有小于栈顶元素，则计算目前栈顶元素到栈顶前一个元素之间的宽乘栈顶高判断是否有最大值
        第一个栈顶元素必是当前元素的前一个元素）
        对于一直没有出栈的元素，直接判断前一个元素和数组长度差值乘高度即可能是max
      
[合并两个有序数组](https://leetcode-cn.com/problems/merge-sorted-array/)
正常做
[解码方法](https://leetcode-cn.com/problems/decode-ways/)
动态规划
[二叉树的中序遍历](https://leetcode-cn.com/problems/binary-tree-inorder-traversal/)
root入栈,root=root.left，当root 没有左子节点时,root=root.right

[验证搜索二叉树](https://leetcode-cn.com/problems/validate-binary-search-tree/)
递归 func(root,min,max)从根节点向下遍历

[LeetCode101对称二叉树](https://leetcode-cn.com/problems/symmetric-tree/)
递归

[LeetCode102层序遍历](https://leetcode-cn.com/problems/binary-tree-level-order-traversal/)
每一层放Quue中

[LeetCode103锯齿型打印](https://leetcode-cn.com/problems/binary-tree-zigzag-level-order-traversal/)
偶数层逆序打印即可

[LeetCode104 二叉树的最大深度](https://leetcode-cn.com/problems/maximum-depth-of-binary-tree)
递归取得最大值向上传

[LeetCode105 前序+中序还原二叉树](https://leetcode-cn.com/problems/construct-binary-tree-from-preorder-and-inorder-traversal/)
剑指Offer原题

[LeetCode108将有序数组转换成二叉搜索树](https://leetcode-cn.com/problems/convert-sorted-array-to-binary-search-tree/)
遍历,取mid结点为root

[LeetCode116填充每个结点的下一个右侧结点](https://leetcode-cn.com/problems/populating-next-right-pointers-in-each-node/submissions/)
递归 从左到右开始遍历

[LeetCode118杨辉三角](https://leetcode-cn.com/problems/pascals-triangle/)
递归 一层一层做

[LeetCode121买卖股票最佳实际](https://leetcode-cn.com/problems/best-time-to-buy-and-sell-stock/)
维持一个最小值和一个最大值,差值就是买卖时机

[LeetCode122买卖股票最佳实际二](https://leetcode-cn.com/problems/best-time-to-buy-and-sell-stock-ii/)
sum=每个i大于i-1时的差值之和

[二叉树中的最大路径和](https://leetcode-cn.com/problems/binary-tree-maximum-path-sum/)
维持个最大值,每个结点向上传左子结点的最大值和右子结点的最大值中最大的那一个加上本结点或者仅仅为本结点

[验证回文串](https://leetcode-cn.com/problems/valid-palindrome/)
预处理+回文判断

[单词接龙](https://leetcode-cn.com/problems/word-ladder/)
广度优先+visited数组判断是否访问过