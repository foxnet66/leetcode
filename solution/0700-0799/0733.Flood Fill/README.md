# [733. 图像渲染](https://leetcode-cn.com/problems/flood-fill)

[English Version](/solution/0700-0799/0733.Flood%20Fill/README_EN.md)

## 题目描述

<!-- 这里写题目描述 -->

<p>有一幅以二维整数数组表示的图画，每一个整数表示该图画的像素值大小，数值在 0 到 65535 之间。</p>

<p>给你一个坐标&nbsp;<code>(sr, sc)</code>&nbsp;表示图像渲染开始的像素值（行 ，列）和一个新的颜色值&nbsp;<code>newColor</code>，让你重新上色这幅图像。</p>

<p>为了完成上色工作，从初始坐标开始，记录初始坐标的上下左右四个方向上像素值与初始坐标相同的相连像素点，接着再记录这四个方向上符合条件的像素点与他们对应四个方向上像素值与初始坐标相同的相连像素点，&hellip;&hellip;，重复该过程。将所有有记录的像素点的颜色值改为新的颜色值。</p>

<p>最后返回经过上色渲染后的图像。</p>

<p><strong>示例 1:</strong></p>

<pre>
<strong>输入:</strong> 
image = [[1,1,1],[1,1,0],[1,0,1]]
sr = 1, sc = 1, newColor = 2
<strong>输出:</strong> [[2,2,2],[2,2,0],[2,0,1]]
<strong>解析:</strong> 
在图像的正中间，(坐标(sr,sc)=(1,1)),
在路径上所有符合条件的像素点的颜色都被更改成2。
注意，右下角的像素没有更改为2，
因为它不是在上下左右四个方向上与初始点相连的像素点。
</pre>

<p><strong>注意:</strong></p>

<ul>
	<li><code>image</code> 和&nbsp;<code>image[0]</code>&nbsp;的长度在范围&nbsp;<code>[1, 50]</code> 内。</li>
	<li>给出的初始点将满足&nbsp;<code>0 &lt;= sr &lt; image.length</code> 和&nbsp;<code>0 &lt;= sc &lt; image[0].length</code>。</li>
	<li><code>image[i][j]</code> 和&nbsp;<code>newColor</code>&nbsp;表示的颜色值在范围&nbsp;<code>[0, 65535]</code>内。</li>
</ul>

## 解法

<!-- 这里可写通用的实现逻辑 -->

DFS。

<!-- tabs:start -->

### **Python3**

<!-- 这里可写当前语言的特殊实现逻辑 -->

```python
class Solution:
    def floodFill(self, image: List[List[int]], sr: int, sc: int, newColor: int) -> List[List[int]]:
        def dfs(i, j, oc, nc):
            if i < 0 or i >= len(image) or j < 0 or j >= len(image[0]) or image[i][j] != oc or image[i][j] == nc:
                return
            image[i][j] = nc
            for x, y in [[0, 1], [0, -1], [1, 0], [-1, 0]]:
                dfs(i + x, j + y, oc, nc)

        dfs(sr, sc, image[sr][sc], newColor)
        return image
```

```python
class Solution:
    def floodFill(self, image: List[List[int]], sr: int, sc: int, newColor: int) -> List[List[int]]:
        if image[sr][sc] == newColor:
            return image
        q = deque([(sr, sc)])
        oc = image[sr][sc]
        image[sr][sc] = newColor
        while q:
            i, j = q.popleft()
            for a, b in [[0, -1], [0, 1], [1, 0], [-1, 0]]:
                x, y = i + a, j + b
                if 0 <= x < len(image) and 0 <= y < len(image[0]) and image[x][y] == oc:
                    q.append((x, y))
                    image[x][y] = newColor
        return image
```

### **Java**

<!-- 这里可写当前语言的特殊实现逻辑 -->

```java
class Solution {
    private int[][] dirs = new int[][]{{0, 1}, {0, -1}, {1, 0}, {-1, 0}};

    public int[][] floodFill(int[][] image, int sr, int sc, int newColor) {
        dfs(image, sr, sc, image[sr][sc], newColor);
        return image;
    }

    private void dfs(int[][] image, int i, int j, int oc, int nc) {
        if (i < 0 || i >= image.length || j < 0 || j >= image[0].length || image[i][j] != oc || image[i][j] == nc) {
            return;
        }
        image[i][j] = nc;
        for (int[] dir : dirs) {
            dfs(image, i + dir[0], j + dir[1], oc, nc);
        }
    }
}
```

```java
class Solution {
    public int[][] floodFill(int[][] image, int sr, int sc, int newColor) {
        if (image[sr][sc] == newColor) {
            return image;
        }
        Deque<int[]> q = new ArrayDeque<>();
        q.offer(new int[]{sr, sc});
        int oc = image[sr][sc];
        image[sr][sc] = newColor;
        int[] dirs = {-1, 0, 1, 0, -1};
        while (!q.isEmpty()) {
            int[] p = q.poll();
            int i = p[0], j = p[1];
            for (int k = 0; k < 4; ++k) {
                int x = i + dirs[k], y = j + dirs[k + 1];
                if (x >= 0 && x < image.length && y >= 0 && y < image[0].length && image[x][y] == oc) {
                    q.offer(new int[]{x, y});
                    image[x][y] = newColor;
                }
            }
        }
        return image;
    }
}
```

### **C++**

```cpp
class Solution {
public:
    vector<vector<int>> dirs = {{0, 1}, {0, -1}, {1, 0}, {-1, 0}};

    vector<vector<int>> floodFill(vector<vector<int>>& image, int sr, int sc, int newColor) {
        dfs(image, sr, sc, image[sr][sc], newColor);
        return image;
    }

    void dfs(vector<vector<int>>& image, int i, int j, int oc, int nc) {
        if (i < 0 || i >= image.size() || j < 0 || j >= image[0].size() || image[i][j] != oc || image[i][j] == nc) return;
        image[i][j] = nc;
        for (auto& dir : dirs) dfs(image, i + dir[0], j + dir[1], oc, nc);
    }
};
```

```cpp
class Solution {
public:
    vector<vector<int>> floodFill(vector<vector<int>>& image, int sr, int sc, int newColor) {
        if (image[sr][sc] == newColor) return image;
        int oc = image[sr][sc];
        image[sr][sc] = newColor;
        queue<pair<int, int>> q;
        q.push({sr, sc});
        vector<int> dirs = {-1, 0, 1, 0, -1};
        while (!q.empty())
        {
            auto p = q.front();
            q.pop();
            for (int k = 0; k < 4; ++k)
            {
                int x = p.first + dirs[k];
                int y = p.second + dirs[k + 1];
                if (x >= 0 && x < image.size() && y >= 0 && y < image[0].size() && image[x][y] == oc)
                {
                    q.push({x, y});
                    image[x][y] = newColor;
                }
            }
        }
        return image;
    }
};
```

### **Go**

```go
func floodFill(image [][]int, sr int, sc int, newColor int) [][]int {
	dfs(image, sr, sc, image[sr][sc], newColor)
	return image
}

func dfs(image [][]int, i, j, oc, nc int) {
	if i < 0 || i >= len(image) || j < 0 || j >= len(image[0]) || image[i][j] != oc || image[i][j] == nc {
		return
	}
	image[i][j] = nc
	dirs := [4][2]int{{0, -1}, {0, 1}, {1, 0}, {-1, 0}}
	for _, dir := range dirs {
		dfs(image, i+dir[0], j+dir[1], oc, nc)
	}
}
```

```go
func floodFill(image [][]int, sr int, sc int, newColor int) [][]int {
	if image[sr][sc] == newColor {
		return image
	}
	oc := image[sr][sc]
	q := [][]int{[]int{sr, sc}}
	image[sr][sc] = newColor
	dirs := []int{-1, 0, 1, 0, -1}
	for len(q) > 0 {
		p := q[0]
		q = q[1:]
		for k := 0; k < 4; k++ {
			x, y := p[0]+dirs[k], p[1]+dirs[k+1]
			if x >= 0 && x < len(image) && y >= 0 && y < len(image[0]) && image[x][y] == oc {
				q = append(q, []int{x, y})
				image[x][y] = newColor
			}
		}
	}
	return image
}
```

### **TypeScript**

```ts
function floodFill(
    image: number[][],
    sr: number,
    sc: number,
    newColor: number,
): number[][] {
    const m = image.length;
    const n = image[0].length;
    const target = image[sr][sc];
    const dfs = (i: number, j: number) => {
        if (
            i < 0 ||
            i === m ||
            j < 0 ||
            j === n ||
            image[i][j] !== target ||
            image[i][j] === newColor
        ) {
            return;
        }
        image[i][j] = newColor;
        dfs(i + 1, j);
        dfs(i - 1, j);
        dfs(i, j + 1);
        dfs(i, j - 1);
    };
    dfs(sr, sc);
    return image;
}
```

### **Rust**

```rust
impl Solution {
    fn dfs(image: &mut Vec<Vec<i32>>, sr: i32, sc: i32, new_color: i32, target: i32) {
        if sr < 0 || sr == image.len() as i32 || sc < 0 || sc == image[0].len() as i32 {
            return;
        }
        let sr = sr as usize;
        let sc = sc as usize;
        if sr < 0 || image[sr][sc] == new_color || image[sr][sc] != target {
            return;
        }
        image[sr][sc] = new_color;
        let sr = sr as i32;
        let sc = sc as i32;
        Self::dfs(image, sr + 1, sc, new_color, target);
        Self::dfs(image, sr - 1, sc, new_color, target);
        Self::dfs(image, sr, sc + 1, new_color, target);
        Self::dfs(image, sr, sc - 1, new_color, target);
    }
    pub fn flood_fill(image: Vec<Vec<i32>>, sr: i32, sc: i32, new_color: i32) -> Vec<Vec<i32>> {
        let target = image[sr as usize][sc as usize];
        Self::dfs(&mut image, sr, sc, new_color, target);
        image
    }
}
```

### **...**

```

```

<!-- tabs:end -->
