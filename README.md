# 常用算法
## Union-Find
```
Class UnionFindSet:
    func UnionFindSet(n):
        parents = [1..n]
        ranks = [0..0] (n zeros)

    func Find(x):
        if x != parents[x]:
            parents[x] = Find(parents[x])
        return parents[x];

    func Union(x, t):
        px, py = Find(x), Find(y)
        if ranks[px] > ranks[py]: parents[py] = px;
        if ranks[py] > ranks[px]: parents[px] = py;
        if ranks[px] == ranks[py]:
            parents[py] = px
            ranks[px]++
```

## Fenwick Tree
``` C++
Class FenwickTree {
public:
    FenwickTree(int n) { 
        sums_(n + 1, 0);
    }
    
    void update(int i, int delta) {
        while (i < sums_.size()) {
            sums_[i] += delta;
            i += lowbit(i);
        }
    }
    
    int query(int i) const {
        int sum = 0;
        while (i > 0) {
            sum += sums_[i];
            i -= lowbit(i);
        }
        return sum;
    }
    
private:
    static inline int lowbit(int x) { return x & (-x); }
    vector<int> sums_;
}
```
