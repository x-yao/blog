title: leetCode 记录（2）
---
## leetCode 记录（2）

### Longest Substring Without Repeating Characters
> [Longest Substring Without Repeating Characters](https://leetcode.com/problems/longest-substring-without-repeating-characters)

### 初始思路

* O(n^2)遍历数组
* 把不重复的字符存入数组
* 遇到重复的break
* 遍历完成获取最后数组的长度

### 遇到问题

* 时间复杂度问题

### 结果
	function lengthOfLongestSubstring(s) {
	    const map = {};
	    var left = 0;
	    
	    return s.split('').reduce((max, v, i) => {
	        left = map[v] >= left ? map[v] + 1 : left;
	        map[v] = i;
	        return Math.max(max, i - left + 1);
	    }, 0);
	}

### 思考
* 上述代码核心是通过扫描求出最长解
	* 首先map作为储存每个字符的index位置的变量，left作为最长解的左边位置变量
	* 其次开始扫描整个字符串，给每个字符存储最后出现的位置在map中
	* 然后如果出现重复的字符，更新left位置为改字符上次出现的位置的下一个
	* 每次对比已经存储的最大长度与当前的不重复长度

### TODO

* 理解上述代码