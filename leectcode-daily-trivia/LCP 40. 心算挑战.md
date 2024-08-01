### [LCP 40. 心算挑战](https://leetcode.cn/problems/uOAnQW/)

简单

「力扣挑战赛」心算项目的挑战比赛中，要求选手从 `N` 张卡牌中选出 `cnt` 张卡牌，若这 `cnt` 张卡牌数字总和为偶数，则选手成绩「有效」且得分为 `cnt` 张卡牌数字总和。 给定数组 `cards` 和 `cnt`，其中 `cards[i]` 表示第 `i` 张卡牌上的数字。 请帮参赛选手计算最大的有效得分。若不存在获取有效得分的卡牌方案，则返回 0。

**示例 1：**

> 输入：`cards = [1,2,8,9], cnt = 3`
>
> 输出：`18`
>
> 解释：选择数字为 1、8、9 的这三张卡牌，此时可获得最大的有效得分 1+8+9=18。

**示例 2：**

> 输入：`cards = [3,3,1], cnt = 1`
>
> 输出：`0`
>
> 解释：不存在获取有效得分的卡牌方案。

**提示：**

- `1 <= cnt <= cards.length <= 10^5`
- `1 <= cards[i] <= 1000`

C++版本

```c++
// 方法一：一次遍历
class Solution {
public:
    int maxmiumScore(vector<int>& cards, int cnt) {
        sort(cards.begin(), cards.end());
        
        int ans = 0;
        int tmp = 0;
        int odd, even = -1;
        int end = cards.size() - cnt;
        for (int i = cards.size() - 1; i >= end; i--) {
            tmp += cards[i];
            if (cards[i] & 1) {
                odd = cards[i];
            } else {
                even = cards[i];
            }
        }

        if (!(tmp & 1)) {
            return tmp;
        }

        for (int i = cards.size() - cnt - 1; i >= 0; i--) {
            if (cards[i] & 1) {
                if (even != -1) {
                    ans = max(ans, tmp - even + cards[i]);
                }
            } else {
                if (odd != -1) {
                    ans = max(ans, tmp - odd + cards[i]);
                }
            }
        }
        return ans;
    }
};

// 方法二：哈希
class Solution {
public:
    int maxmiumScore(vector<int>& cards, int cnt) {
        int val[1005];
        for (int i = 0; i < cards.size(); i++) {
            val[cards[i]]++;
        }

        int ans = 0;
        int tmp = 0;
        int ed = -1;
        int odd, even = -1;
        for (int i = 1000; i >= 1; i--) {
            if (val[i] == 0) {
                continue;
            }

            if (val[i] > cnt) {
                tmp += cnt * i;
                cnt = 0;
            } else {
                tmp += val[i] * i;
                cnt -= val[i];
                val[i] = 0;
            }

            if (i & 1) {
                odd = i;
            } else {
                even = i;
            }

            if (!cnt) {
                if (val[i] > 0) {
                    ed = i;
                } else {
                    ed = i - 1;
                }
                break;
            }
        }

        if (!(tmp & 1)) {
            return tmp;
        }

        for (int i = ed; i >= 1; i--) {
            if (val[i] == 0) {
                continue;
            }

            if (i & 1) {
                if (even != -1) {
                    ans = max(ans, tmp - even + i);
                }
            } else {
                if (odd != -1) {
                    ans = max(ans, tmp - odd + i);
                }
            }
        }

        return ans;
    }
};
```

Java版本

```java
// 方法一：一次遍历
class Solution {
    public int maxmiumScore(int[] cards, int cnt) {
        Arrays.sort(cards);
        
        int ans = 0;
        int tmp = 0;
        int odd = -1, even = -1;
        int end = cards.length - cnt;
        for (int i = cards.length - 1; i >= end; i--) {
            tmp += cards[i];
            if ((cards[i] & 1) != 0) {
                odd = cards[i];
            } else {
                even = cards[i];
            }
        }

        if ((tmp & 1) == 0) {
            return tmp;
        }

        for (int i = cards.length - cnt - 1; i >= 0; i--) {
            if ((cards[i] & 1) != 0) {
                if (even != -1) {
                    ans = Math.max(ans, tmp - even + cards[i]);
                    break;
                }
            }
        }

        for (int i = cards.length - cnt - 1; i >= 0; i--) {
            if ((cards[i] & 1) == 0) {
                if (odd != -1) {
                    ans = Math.max(ans, tmp - odd + cards[i]);
                    break;
                }
            }
        }

        return ans;
    }
}

// 方法二：哈希
class Solution {
    public int maxmiumScore(int[] cards, int cnt) {
        int[] val = new int[1001];
        for (int i = 0; i < cards.length; i++) {
            val[cards[i]]++;
        }

        int ans = 0;
        int tmp = 0;
        int ed = -1;
        int odd = -1, even = -1;
        for (int i = 1000; i >= 1; i--) {
            if (val[i] == 0) {
                continue;
            }

            if (val[i] > cnt) {
                tmp += cnt * i;
                cnt = 0;
            } else {
                tmp += val[i] * i;
                cnt -= val[i];
                val[i] = 0;
            }

            if ((i & 1) != 0) {
                odd = i;
            } else {
                even = i;
            }

            if (cnt == 0) {
                if (val[i] > 0) {
                    ed = i;
                } else {
                    ed = i - 1;
                }
                break;
            }
        }

        if ((tmp & 1) == 0) {
            return tmp;
        }

        for (int i = ed; i >= 1; i--) {
            if (val[i] == 0) {
                continue;
            }

            if ((i & 1) != 0) {
                if (even != -1) {
                    ans = Math.max(ans, tmp - even + i);
                }
            } else {
                if (odd != -1) {
                    ans = Math.max(ans, tmp - odd + i);
                }
            }
        }

        return ans;
    }
}
```

