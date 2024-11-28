---
title: 「Algorithm4」读书笔记三：查找
tags:
  - Java
  - Algorithm
  - Note
categories:
  - Notes
  - Algorithm
  - 《Algorithms, 4th Edition》
description:
  - 《算法4》第三章：查找 读书笔记
cover: 'https://hmxs-1315810738.cos.ap-shanghai.myqcloud.com/img/202309132308381.png'
abbrlink: 4003
date: 2024-9-30 12:00:00
katex: true
---

<img src="https://hmxs-1315810738.cos.ap-shanghai.myqcloud.com/img/202409300327321.png" alt="algorithm4" style="zoom: 50%;" />

> **官网：[https://algs4.cs.princeton.edu/home/](https://algs4.cs.princeton.edu/home/)**
>
> **笔记目录：[https://hmxs.games/posts/4000/](https://hmxs.games/posts/4000/)**

---

# 查找

查找便是尽可能高效地在数据集合中定位特定元素的过程，而现代计算机和网络使我们能够得以访问到极其海量的信息，这时，查找便显得极为重要。本章将介绍一些经典查找算法。

符号表（字典）是对于查找的抽象数据结构实现，其最主要的目的就是将一个键（`key`）和一个值（`value`）联系起来，通过键我们便可以快速访问到值，而其实现有着多种方式，如二叉查找树、红黑树、散列表等，在本章中将着重介绍它们。

*使用了C++对查找算法进行了重新实现，在[这一Github仓库下](https://github.com/hmxsqaq/Algorithm-Searching)可以找到我的全部实现*

## 顺序查找符号表 - Sequential search in an unordered linked list

这是最简单的一种对于符号表的实现：我们可以使用链表来存储数据，在我们需要获取数据时，我们只需要从头一个个查找过去即可。

```c++
template <typename KEY, typename VALUE>
class SequentialSearchSymbolTable {
public:
    ~SequentialSearchSymbolTable() {
        while (head_ != nullptr) {
            Node* old = head_;
            head_ = head_->next;
            delete old;
        }
    }

    void put(KEY key, VALUE value) {
        for (Node* x = head_; x != nullptr; x = x->next) {
            if (x->key == key) {
                x->value = value;
                return;
            }
        }
        head_ = new Node(key, value, head_);
        size_++;
    }

    std::optional<VALUE> get(KEY key) {
        for (Node* x = head_; x != nullptr; x = x->next) {
            if (x->key == key) {
                return x->value;
            }
        }
        return std::nullopt;
    }

    void remove(KEY key) {
        Node* previous = nullptr;
        for (Node* x = head_; x != nullptr; previous = x, x = x->next) {
            if (x->key == key) {
                if (previous != nullptr) {
                    previous->next = x->next;
                } else {
                    head_ = x->next;
                }
                delete x;
                size_--;
                return;
            }
        }
    }

    bool contains(KEY key) { return get(key).has_value(); }
    bool is_empty() { return head_ == nullptr; }
    size_t size() { return size_;}

private:
    struct Node {
        KEY key;
        VALUE value;
        Node* next;
        Node(KEY key, VALUE value, Node* next) : key(key), value(value), next(next) {}
    };

    Node* head_ = nullptr;
    size_t size_ = 0;
};
```

以上使用了C++简单地实现了一个顺序符号表，其使用了一个私有内部`Node`类来表示链表节点用于存储`KEY`与`VALUE`，`get`与`put`方法都会顺序地搜索链表查找给定值。

这种实现当然可以运行，但是其效率相对较低，其`get`与`put`方法都有着$O(N)$级别的时间复杂度，在面对大规模问题时会显得力不从心。

## 二分查找符号表 - Binary search in an ordered array

这种符号表的实现也较为简单，其通过一对平行数组，分别存储键与值，然后通过二分查找实现数据的插入与获取

通过使用二分查找替换遍历，这种实现可以将`get`方法的复杂度优化到$logN$

```c++
template<Comparable KEY, typename VALUE>
class BinarySearchSymbolTable {
public:
    void put(KEY key, VALUE value) {
        int i = rank(key);
        if (i < keys_.size() && keys_[i] == key) {
            values_[i] = value;
            return;
        }
        keys_.insert(keys_.begin() + i, key);
        values_.insert(values_.begin() + i, value);
    }

    std::optional<VALUE> get(KEY key) {
        if (is_empty()) return std::nullopt;
        int i = rank(key);
        if (i < keys_.size() && keys_[i] == key) {
            return values_[i];
        }
        return std::nullopt;
    }

    void remove(KEY key) {
        if (is_empty()) return;
        int i = rank(key);
        if (i < keys_.size() && keys_[i] == key) {
            keys_.erase(keys_.begin() + i);
            values_.erase(values_.begin() + i);
        }
    }

    // returns the number of keys in this symbol table strictly less than key.
    int rank(KEY key) {
        int left = 0, right = keys_.size() - 1;
        while (left <= right) {
            int mid = left + (right - left) / 2; // prevent integer overflow
            if (key < keys_[mid]) {
                right = mid - 1;
            } else if (key > keys_[mid]) {
                left = mid + 1;
            } else {
                return mid;
            }
        }
        return left;
    }

    std::optional<KEY> min() {
        if (is_empty()) return std::nullopt;
        return keys_.front();
    }

    std::optional<KEY> max() {
        if (is_empty()) return std::nullopt;
        return keys_.back();
    }

    std::optional<KEY> select(int k) {
        if (k < 0 || k >= keys_.size()) return std::nullopt;
        return keys_[k];
    }

    std::optional<KEY> ceiling(KEY key) { return select(rank(key)); }

    std::optional<KEY> floor(KEY key) { return select(rank(key) - 1); }

    bool contains(KEY key) { return get(key).has_value(); }

    bool is_empty() { return keys_.empty(); }

    size_t size() { return keys_.size(); }

private:
    std::vector<KEY> keys_;
    std::vector<VALUE> values_;
};
```

## 二叉查找树 - Binary Search Tree

在上面的实现中，二叉查找树很好地优化了算法的速度，但因为要使用二分查找这种实现使用了数组存储数据，但这会让`put`方法的效率不够令人满意

或许我们需要链式地存储数据才能让插入操作变得快捷，那么应该如何在链式存储中使用二分查找解决问题呢？这就轮到`二叉查找树 - BST`出场了

