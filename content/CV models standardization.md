+++
title = "CV 模型的标准化"
date = 2019-03-09
[taxonomies]
categories = ["Deep Learning", "Computer Vision"]
+++

<h2>序数 or 偏移</h2><p>深度学习第一个坑就是从 0 编号还是从 1 编号.</p><p>虽然大多数框架都选择了从 0 开始, 但我还是得说这是反人类的.</p><p>从零开始计数一般用在偏移中, 比如内存, 每个字节是几偏移.</p><p>但是深度学习就不一样了, 比如标签, 我们通常说是第几个.</p><p>这是序数啊, 从来不会有第 0 个这么奇葩的说法.</p><p>依次交换0维3维1维2维, 我是无力吐槽了, 你和谁的偏移量0312, 是给人看的吗你这...</p><p>推导不能, 反正全给他 +1 就得了.</p><h2>RGB or BGR</h2><p>这个问题无关痛痒,</p><p>我不知道重排首个卷积核的话怎么写, 理论上也是可以的.</p><h2>NCHW or NHWC</h2><p>我是真的不知道 NHWC 的优点在哪, 难道硬件底层更加合适吗?</p><p>NCHW 下取第一张图片就是 <code>img[1]</code>, NHWC 则要写 <code>img[:,:,:,1]</code>, 何苦啊这是...</p><h2>左下系 or 左上系</h2><p>左上系的支持者理由是图片即矩阵, 第一行第一个像素就是 <code>img[:,1,1]</code></p><p>用左下系就要写 <code>img[:,1,-1]</code>, 多了个负号</p><p>哦对了, 某些从零开始计数的垃圾玩意儿压根没法用 <code>-1</code> 表示倒着数.</p><p>(所以为什么不规定为矩阵从下开始数...</p><p>但是不管怎么说数学老师当初教我就是左下建系, 所以这样更符合习惯.</p><h3>解决方案</h3><p>遇到左下系就转换为左上系</p><p>施加变换 <span class="math">$y -> 1-y$</span>  即可, 记得目标检测里 {y_<pre>\min{}</pre>} 和 {y_<pre>\max{}</pre>} 对调一下.</p><h2>右手系 or 左手系</h2><p>如果 2D 用了左上系, 3D 用左手系似乎是天经地义的.</p><p>当我们还是习惯朝对我们的平面是 0, 越往里面越远不是, y 轴越大离摄像头就越远, 景深就越深</p><p>不管怎么说这也是习惯问题罢了, 不是很重要.</p><h3>解决方案</h3><p>遇到左手系就转换为右手系</p><p>乘上变换矩阵 <span class="math">$\begin{bmatrix}1&0&0&0\\0&0&1&0\\0&1&0&0\\0&0&0&1\\\end{bmatrix}$</span> .</p>
