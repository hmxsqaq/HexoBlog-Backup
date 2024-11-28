---
title: 「LeetCode」算法练习记录
tags:
  - C/C++
  - Algorithm
  - Note
categories:
  - Notes
  - Algorithm
description:
  - "这是一个个人的算法练习留档，记录了一些在LeetCode上做的题"
cover: 'https://hmxs-1315810738.cos.ap-shanghai.myqcloud.com/img/202309090059804.png'
abbrlink: 102
date: 2023-09-09 00:55:29
---

![leetcode](https://hmxs-1315810738.cos.ap-shanghai.myqcloud.com/img/202309090059804.png)

> **力扣个人主页：[https://leetcode.cn/u/nervous-i3elllb6/](https://leetcode.cn/u/nervous-i3elllb6/)**

---

## template：

```
## date

### title

#### 思路

#### 题解
```

---

## 230905

### [2605. 从两个数字数组里生成最小数字](https://leetcode.cn/problems/form-smallest-number-from-two-digit-arrays/)

#### 思路

- 哈希：通过哈希表筛出两个数组的重复数与最小数


#### 题解

```c++
// 哈希
class Solution {
public:
    int minNumber(vector<int>& nums1, vector<int>& nums2) {
        int result = 10;
        unordered_set<int> hash(nums1.begin(), nums1.end());
        for (int num : nums2)
        {
            if (hash.find(num) != hash.end())
                result = min(result, num);
        }
        if (result < 10)
            return result;
        int num1 = *min_element(nums1.begin(), nums1.end());
        int num2 = *min_element(nums2.begin(), nums2.end());
        result = num1 < num2 ? num1 * 10 + num2 : num2 * 10 + num1;
        return result;
    }
};
```

### [1. 两数之和](https://leetcode.cn/problems/two-sum/)

#### 思路

- 遍历爆搜：把所有两两组合全试一遍
- 哈希：由`x + y = target`得`target - x = y`，建立哈希表进行索引，遍历过程中，设遍历到的数为`x`，在哈希表中寻找`target - x`，如果找不到就把`x`加入哈希表

#### 题解

```c++
// 遍历爆搜
class Solution {
public:
    vector<int> twoSum(vector<int>& nums, int target) {
        for (int i = 0; i < nums.size(); ++i)
        {
            for (int j = i + 1; j < nums.size(); ++j)
            {
                if (nums[i] + nums[j] == target)
                {
                    return { i, j };
                }
            }
        }
        return vector<int>(NULL);
    }
};

// 哈希
class Solution {
public:
    vector<int> twoSum(vector<int>& nums, int target) {
        unordered_map<int, int> hash;
        for (int i = 0; i < nums.size(); ++i)
        {
            auto iterator = hash.find(target - nums[i]);
            if (iterator != hash.end())
            {
                return { iterator->second, i};
            }
            hash[nums[i]] = i;
        }
        return vector<int>(NULL);
    }
};
```

---

## 230906

### [2. 两数相加](https://leetcode.cn/problems/add-two-numbers/)

#### 思路

链表题

- 迭代模拟：模拟加法的过程，遍历即可，需要注意边界检查等细节，需要采用一个`dummy head`（傀儡头指针）对链表头进行标记
- 递归：`sum = l1 + l2 + carry_bit`，使用递归进行链表的递推

#### 题解

```c++
// 模拟
class Solution {
public:
    ListNode* addTwoNumbers(ListNode* l1, ListNode* l2) {
        ListNode* tail = nullptr;
        ListNode* head = nullptr;
        int carry_bit = 0;
        while (l1 || l2 )
        {
            int add1 = l1 ? l1->val : 0;
            int add2 = l2 ? l2->val : 0;
            int sum = add1 + add2 + carry_bit;
            carry_bit = sum / 10;
            if (head == nullptr)
            {
                tail = new ListNode(sum % 10);
                head = new ListNode(0, tail);
            }
            else
            {
                tail->next = new ListNode(sum % 10);
                tail = tail->next;
            }
            if (l1) l1 = l1->next;
            if (l2) l2 = l2->next;
        }
        if (carry_bit > 0)
        {
            tail->next = new ListNode(carry_bit);
        }
        return head->next;
    }
};

// 递归
class Solution {
public:
    ListNode* add(ListNode* l1, ListNode* l2, int carry)
    {
        if (l1 == nullptr && l2 == nullptr && carry == 0)
            return nullptr;
        int sum = (l1 ? l1->val : 0) + (l2 ? l2->val : 0) + carry;
        ListNode* node = new ListNode(sum % 10);
        node->next = add(l1 ? l1->next : nullptr, l2 ? l2->next : nullptr, sum / 10);
        return node;
    }

    ListNode* addTwoNumbers(ListNode* l1, ListNode* l2)
    {
        return add(l1, l2, 0);
    }
};
```

---

## 230908

### [3. 无重复字符的最长子串](https://leetcode.cn/problems/longest-substring-without-repeating-characters/)

#### 思路

- 滑动窗口：使用一左一右两个索引“指针”，创造一个“窗口”，窗口中的元素是符合要求的，使其在字符串/数组上移动，完成遍历

#### 题解

```c++
// 滑动窗口
class Solution
{
public:
    int lengthOfLongestSubstring(string s)
    {
        unordered_set<char> hash;
        int right = 0;
        int ans = 0;
        for (int left = 0;left < s.length();left++)
        {
	        while (right < s.length() && hash.find(s[right]) == hash.end())
	        {
                hash.insert(s[right]);
                right++;
	        }
            ans = max(ans, right - left);
            hash.erase(s[left]);
        }
        return ans;
    }
};
```

---

## 230909

看了俩题都做寄了，摆！待我学成《算法4》归来

---

