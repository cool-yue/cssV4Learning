## flex ##
以前全部都是通过posittion布局来实现一些特定的布局，css3引入了新的flex布局，下面看看。个人认为flex布局是一个非常重要的局部的，也许因为它的出现，position几乎退出了历史舞台，同时css权威指南写了90页之多，足够说明flex的东西特别多。这一章节由于我个人认为比较有意义，所以准备翻译书中的一些东西。
## Flexible Box Layout ##
css弹性盒子模块等级1，或者简称弹性盒子，使得曾经输出很多种类的页面，组件，应用，图片库这个复杂的事情变得简单。使用FlexBox，你经常不需要一个css框架，使用很少的行的css，基本上能够为你的网站创建任何你想要的特性。
## Flexible Fundamentals ##
FlexBox是一个简单又强大的方式来输出页面组件，通过描述空间如何分布，内容如果对齐，以及元素以何种顺序可视化。这些内容能够被处理垂直方向和水平方向，沿着单一一个轴输出或以多行被包含还有更多效果，通过FlexBox很轻松实现。

使用flexbox，内容的样式跟源顺序是独立的，尽管视觉上的顺序改变，flex属性不应该影响屏幕观看者读取内容的顺序。

也许最重要的是，使用`flexible box module layouts`，元素的行为针对不同屏幕的尺寸和不同的显示设备能够被预知。FlexBox能够非常好地用于响应式网站，正如它的内容能够增加或减少尺寸当提供的空间增加或者减少。

flexbox以父子关系的形式来工作。激活flexbox输出通过申明在`一个元素`上申明`display:flex or display:inline-flex`，申明之后`这个元素`就变成了一个`flex container`,从而安排它的孩子节点，在这个提供的空间内，控制它们的布局输出。在这个`flex container`中的孩子节点变成了`flex items`。

加入一个`flex container`中有很多个div,span,它们是如何变成一个`flex item`呢，为什么他们能够以先相同的方式输出，在一个`flex container`里面，并不会关心某些标签是`p`,某些标签是`span`,某些标签是`div`,它们只要在一个`flex container`中，它们统一全部变成`flex items`(唯一不同的是，它们各自的私有的css属性可以另外设置，比如margin，如果不单另设置其他的值，那么这些孩子元素全部都是等同看成`flex item`)

将一个元素变成`flex-container`，可以通过`display:flex or display:inline-flex`，那么它们有什么区别呢，顾名思义，`flex`会以块元素的形式占据一行，而`inline-flex`会以行内块元素(`inline-block`）的形式，`children`占据多少就是多少。他们的共同点就是，它们的内部的内容全部以`flex-item`形式输出。

一个关键的事情需要记住的是一旦设置了一个元素是一个`flex container`,它只会影响它的直接孩子节点，使他们以`flex-item`的形式输出，而不会影响到更后面的子孙。不过，可以通过将这些更后面的子孙也变成`flex-container`（继续在后代里面设置`display:flex or display:inline-flex`）来得到一些更复杂的输出。

在一个`flex-container`里面，`items`沿着`main axis`排成一排。`main-axis`既可以是水平也可以是垂直，这样就可以以列的形式或者行的形式来处理这些items.`main-axis`的方向通过写`mode`,后面详细讨论。

当`flex items`并没有填充满整个`flex container`轴（也就是width）的时候，会有多余的空间。这里也有属性可以设置怎么来处理这些多余的空间。我们可以让items整体靠左，整体靠右，整体居中或者让它们充分展开，也能定义这些空间分布在children之间或者围绕四周。

除了分配这些多余的空间，也能够让`flex items`变大来占据所有可用的多余空间，可以设置一个，二个，任意个都行。如果没有足够的空间包含所有的`flex-items`,可以设置它们缩小来适应它们的容器空间或者它们是否被允许占用多个flex行。

还有更多，子孙节点能够针对它们的容器或者互相之间进行对齐。对齐容器的底（bottom），顶（top），中间（center），或者完全展开充满容器。不管每个兄弟容器内容的长度是多少，都能够通过一个申明将它们设置成一样的尺寸。

以上大致说了` Flexible Box Layout`能够做到的一些输出。

前面提到的一些，有2个核心的东西，一个是`flex-item`,一个是`flex-container`,从外及内，那么先分析`flex-container`。
## 什么时候用flex ##
flex布局是一个一维布局，它可以让`flex-container`中的元素沿着某个轴来排，如果要以二维布局，那么flex不适合，这个时候需要用到grid布局，也就是网格布局，网格布局另开一篇md来写。
## flex-container ##
当给一个元素设置了`display:flex or display:inline-flex`，那么这个元素就是`flex-container`,然后它里面的内容就是`flex format`形式的来布局，因此指定这个`container`,告之它里面的`flex-items`如果展示下面来列出这一些列的属性。
### flex-direction ###
这个属性顾名思义，就是可以指定你的输出，从top到bottom，从left到right，从right到left，从bottom到top，通过这个属性可以设置一个`main axis`，items全部沿着这个轴来排列。不设置这个属性的时候，这个属性的默认值是`row`,也就是left到right，其余几个值有`row-reverse`,从right到left，`column`，从top到bottom，`column-reverse`,从bottom到top。大致上这个属性的内容就只有这么多。

其他的情况就是，某些语言并不是从左往右写，它可能是从上往下或者从右往左书写，这个称呼为`writing mode`,这个既可以作为css属性写，也可以在标签上面使用`dir`这个属性，例如如果指定某个语言是从上往下书写的，那么`flex-direction:row`,就不会从左往右了，而是遵从语言习惯，从上到下。
### flex-wrap ###
前面的属性，指定了`flex-items`如何排列，但是如果出现了不能适应`main axis`的情况，比如说位置不够，默认情况下，`flex-items`不会换行，也不会进行必要地重新计算`size`。相对来说，`flex-items`最好能够shrink(缩小)，如果允许的话，通过`flex`属性，要不然`flex-items`就会移除包围的容器盒子。为了不让`flex-items`溢出`container`或者`shrink(缩小)`保证都在一行。可以使用`flex-wrap`让其换行。

`flex-wrap`属性控制`flex-container`是否被限制成只有一行的`container`,或者如果需要允许变成多行。

默认情况下`flex-wrap`的值是`nowrap`,其余的2个值，`wrap`和`wrap-reverse`，如果他们被设置了,决定了在"flex-items"的原始行的前后，是否有额外的行。`wrap`设置之后，换行的内容放在原始行的后面，`wrap-reverse`除了换行以外，后面的行放到前面。这些都是针对`左到右`的书写规则的语言的，如果有其他书写规则的语言，同理相应的属性的方向会旋转，类比`direction`.

### flex-flow ###
前面定义了`flex-direction`和`flex-wrap`这2个属性，下面有一个复合属性，第一个值是`flex-direction`,第二个值是`flex-wrap`,默认情况下是`row nowrap`，因此一旦只设定了`display:flex or display:inline-flex`,那么相当于设置了默认的`flex-flow`也会被应用。

## 理解axes ##
<pre>
main axis:内容排列依据的轴，在flexbox中，这是flex-items排列的方向。
main size：沿着main axis的内容的总长度
main start：沿着main axis，内容开始排列的地方
main end：对着main start的地方
cross axis：交叉轴，该轴的方向就是新行被放置的方向。
cross size：交叉轴方向的总长度
cross start：行的起始放置位置
cross end：cross start的对缅

对于从左向右的语言，默认情况下，flex-item的顺序是从左到右，从上到下（设置了换行），也就是main start到main end，cross start到cross end，那么可以知道优先摆放item的位置是main start和cross start，因此有可能空出来的位置就是main end和cross end。好，这一部分空的地方，能不能控制呢，答案是，可以。
</pre>
## Flex Container ##
当flex items没有填满flex container，flex items就会以分组的形式沿着主轴对着main start放置。Flex items也能够对着main end开始放置，放置在中间，最后甚至均匀的放置。

flex layout说明文档为我们提供了flex container属性来控制container空间内容的分布方式，除了`display`和`flex-flow`，还有`justify-content`,`align-content`,`align-items`。

`justify-content`属性控制flex items在一个flex line的范围中，如何沿着main axis来布置items

`align-content`属性定义了flex lines在一个flex container的范围中，如何在cross axis上面布置lines。

`align-items`属性定义了flex items在每个flex line中沿着cross axis如何布置items

## justify Content ##
`justify-content`属性能够让我们指引`flex items`如何在每一个`flex-line`中，沿着`flex-container`中的`main axis`如何来布置`flex items`。这个属性是应用于`flex container`，不应用在`flex items`。

`justify-content`的值定义了周围的空间如何分布，或者在某些盒子中间，也就是`flex items`在一个`flex container`,它有6个值，`flex-start`,`flex-end`,`center`,`space-between`,`space-around`,`space-evenly`,初始化的值也就是默认值是`flex-start`。
### flex-start ###

### flex-end ###
### center ###
### space-between ###
### space-around ###
### space-evenly ###



## align Content ##

## align items ##
align-items属性定义了`flex items`是如何对齐的，沿着`cross axis`,上面提到的`justify-cotent`和`align-items`都应用于`flex-container`。align-items以下几个值,`flex-start`,`flex-end`,`center`,`baseline`,`stretch`这5个属性，默认值是`stretch`.需要注意的是`align items`它们是用于在一行`flex-line`范围内的对齐。如果是`flex-direction:row`就非常好理解了，如果是`flex-direction:column`,`flex-line`就变成了竖直方向，如果数值方向足够，那么相当于整个容器就只有一行，那么对齐的方式就会在这整个`container`作为`flex-line`的范围进行对齐，如果数值方向空间不够，并且设置了`flex-wrap:wrap`,那么可能`flex-items`就有多行，但是由于`flex-direction:column`,因此这时候`container`里面的就会被平分出多个行(这里是列的形式展现)，在每个行中各自对齐。
### 铺垫 ###
注意到`flex-items`在对齐的时候，需要考虑到`margin`,换句话说，`margin`是`flex-items`的一部分，也就是说如果设置了`align-items:center`,假如还有`margin-top:10px`,那么实际上中心点是要往上偏移`5px`的，因为这个时候`flex-items`的中心向上偏移了`5px`，算上了设置的margin，同理对于左右，水方向同样适用。
### flex-start ###
### flex-end ###
### flex-center ###
### flex-baseline ###
baseline这个在某种情况下与flex-start表现出来的感觉是一样的，那就是同一行元素，有相同的行高，相同的margin，相同的baseline的位置，这个baseline对齐的方式是，取出在一行的所有的`flex-items`,然后计算出每个item中第一行文本的baseline距离cross-start最远的那个baseline，然后将其他的同一行的flex-items移动到第一行文本的baseline跟最远距离的baseline对齐
### flex-stretch ###
## align-self ##
该属性用在一个`flex-item`来覆盖`align-items`已经定义好的属性值。`align-self`的属性与`align-items`一样，一个用于个体，一个用于`flex-container`来影响整个内部的`flex-item`。`align-self`的值默认是`auto`,这个auto的值就是继承于父元素的`align-items`的值。
## align-content ##
`align-content`类似于`justify-content`,它们都是定义如何处理多余的空间，`justifiy`是针`main axis`方向，`align`是针对`cross axis`方向。需要注意的是`align-items`是针对每个`flex-line`的范围内。`align-content`顾名思义，如果只有一个`flex-line`那么就没有所谓的效果了，因此`align-content`需要针对多个`flex-line`的`flex-container`。类似于`justify-content`,它的默认是`strech`,它的所有值如下:
### flex-start ###
### flex-end ###
### center ###
### space-between ###
### space-around ###
### space-evenly ###
### stretch###

## 总结 ##