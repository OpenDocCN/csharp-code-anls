# `.\better-genshin-impact\BetterGenshinImpact\GameTask\AutoFishing\RodNet.cs`

```cs
# 定义 RodNet 类，用于某些计算或处理任务
public class RodNet
{
    # 定义常量 alpha，用于后续计算
    const double alpha = 1734.34 / 2.5;

    # 定义静态只读数组 dz，存储预定义的值
    static readonly double[] dz = [ 0.561117562965, 0.637026851288, 0.705579317577,
                                     1.062734463845, 0.949307580751, 1.015620474332,
                                     1.797904203405, 1.513476738412, 1.013873007495,
                                     1.159949954831, 1.353650974146, 1.302893195071 ];

    # 定义静态只读二维数组 theta，存储多个预定义的值
    static readonly double[,] theta = {
                                        {-0.262674397633, 0.317025388945, -0.457150765450, 0.174522158281,
                                          -0.957110676932, -0.095339800558, -0.119519564026, -0.139914755291,
                                          -0.580893838475, 0.702302245305, 0.271575851220, 0.708473199472,
                                          0.699108382380},
                                        {-1.062702043060, -0.280779165943, -0.289891597384, 0.220173840594,
                                          0.493463877037, -0.326492366566, 1.215859141832, 1.607133159643,
                                          1.619199133672, 0.356402262447, 0.365385941958, 0.411869019381,
                                          0.224962055122},
                                        {0.460481782256, 0.048180392806, 0.475529271293, -0.150186412126,
                                          0.135512307120, 0.087365984352, -1.317661146364, -1.882438208662,
                                          -1.502483859283, -0.580228373556, -1.005821958682, -1.184199131739,
                                          -1.285988918494}
                                       };

    # 定义静态只读数组 B，存储预定义的系数
    static readonly double[] B = [1.241950004386, 3.051113640564, -3.848898190087];

    # 定义静态只读数组 offset，存储预定义的偏移值
    static readonly double[] offset = [ 0.4, 0.2, 0.4, 0, 0.3, 0.3,
        0.3, 0.15, 0.5, 0.5, 0.5, 0.5 ];

    # 定义静态方法 F，用于计算某些值
    static void F(double[] dst, double[] x, double[] y)
    {
        # 从输入数组 x 中获取值
        double y0 = x[0], z0 = x[1], t = x[2];
        # 计算 tmp 作为公式的一部分
        double tmp = (y0 + t * z0) * (y0 + t * z0) - 1;
        # 计算 dst 数组中的第一个值
        dst[0] = Math.Sqrt((1 + t * t) / tmp) - y[0];
        # 计算 dst 数组中的第二个值
        dst[1] = (1 + t * t) * z0 / tmp - y[1];
        # 计算 dst 数组中的第三个值
        dst[2] = ((t * t - 1) * y0 * z0 + t * (y0 * y0 - z0 * z0 - 1)) / tmp - y[2];
    }

    # 定义静态方法 DfInv，未完成
    static void DfInv(double[] dst, double[] x)
    # 使用给定的 x 数组计算一系列值并将结果存储在 dst 数组中
    {
        # 将 x 数组中的第一个元素赋值给 y0，第二个元素赋值给 z0，第三个元素赋值给 t
        double y0 = x[0], z0 = x[1], t = x[2];
        # 计算 tmp1，首先计算 (y0 + t * z0) 的平方，然后减去 1
        double tmp1 = (y0 + t * z0) * (y0 + t * z0) - 1;
        # 计算 tmp2，1 加上 t 的平方
        double tmp2 = 1 + t * t;
        # 根据给定公式计算 dst[0] 的值
        dst[0] = (1 - y0 * y0 + z0 * z0) / y0 * Math.Sqrt(tmp1 / tmp2);
        # 根据给定公式计算 dst[1] 的值
        dst[1] = -z0 * (y0 * y0 + t * (t * (1 + z0 * z0) + 2 * y0 * z0)) / tmp2 / y0;
        # 根据给定公式计算 dst[2] 的值
        dst[2] = ((t * t - 1) * y0 * z0 + t * (y0 * y0 - z0 * z0 - 1)) / y0 / tmp2;
        # 根据给定公式计算 dst[3] 的值
        dst[3] = -2 * z0 * Math.Sqrt(tmp1 / tmp2);
        # 根据给定公式计算 dst[4] 的值
        dst[4] = tmp1 / tmp2;
        # 将 dst[5] 设为 0
        dst[5] = 0;
        # 根据给定公式计算 dst[6] 的值
        dst[6] = -z0 / y0 * Math.Sqrt(tmp1 / tmp2);
        # 根据给定公式计算 dst[7] 的值
        dst[7] = (y0 + t * z0) * (y0 + t * z0) / y0;
        # 根据给定公式计算 dst[8] 的值
        dst[8] = 1 + t * z0 / y0;
    }

    # 使用 Newton-Raphson 方法求解方程
    static bool NewtonRaphson(Action<double[], double[], double[]> f, Action<double[], double[]> dfInv, double[] dst, double[] y,
                               double[] init, int n, int maxIter, double eps)
    {
        # 定义用于存储函数估计值、导数矩阵和当前解的数组
        double[] fEst = new double[n];
        double[] dfInvMat = new double[n * n];
        double[] x = new double[n];
        double err;

        # 复制初始值到 x 数组
        Array.Copy(init, x, n);

        # 进行最大迭代次数的循环
        for (int iter = 0; iter < maxIter; iter++)
        {
            # 初始化误差为 0
            err = 0;
            # 计算函数 f 在当前 x 值下的估计值
            f(fEst, x, y);
            # 累加估计值的绝对值来计算误差
            for (int i = 0; i < n; i++)
            {
                err += Math.Abs(fEst[i]);
            }

            # 如果误差小于给定的阈值 eps，则认为收敛
            if (err < eps)
            {
                # 将当前解复制到 dst 数组并返回 true 表示收敛
                Array.Copy(x, dst, n);
                // printf("Newton-Raphson solver converge after %d steps, err: %lf !\n",
                //        iter, err);
                return true;
            }

            # 计算导数矩阵 dfInvMat
            dfInv(dfInvMat, x);

            # 使用导数矩阵更新当前解 x
            for (int i = 0; i < n; i++)
            {
                for (int j = 0; j < n; j++)
                {
                    x[i] -= dfInvMat[n * i + j] * fEst[j];
                }
            }
        }
        # 达到最大迭代次数仍未收敛，返回 false
        return false;
    }

    # 计算给定数组 x 的 softmax 值，并将结果存储在 dst 数组中
    static void Softmax(double[] dst, double[] x, int n)
    {
        # 计算 softmax 函数的归一化因子
        double sum = 0;
        for (int i = 0; i < n; i++)
        {
            # 计算每个元素的指数值
            dst[i] = Math.Exp(x[i]);
            # 累加指数值
            sum += dst[i];
        }
        # 归一化每个元素的指数值
        for (int i = 0; i < n; i++)
        {
            dst[i] /= sum;
        }
    }

    # 代码块结束，未完成的方法声明
    public static int GetRodState(RodInput input)
    {
        // 声明变量 a, b, v0, u, v 用于计算
        double a, b, v0, u, v;

        // 计算 rod 的水平和垂直半径，并将其除以 alpha
        a = (input.rod_x2 - input.rod_x1) / 2 / alpha;
        b = (input.rod_y2 - input.rod_y1) / 2 / alpha;

        // 如果 a 小于 b，则交换 a 和 b 的值
        if (a < b)
        {
            (b, a) = (a, b);
        }

        // 计算 v0 为 rod 的垂直中点距离 288 减去 rod 的中心点 y 坐标并除以 alpha
        v0 = (288 - (input.rod_y1 + input.rod_y2) / 2) / alpha;

        // 计算 u 和 v 分别为鱼的水平位置和垂直位置
        u = (input.fish_x1 + input.fish_x2 - input.rod_x1 - input.rod_x2) / 2 / alpha;
        v = (288 - (input.fish_y1 + input.fish_y2) / 2) / alpha;

        // 初始化 y0z0t 数组用于存储计算结果
        double[] y0z0t = new double[3];
        // 初始化 abv0 数组并赋值为 [a, b, v0]
        double[] abv0 = [a, b, v0];
        // 初始化 init 数组为 [30, 15, 1]
        double[] init = [30, 15, 1];

        // 使用牛顿-拉夫森法求解，求解结果保存在 y0z0t 中
        bool solveSuccess = NewtonRaphson(F, DfInv, y0z0t, abv0, init, 3, 1000, 1e-6);

        // 如果求解失败，返回 -1
        if (!solveSuccess)
        {
            return -1;
        }

        // 从 y0z0t 中提取结果 y0, z0 和 t
        double y0 = y0z0t[0], z0 = y0z0t[1], t = y0z0t[2];
        double x, y, dist;
        // 计算 x、y 和 dist
        x = u * (z0 + dz[input.fish_label]) * Math.Sqrt(1 + t * t) / (t - v);
        y = (z0 + dz[input.fish_label]) * (1 + t * v) / (t - v);
        dist = Math.Sqrt(x * x + (y - y0) * (y - y0));

        // 初始化 logits 数组用于存储每个类别的 logits
        double[] logits = new double[3];
        // 计算每个类别的 logits 值
        for (int i = 0; i < 3; i++)
        {
            logits[i] = theta[i, 0] * dist + theta[i, 1 + input.fish_label] + B[i];
        }

        // 初始化 pred 数组用于存储 softmax 的结果
        double[] pred = new double[3];
        // 使用 softmax 函数计算概率分布
        Softmax(pred, logits, 3);
        // 从 pred 中减去 offset[input.fish_label]
        pred[0] -= offset[input.fish_label];

        // 返回预测概率最高的索引
        return Array.IndexOf(pred, pred.Max());
    }

    // 根据 rod 和 fish 的位置及鱼的类型索引获取 rod 的状态
    public static int GetRodState(Rect rod, Rect fish, int fishTypeIndex)
    {
        // 创建一个 RodInput 对象并初始化其属性
        RodInput input = new()
        {
            rod_x1 = rod.Left,
            rod_x2 = rod.Right,
            rod_y1 = rod.Top,
            rod_y2 = rod.Bottom,
            fish_x1 = fish.Left,
            fish_x2 = fish.Right,
            fish_y1 = fish.Top,
            fish_y2 = fish.Bottom,
            fish_label = fishTypeIndex
        };
        // 调用 GetRodState 方法并返回结果
        return GetRodState(input);
    }
# 结束前一个代码块或代码块中的控制结构
}
```