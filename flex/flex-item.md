## flex-item ##
通过`flex-container`,能够大体上安排内部的children的排布情况.css提供了一些直接应用于`flex-item`，能够精确地控制`container`中的单独的个体(`flex-item`)
## 什么是flex-item ##
我们创建`flex-container`只需通过给一个有子节点的元素设置`display:flex或者display:inline-flex`。那么这个元素的子节点就被称为`flex-item`,不论它们是子节点，非空文本节点还是生成内容（::after,::before）,它们都是flex-item.

这里要注意的是对于文本节点，如果是纯粹的whitespace范畴的，那么就会忽略掉，如果只是纯粹的文本，并没有标签包含，那么这个文本节点就会生成`匿名flex-item`，这个匿名的`flex-item`继承所有设置在`flex-container`上的`flex-property`，它们并不能够直接通过css来定位目标，不能够直接设置专属的`flex-item`特定属性在它们上面。

	<p style="display: flex;">
	    <strong>Flex items:</strong> they’re what’s for <em>&lt;br&gt;fast!</em>
	</p>

如上代码，这个`flex-container`生成3个`flex-item`,分别是strong，匿名文本形成的匿名flex-item，em元素。

仅仅只有whitespace的文本节点，比如空格，换行，tab这种，就像被设置成了`display:none`一样，完全忽略。

	nav ul {
		display: flex;
	}
	<nav>
		<ul>
			<li><a href="#1">Link 1</a></li>
			<li><a href="#2">Link 2</a></li>
			<li><a href="#3">Link 3</a></li>
			<li><a href="#4">Link 4</a></li>
			<li><a href="#5">Link 5</a></li>
		</ul>
	</nav>

以上代码中，li元素全部都是`flex-item`，它们看起来在标签形式上还是li，实际上它们已经不是块级元素，而是flex-item了，因为它们都处在了`flex formatting context`的上下文中。值的注意的是，li如果存在后代的li，那么它们还是块级元素。

## flex-item特性 Flex Item Features##
`flex-item`的margin不会重叠，float和clear属性在flex-items上面没有什么效果，并且不会把它们弄出文档流，效果就是，float和clear应用到`flex-item`上会被忽略。（但是float属性仍然通过影响display属性的计算值来影响盒子的生成，比如inline变成block）。

`vertical-align`对flex-item没有用，除了它影响在`flex-item`中的文本。换句话说就是，设置`vertical-align:bottom`在一个`flex-item`将会让`flex-item`里面的文本都对齐到它们`line-box`的底部。

### absolute定位的例外性 ###
上面说了`float`并不能去浮动`flex-item`,但是`absolute`就不一样了，在`flex-container`中的绝对定位元素，就像其他的绝对定位元素一样，会被移出文档流。

被绝对定位后，它们不参与`flex-layout`,不是文档流中的一部分。但是，他们他们会被`flex-container`的样式所影响,就像一个子元素会被一个非flex-container的父元素所影响，换句话说，就是继承那些可继承的属性，`flex-container`属性能够影响定位的origin。

绝对定位的子节点，既能够被`justify-content`影响，也能够被它自己的`align-self`所影响。例如,如果你在一个绝对定位的子节点上设置`align-self:center`，它将相对于它父弹性容器的`croxx axis`来居中。这是origin，所以设置left，right，bottom，margin，这些就需要考虑到这个偏移量。

order属性不会影响绝对定位后的子节点在哪里绘制，但是它确实影响相对于它的兄弟来说，该什么时候绘制。

### minimum宽度 ###
对于`flex-item`,min-width已经为重写成`auto`了，已经不是`0`,因此当容器放不下，flex-item默认是不会缩小的。所以当设置`flex-wrap:nowrap`和`flex-wrap:wrap`，如果将min-width又重新设置为0，那么`nowrap`就会当空间不够的时候缩小，直到0为止，而允许换行`wrap`,会缩到刚好容下内容就不会再小了。

### flex-item 特定属性 ###
`flex-item`的特定属性，能够更细粒度去控制flex-item。

flex属性是有3个部件组成，分别是`flex-grow`,`flex-shrink`,`flex-basis`,这个3属性共同控制`flex-item`的弹性。

弹性就是一个`flex-item`沿着主轴扩大或者缩小的量。


### flex的比例计算 ###
flex顾名思义，是弹性的意思，当位置不够的时候，或者当位置多的时候，每个`flex-item`应该怎么去弹,那么计算方式是：

1.如果设置为0，表示不弹性。设置负数整句话都无效。

2.如果设置为正数，不一定非要是整数，`flex-grow`,`flex-shrink`都是相对兄弟的这个值的,假如这里已经将因子比例转化成了整数比例。

多余的距离/因子总数 = 一份因子的。

3.每个`flex-item`的basis 加一份因子`乘以当前这个item的因子`。

4.对于grow的比例非常简单，单纯计算因子就完了不需要考虑flex-item的宽度，但最后需要抛出的问题是，怎么去算shrink,shrink要结合自身的宽度。

比如1000px的宽度有250px，500px，250px，3个块。

这3个块的shrink因子是1，1，3

这1000px要放到750px中，那么少了250px，这250px算出每份因子应该减去多少比例。

shrinkPercent = 总共少的空间/(`item1*shrinkFactor1` + ... + `itemN*shrinkFactorN`);

也就是250/(250 X 1) + (500 X 1) + (250 X 3) = 0.166667%;

item1 = 250 * (1 - 16% X 1) = 208.33

item2 = 500 * (1 - 16% X 1) = 416.67

item3 = 250 * (1 - 16% X 3) = 125

最后就是因子越大，增加的幅度或者减少的幅度也越大。


### basis的计算 ###
basis如果设置为百分比，百分比基于container，如果设置为像素，它的优先级高于`width`，但是受min-widht和max-width的影响，如果设置为auto，那么width的优先级更高。

默认是auto，这个值的意思就是，它默认的宽度就是content需要多宽就多宽，尽量不换行。如果需要缩小，那么先看是否有shrink因子，然后基于本身需要多宽的宽度结合shrink因子，算出缩小的比例。


    <div style="display:flex;">
        <nav>这是导航栏这是一个auto宽度的flexitem</nav>
        <article>这是artical</article>
        <aside>这是aside</aside>
    </div>

比如这一段html，他们需要438.26px像素的宽度，其中第一个占用288.13，第二个占78.3，第三个占71.83，这3个完全不换行，但是如果视口缩小到343了，那么它们分别是多少呢？

以第三个最小的宽度为1基准，第二个是1.09，第三个是4

少的宽度是：438.26-343 = 95.26

95.25被分成4，1.09，1这几个部分，，他们分别是62.56，17.04，15.64

最后第一个宽度 288.13 - 62.56 = 225.57

第二个宽度 78.3 - 17.04 = 61.3

第三个宽度 71.83 - 15.64 = 56.19

总结下来就是说，flex的缩小，可以认为是本身不缩小，它们应该占用多少，然后实际是多少，算出差额，然后再根据每个item的应该宽度结合shrink因子，来算出缩放的比例。过程如上面。

这里要注意的是，如果shrink是0，那么这个item是坚决不会缩小的，但是其他的元素缩小后，会发生换行，那么高度就会变高，那么这个不愿意缩小的item，也会变高，因为默认的container的`align-items:normal`,它的表现跟`strech`相似。


flex-basis为0的时候，注意到基准是0，那么就会将整个container的宽度作为比例来分，比如container是1000px，3个grow的比例是1，2，1，那么他们最后的宽度是250，500，250；因为基准是0.

其余的绝对单位和百分比，都是比较简单的。

###flex###
flex的3个部分的默认值是，flex-grow：0，flex-shrink:1,flex-basis:auto;

flex:1 => flex:1 1 0%;

flex:1 2 => flex: 1 2 0%;

flex:auto => flex:1 1 auto;

flex:none => flex:0 0 auto;

If neither the  flex-basis property nor the  flex shorthand is included at all, the flex
basis defaults to  auto . When the  flex property is included, but the flex basis compo‐
nent of the shorthand is omitted from the shorthand, the basis defaults to  0

如果flex-basis或者flex都没有，那么flex-basis的值默认是auto；

如果flex-basis在flex中省略了，那么flex-basisi是0；

我个人觉得应该写明白。

个人感觉通常`flex:1`用的比较多，其余的时候用得不多，用多了容易困惑。这里比较困惑的是flex-basis的默认值到底是0%还是auto，反正这里最好能够扩写，写清楚，auto和0%还是有一些区别的。

## order ##
这个属性很简单，使用在item上面，然后控制显示的顺序。基本上很少用。