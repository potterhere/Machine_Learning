# 多变量逻辑回归
## 引言 
### Multiple features(variables) 多特征
| Size($feet^2$) | Number of bedrooms | Number of floors | Age of home(years) | Price($1000) |
| :------------: | :----------------: | :--------------: | :----------------: | :----------: |
| $x_1$          | $x_2$              | $x_3$            | $x_4$              | $y$          |
Notation:
> $n$ = number of features 特征数
> $x^{(i)}$ = input (features) of $i^{th}$ training example. 第$i^{th}$个训练样本 (引索)
> $x_j^{(i)}$ = value of features $j$ in $i^{th}$ training example.第$i^{th}$个训练样本中的第$j$个特征

### 假设形式
$h_\theta(x)=\theta_0+\theta_1 x_1+\theta_2 x_2+\theta_3 x_3+ ... + \theta_n x_n$
For convenience of notation,define $x_0=1$ ，方便运算 将 $x_0 = 1$ 
> 特别注意 : $x_0^{(i)}=1$
>
> $x=\begin{bmatrix}x_0\\x_1\\x_2\\.\\.\\.\\x_n\end{bmatrix} \in \Re^{n+1} \ \ \ \ \ \theta = \begin{bmatrix}\theta_0\\\theta_1\\\theta_2\\.\\.\\.\\\theta_n\end{bmatrix} \in \Re^{n+1}$
 
则 $h_{\theta}(x) = \theta_0 x_0 +\theta_1 x_1+\theta_2 x_2+\theta_3 x_3+ ... + \theta_n x_n$ 其中 $x_0 =1$ 
> $h_{\theta}(x) = \theta^T x = \begin{bmatrix}\theta_0\\\theta_1\\\theta_2\\.\\.\\.\\\theta_n\end{bmatrix}^T \times \begin{bmatrix}x_0\\x_1\\x_2\\.\\.\\.\\x_n\end{bmatrix}$

## 多元梯度下降法
Hypothesis 假设 : $h_{\theta}(x) = \theta^T x = \theta_0+\theta_1 x_1+\theta_2 x_2+\theta_3 x_3+ ... + \theta_n x_n$ 其中 $x_0 = 1$
Parameters 参数 : $\theta_0,\theta_1,...,\theta_n = \theta$
Cost function : $J(\theta_0,\theta_1,...,\theta_n) = J(\theta) = \dfrac{1}{2m} \sum^m_{i=1}(h_{\theta}(x^{(i)})-y^{(i)})^2$

Gradient descent :
Repeat {
    $\ \ \ \ \theta_j := \theta_j - \alpha \dfrac{\partial}{\partial\theta_j}J(\theta)$ 
} (simultaneously update for every $j =0,...,n$)
> 需要同时更新 $j =0,...,n$

* Previously(n=1) 单变量线性回归:
Repeat {
    $\ \ \ \ \theta_0 := \theta_0 - \alpha \dfrac{\partial}{\partial\theta_j}J(\theta) = \theta_0 - \alpha \dfrac{1}{m} \sum^m_{i=1}(h_{\theta}(x^{(i)})-y^{(i)})$ 
    $\ \ \ \ \theta_1 := \theta_1 - \alpha \dfrac{1}{m} \sum^m_{i=1}(h_{\theta}(x^{(i)})-y^{(i)}) \textcolor{red}{x^{(i)}}$ 
} (simultaneously update $\theta_0 ,\theta_1$)

* New algorithm 多元线性回归:
Repeat {
    $\ \ \ \ \theta_j := \theta_j - \alpha \dfrac{\partial}{\partial\theta_j}J(\theta) = \theta_j - \alpha \dfrac{1}{m}\sum^m_{i=1}(h_{\theta}(x^{(i)})-y^{(i)}) \textcolor{red}{x_j^{(i)}}$ 
} (simultaneously update $\theta_j$ for $j =0,...,n$)
---
$\textcolor{red}{\theta_0}:=\textcolor{red}{\theta_0}-\alpha\dfrac{1}{m}\sum^m_{i=1}(h_{\theta}(x^{(i)})-y^{(i)}) \textcolor{red}{x_0^{(i)}} \ \ \textcolor{blue}{x_0^{(i)}=0}$
$\textcolor{red}{\theta_1}:=\textcolor{red}{\theta_1}-\alpha\dfrac{1}{m}\sum^m_{i=1}(h_{\theta}(x^{(i)})-y^{(i)}) \textcolor{red}{x_1^{(i)}}$
$\textcolor{red}{\theta_2}:=\textcolor{red}{\theta_2}-\alpha\dfrac{1}{m}\sum^m_{i=1}(h_{\theta}(x^{(i)})-y^{(i)}) \textcolor{red}{x_2^{(i)}}$
...

## 多元梯度下降法处理技巧-特征缩放
1. Feature Scaling 特征缩放 
    * Idea: Make sure feature are on a similar scale. 确保每个特征的取值范围接近
    > 举例: 
    > * $x_1$ = size ( 0 ~ 2000 $feet^2$ ); $x_2$ = number of bedrooms ( 1 ~ 5 )
    这样损失函数会非常的狭长，收敛速度会非常慢
    > * 应用特城缩放 $x_1 = \dfrac{size(feet^2)}{2000}$ , $x_2 = \dfrac{number of bedrooms}{5}$
    这样损失函数是个圆，梯度下降会更加快，收敛速度也会很快
    * Get every feature into approximately a $-1 \leqslant x_i \leqslant 1$ range. 
    将每个特征的取值转化到与 $-1 \leqslant x_i \leqslant 1$ 相近的范围。
    > 举例:
    > * 可接受：$0 \leqslant x_1 \leqslant 3$ , $-1 \leqslant x_2 \leqslant 0.5$
    > * 不可接受: $-100 \leqslant x_3 \leqslant 100$ , $-0.0001 \leqslant x_4 \leqslant 0.0001$
    > 吴恩达个人经验认为 $-3 \leqslant x \leqslant 3$ and $-\dfrac{1}{3} \leqslant x \leqslant \dfrac{1}{3}$ 是合适的 
    > (足够接近梯度下降法就可以很好的工作)

2. Mean normalization 均值标准化
    * Replace $x_i$ with $x_i-\mu_i$ to make features have approximately zero mean (Do not apply to $x_0 = 1$).
    使用 $x_i-\mu_i$ 取代 $x_i$ 使训练集均值约为 **0**。
    (此方法不适用于 $x_0 = 1$ )
    > 举例：
    > *  $x_1 = \dfrac{size - 1000}{2000}$ , $x_2 = \dfrac{\#bedrooms-2}{5}$
    得到的均值归一化范围为 : $-0.5 \leqslant x_1 \leqslant 0.5 \ , \ -0.5 \leqslant x_2 \leqslant 0.5$

3. 综上 
    $x = \dfrac{x-\mu}{s_1}$ 此方法是一种接近取值的方法，目的是让**梯度下降法可以更高效的运行**
    > * $\mu$ 是 x 训练集的平均数 (avg value of x in training set)
    > * $s_1$ 取值范围 (range : $max-min$ ) 或者 标准差 ( standard deviation )

## 多元梯度下降法处理技巧-学习率
$\ \ \ \ \theta_j := \theta_j - \alpha\dfrac{\partial}{\partial\theta_j}J(\theta)$
- "Debugging" : How to make sure grandient descent is working correctly.
    > “调试” 如何确保梯度下降法正确运行
- How to choose learning rate $\alpha$.
    > 如何选择合适的学习率 $\alpha$

1. 确保梯度下降运算正确
    观察迭代次数增加的同时 $\underset{\theta}{min}J(\theta)$是否呈下降趋势。
    ``` vega
    {
        "$schema": "https://vega.github.io/schema/vega/v4.json",
        "width": 500,
        "height": 200,
        "padding": 5,

        "signals": [
            {
            "name": "interpolate",
            "value": "basis",
            "bind": {
                "input": "select",
                "options": [
                "basis",
                "cardinal",
                "catmull-rom",
                "linear",
                "monotone",
                "natural",
                "step",
                "step-after",
                "step-before"
                ]
            }
            }
        ],

        "data": [
            {
            "name": "table",
            "values": [
                {"x": 0, "y": 180, "c":0}, {"x": 0, "y": 200, "c":1},
                {"x": 100, "y": 75, "c":0}, {"x": 100, "y": 120, "c":1},
                {"x": 200, "y": 40, "c":0}, {"x": 200, "y": 50, "c":1},
                {"x": 300, "y": 30, "c":0}, {"x": 300, "y": 20, "c":1},
                {"x": 400, "y": 15, "c":0}, {"x": 400, "y": 16, "c":1},
                {"x": 500, "y": 10, "c":0}, {"x": 500, "y": 10, "c":1}
            ]
            }
        ],

        "scales": [
            {
            "name": "x",
            "type": "point",
            "range": "width",
            "domain": {"data": "table", "field": "x"}
            },
            {
            "name": "y",
            "type": "linear",
            "range": "height",
            "nice": true,
            "zero": true,
            "domain": {"data": "table", "field": "y"}
            },
            {
            "name": "color",
            "type": "ordinal",
            "range": "category",
            "domain": {"data": "table", "field": "c"}
            }
        ],

        "axes": [
            {"orient": "bottom", "scale": "x"},
            {"orient": "left", "scale": "y"}
        ],

        "marks": [
            {
            "type": "group",
            "from": {
                "facet": {
                "name": "series",
                "data": "table",
                "groupby": "c"
                }
            },
            "marks": [
                {
                "type": "line",
                "from": {"data": "series"},
                "encode": {
                    "enter": {
                    "x": {"scale": "x", "field": "x"},
                    "y": {"scale": "y", "field": "y"},
                    "stroke": {"scale": "color", "field": "c"},
                    "strokeWidth": {"value": 2}
                    },
                    "update": {
                    "interpolate": {"signal": "interpolate"},
                    "fillOpacity": {"value": 1}
                    },
                    "hover": {
                    "fillOpacity": {"value": 0.5}
                    }
                }
                }
            ]
            }
        ]
    }
    ```
    > 纵轴 ：$\underset{\theta}{min}J(\theta)$ 
    > 横轴 : No. of iterations 迭代次数
    > 在很多迭代次数后 $\underset{\theta}{min}J(\theta)$ 不再变化，表示进入局部最优解，收敛状态
    
    * Convergence test 收敛测试
    Declare convergence if $J(\theta)$ decreases by less than $10^{-3}$ in one iteration.
    如果在下一次迭代中 $J(\theta)$ 下降 $\epsilon$ 少于 $10^{-3}$ 则认为收敛。

2. 如何选择合适的学习率
    
    ``` vega
    {
            "$schema": "https://vega.github.io/schema/vega/v4.json",
        "width": 500,
        "height": 200,
        "padding": 5,

        "signals": [
            {
            "name": "interpolate",
            "value": "basis",
            "bind": {
                "input": "select",
                "options": [
                "basis",
                "cardinal",
                "catmull-rom",
                "linear",
                "monotone",
                "natural",
                "step",
                "step-after",
                "step-before"
                ]
            }
            }
        ],
        "data": [
            {
            "name": "table",
            "values": [
                {"x": 0, "y": 20, "c":0},{"x": 0, "y": 15, "c":1},
                {"x": 50, "y": 150, "c":0},{"x": 50, "y": 30, "c":1},
                {"x": 100, "y": 10, "c":0},{"x": 100, "y": 50, "c":1},
                {"x": 150, "y": 160, "c":0},{"x": 150, "y": 90, "c":1},
                {"x": 200, "y": 30, "c":0},{"x": 200, "y": 120, "c":1},
                {"x": 250, "y": 145, "c":0}, {"x": 250, "y": 150, "c":1},
                {"x": 300, "y": 18, "c":0},{"x": 300, "y": 180, "c":1},
                {"x": 350, "y": 170, "c":0}, {"x": 350, "y": 190, "c":1},
                {"x": 400, "y": 40, "c":0},{"x": 400, "y": 200, "c":1}
            ]
            }
        ],
        "scales": [
            {
            "name": "x",
            "type": "point",
            "range": "width",
            "domain": {"data": "table", "field": "x"}
            },
            {
            "name": "y",
            "type": "linear",
            "range": "height",
            "nice": true,
            "zero": true,
            "domain": {"data": "table", "field": "y"}
            },
            {
            "name": "color",
            "type": "ordinal",
            "range": "category",
            "domain": {"data": "table", "field": "c"}
            }
        ],

        "axes": [
            {"orient": "bottom", "scale": "x"},
            {"orient": "left", "scale": "y"}
        ],

        "marks": [
            {
            "type": "group",
            "from": {
                "facet": {
                "name": "series",
                "data": "table",
                "groupby": "c"
                }
            },
            "marks": [
                {
                "type": "line",
                "from": {"data": "series"},
                "encode": {
                    "enter": {
                    "x": {"scale": "x", "field": "x"},
                    "y": {"scale": "y", "field": "y"},
                    "stroke": {"scale": "color", "field": "c"},
                    "strokeWidth": {"value": 2}
                    },
                    "update": {
                    "interpolate": {"signal": "interpolate"},
                    "fillOpacity": {"value": 1}
                    },
                    "hover": {
                    "fillOpacity": {"value": 0.5}
                    }
                }
                }
            ]
            }
        ]
    }
    ```
    * 如果 $\underset{\theta}{min}J(\theta)$ 随着迭代次数不断 **上升** (橘黄色线)，则说明需要减小学习率 $\textcolor{red}{\alpha}$
    
    * 如果 $\underset{\theta}{min}J(\theta)$ 随着迭代次数不断 **上升下降反复抖动** (蓝色线) ，则说明需要减小学习率 $\textcolor{red}{\alpha}$
    
    * 如果学习率 $\alpha$ 过小，训练收敛速度会很慢

### 总结
- If $\alpha$ is too small : slow convergence.
    > 如果学习率 $\alpha$ 太小，收敛会很慢。
- If $\alpha$ is too larget : $J(\theta)$ may not decrease on every iteration; may not converge.
    > 如果学习率 $\alpha$ 太大，$J(\theta)$ 在每次迭代不会下降，甚至不会收敛。
    > (也有可能出现收敛缓慢的问题)
- $\alpha$ 可以尝试 $...,0.001,0.01,0.1,1,...$ (一般 **10** 倍增长,可采用二分法逐步找到合适的学习率$\alpha$)
  
## 特征 与 多项式回归