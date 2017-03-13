title: Leetcode 记录
categories: 算法
---
## Leetcode 记录

### K-diff Pairs in an Array

> [K-diff Pairs in an Array](https://leetcode.com/problems/k-diff-pairs-in-an-array/?tab=Description)

### 初始思路

* O(n^2)遍历数组
* 相减绝对值对比k
* 算出个数

<!-- more -->
### 遇到问题

* 遍历数量多
	* （解决）先排序，然后遍历出第一个结果则break
* 会出现重复统计
	* （解决）使用临时对象储存已经遍历出来的配对
	* （新问题）会出现(0,1),(0,-1)这样2种情况的配对，单个对象存储是被覆盖
	* （解决）临时两个对象存放同一键值2个状态

### 结果
	/**
	 * @param {number[]} nums
	 * @param {number} k
	 * @return {number}
	 */
	var findPairs = function(nums, k) {
	    var ap = {},
	        ad = {}
	    var p = 0;
	    var sort = nums.sort(function(a,b){
	            return b-a});
	    for(var i = 0;i<sort.length;i++){
	        for(var n = i+1;n<sort.length;n++){
	            var p1 = sort[i],
	                p2 = sort[n]
	            if(Math.abs(p1-p2) === k){
	                if(ap[p1] == p2 || ad[p1] == p2 || ap[p2] == p1 || ad[p2] == p1){
	                    break;
	                }
	                if( ((ap[p1] !== undefined) && (ap[p1] != p2)) || ((ap[p2] !== undefined) && (ap[p2] != p1) )){
	                    ad[p1] = p2;
	                    ad[p2] = p1
	                }else{
	                    ap[p1] = p2;
	                    ap[p2] = p1 
	                }
	                p++
	            }
	        }
	    }
	    return p
	};

### 思考
* 复杂代码理解能力不够，无法完整预演算，需要debug调试。
* 思考问题不够仔细很多问题点没思考出来，运算出问题才去找。
* 复杂度问题没有解决，依旧不是最优处理，需要继续思考。

### TODO

* 训练基本排序，等数据操作算法
* 加强手写代码能力与理解能力
* 减少debug依赖