# 最优二叉树-哈夫曼树（Haffman）

最优二叉树，也称为哈夫曼树，是指对于一组带有确定权值的叶结点，构造的具有最小带权路径长度的二叉树。

设二叉树具有n个带权值的叶子结点,则从根结点到每一个叶子结点的路径长度与该叶子结点权值的乘积之和称为二叉树路径长度,记做：

WPL = W1L1 + W2L2 + ...... + WnLn;

其中：n为二叉树中叶子结点的个数；Wk为第k个叶子的权值;Lk为第k个叶子结点的路径长度。
**根据哈夫曼树的定义，一棵二叉树要使其WPL值越小，必须使权值越大的叶结点靠近根结点，二权值越小的叶结点越远离根结点**

哈夫曼树的建立过程：

![img](https://img-blog.csdnimg.cn/20190305122911342.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQwMzIzMjU2,size_16,color_FFFFFF,t_70)

1.路径长度：在树中从一个结点到另一个结点所经历的分支构成了这两个结点间的路径上的分支数称为它的路径长度

2．树的路径长度：从树根到树中每一结点的路径长度之和。

3．树的带权路径长度(Weighted Path Length of Tree，简记为WPL)
　　结点的权：在一些应用中，赋予树中结点的一个有某种意义的实数。
　　结点的带权路径长度：结点到树根之间的路径长度与该结点上权的乘积。
　　树的带权路径长度(Weighted Path Length of Tree)：定义为树中所有叶结点的带权路径长度之和

【例】给定4个叶子结点a，b，c和d，分别带权7，5，2和4。构造如下图所示的三棵二叉树(还有许多棵)，它们的带权路径长度分别为：

![img](https://img-blog.csdnimg.cn/20190305123415199.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQwMzIzMjU2,size_16,color_FFFFFF,t_70)

​    (a)WPL=7*2+5*2+2*2+4*2=36
​    (b)WPL=7*3+5*3+2*1+4*2=46
​    (c)WPL=7*1+5*2+2*3+4*3=35

其中(c)树的WPL最小，可以验证，它就是哈夫曼树。
代码实现：

```c#
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using System.Threading.Tasks;
 
namespace 用链式存储结构表示二叉树
{
    class HNode
    {
        private int weight;//结点权值
        private int lChild;//左孩子结点
        private int rChild;//右孩子结点
        private int parent;//父结点
 
        public int Weight { get => weight; set => weight = value; }
        public int LChild { get => lChild; set => lChild = value; }
        public int RChild { get => rChild; set => rChild = value; }
        public int Parent { get => parent; set => parent = value; }
 
        public HNode()
        {
            weight = 0;
            lChild = -1;
            rChild = -1;
            parent = -1;
        }
 
    }
 
    //哈夫曼树
    class HuffmanTree
    {
        private int leafNum;//叶子结点数目
        private HNode[] data;//结点数组
 
        public int LeafNum { get => leafNum; set => leafNum = value; }
        public HNode this[int index]
        {
            get { return data[index]; }
            set { data[index] = value; }
        }
 
        public HuffmanTree (int n)
        {
            data = new HNode[2 * n - 1];
            for (int i = 0; i < 2*n-1; i++)
            {
                data[i] = new HNode();
            }
            leafNum = n;
        }
 
        /// <summary>
        /// 创建哈夫曼树
        /// </summary>
        public void Create()
        {
            int m1, m2, x1, x2;
            //输入n个子结点的权值
            for (int i = 0; i < this .leafNum ; i++)
            {
                data[i].Weight = Convert.ToInt32(Console.ReadLine());
            }
 
            //处理n个叶子结点，建立哈夫曼树
            for (int i = 0; i < this .leafNum +-1; i++)
            {
                m1 = m2 = Int32.MaxValue;
                x1 = x2 = 0;
 
                //在全部结点中找权值最小的两个结点
                for (int j = 0; j < this .leafNum +1; j++)
                {
                    if((data [j].Weight <m1)&&(data [j]).Parent == -1)
                    {
                        m2 = m1;
                        x2 = x1;
                        m1 = data[j].Weight;
                        x1 = j;
                    }else if ((data [j].Weight <m2)&&(data [j].Parent == -1))
                    {
                        m2 = data[j].Weight;
                        x2 = j;
                    }
                }
 
                data[x1].Parent = this.leafNum + i;
                data[x2].Parent = this.leafNum + i;
                data[this.leafNum + i].Weight = data[x1].Weight + data[x2].Weight;
                data[this.leafNum + i].LChild = x1;
                data[this.leafNum + i].RChild = x2;
            }
        }
    }
}
```

测试代码：

```c#
    class Program
    {
        static void Main(string[] args)
        {
            HuffmanTree ht;
            Console.Write("请输入叶结点的个数：");
            int leafNum = Convert.ToInt32(Console.ReadLine());
            ht = new HuffmanTree(leafNum);
            Console.WriteLine("请依次输入这{0}个叶结点的权值，输入一次按回车键一次", leafNum);
            ht.Create();
            Console.WriteLine("位置\t权值\t父结点\t左孩子结点\t右孩子结点");
            for (int i = 0; i < 2*leafNum -1; i++)
            {
                Console.WriteLine("{0}\t{1}\t{2}\t{3}\t{4}",i,ht[i].Weight ,ht[i].Parent, ht[i].LChild , ht[i].RChild );
            }
            Console.ReadLine();
        }
    }
```

 运行结果：

![img](https://img-blog.csdnimg.cn/20190305111632940.gif)