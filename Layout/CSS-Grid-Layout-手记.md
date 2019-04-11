[TOC]

# CSS Grid Layout 手记（教程指南）

CSS Grid Layout（网格/栅格布局）是 CSS 最强大的布局系统，随着设备和浏览器的升级，将会是未来的主流的布局方案之一。

## 一、介绍(Introduction)

### 概述(Basic)

CSS Grid Layout 简称为`Grid`，网上普遍译为`网格`，其实更规范的说法应为`栅格`。

`栅格`对有经验的 UI 和 FE 来说并不陌生，肯定会想到`栅格系统(Grid Systems)`。

这“系统”早在欧洲中世纪就已经出现，当时主要用于文字排版，随着时间的推移逐步发展到平面及其他“设计”领域。

在我印象中 08~09 年左右，网页`栅格化设计(Grid Design)`已经出现并被广泛使用，所定义的`12栅格`、`24栅格`等规范方案仍一直被沿用至今。用通俗的方式来讲：就是以一定规则的网格阵列，规范化网页布局。

CSS Grid`包含了`栅格化设计的特性，是第一个专门为解决布局问题而创建的`CSS 模块`。如果你熟悉`栅格系统`和`栅格化设计`那对`栅格布局`的理解会起到一定帮助。

好的栅格布局，对于模块、元素而言都有规律可循，能在不同尺寸/分辨率下呈现最优的方式，使网页更灵活和易于扩展。但同时也考验设计师，对于不同尺寸的把握，针对前端来说也是如此。

之前使用栅格需要用到`Bootstrap`等的框架，并且每个框架都有一套属于自己的`栅格规范`，现在网格布局使我们可以更为简单的实现，无需依赖框架。

### 布局对比(CSS Grid VS Flexbox)

如果用过 Flexbox，就会发现两者在布局上有很多相似性，都是由父容器来控制子元素的显示和布局。

Flexbox 弹性布局：

- `一维布局系统`，基于轴线，但只能沿着坐标轴的一个方向进行布局。
- 主要作用是通过父容器调节子项目的`宽`、`高`和`排列顺序`，以最佳方式`填充父容器内的空间`，从而实现`自适应`。
- 适合`组件`与`小规模的布局`。

CSS Grid 网格布局：

- `二维布局系统`，同样基于轴线，但能同时沿着 X 轴 和 Y 轴 进行布局。
- 它将容器划分成一个个网格（类似 Table/Excel 表格），实现了`行(row)`和`列(column)`，形成不同的空间。通过网格间的不同组合（合并单元格），实现多样的布局。
- 适合`框架`与`较大规模的布局`。

> CSS Grid 不是万能，也不能取代 Flexbox。正确的操作方式应为结合 Flexbox 一起使用，发挥双剑合璧的作用。

## 二、重要术语(Important Terminology)

为了更好的学习 CSS Grid，需要对一些重要术语有所了解。

![](https://file.bluesdream.com/wp-content/uploads/2019/04/css-grid-layout.png)

### 网格容器(Grid Container)

```html
<div class="container">
  <div class="item"></div>
  <div class="item"></div>
  <div class="item"></div>
</div>
```

使用网格布局的区域`container`，称为网格容器(Grid Container)。

### 网格项(Grid Item)

```html
<div class="container">
  <div class="item"></div>
  <div class="item">
    <p class="sub-item"></p>
  </div>
  <div class="item"></div>
</div>
```

网格容器的所有`直接子元素`，都是网格项(Grid Item)。这里的`item`就是直接子元素，但不包括像`sub-item`这样的子孙元素。

- 网格项对`float`、`display: inline-block`、`display: table-cell`、`vertical-align`和`column-*`等设置都将失效。
- 网格项本身也可以是容器，通过嵌套所产生的元素，称之`子网格(Subgrids)`。

### 网格线(Grid Line)

![](https://file.bluesdream.com/wp-content/uploads/2019/04/css-grid-line.png)

网格的分隔线，称为网格线。

### 网格轨道(Grid Track)

![](https://file.bluesdream.com/wp-content/uploads/2019/04/css-grid-track.png)

容器内的两条`相邻`网格线之间的区域，称为网格轨道。分成横向轨道和纵向轨道，也就是网格中的`行(row)`和`列(column)`。

### 网格间隙(Grid Gap)

![](https://file.bluesdream.com/wp-content/uploads/2019/04/css-grid-gap.png)

网格线的大小，就是网格间隔，按照栅格系统的说法也称为`水槽(Gutter)`。用水槽于分割网格轨道，最直白的理解就是`行间距`和`列间距`。

### 网格单元格(Grid Cell)

![](https://file.bluesdream.com/wp-content/uploads/2019/04/css-grid-cell.png)

两个相邻的行网格线和两个相邻的列网格线之间的空间（行和列`交汇构成的区域`），在概念上类似与 Table `单元格`，称为网格单元格。

### 网格区域(Grid Area)

![](https://file.bluesdream.com/wp-content/uploads/2019/04/css-grid-area.png)

由一个或者多个`网格单元格`组合而成的区域，称为网格区域。

## 三、目录(Table of Contents)

网格的结构，比如有多少行、多少列以及他们的大小，都由网格容器的属性来控制。网格项的位置由该容器内子元素的属性来控制。它们都仅为上下级关联，属性定义不会影响到子孙元素和其他元素。

> 对于新手而言，建议下载新版的 Firefox，DevTools 可以帮助你查看网格线和网格轨迹，并且只针对使用了CSS Grid Layout的元素才显示。虽然 Chrome 也支持，但显示效果略微欠缺。

### 1. 网格容器(Grid Container)

通过将`display`属性设置为`gird`或`inline-grid`来创建网格容器。

默认情况下容器元素都是块级元素。

```css
.container {
  display: grid;
}
```

[DEMO 1-1](https://codepen.io/zhonglimh/pen/LvYaPN)

把值设置为`inline-grid`就变成了行内元素。

```css
.container {
  display: inline-grid;
}
```

[DEMO 1-2](https://codepen.io/zhonglimh/pen/ZZEZXK)

### 2. 显示网格(Explicit Grid)

使用`grid-template-rows`、`grid-template-columns`、`grid-template-area`属性，可以定义网格`固定`数量的线和轨道，这些被`手动`定义过的网格，称为显示网格。

`grid-template-rows`、`grid-template-columns`属性，定义了网格轨道的大小（即：行高和列宽)，及网格线名称，但名称是可选参数。

```css
/* 官方语法 */
none | <track-list> | <auto-track-list>
/* 语法解释 */
<track-list> = [ <line-names>? [ <track-size> | <track-repeat> ] ]+ <line-names>?
```

#### 2.1 行高设置(Row Track Sizes)

```css
.container {
  display: grid;
  grid-template-rows: 150px 100px;
}
```

在本例中：
因为只定义了 2 行轨道值，即 item-1、2，所以剩余的 item 的轨道高度由自身内容而定。

[DEMO 2-1](https://codepen.io/zhonglimh/pen/ROwdbV)

#### 2.3 列宽设置(Column Track Sizes)

```css
.container {
  display: grid;
  grid-template-columns: 150px 100px 200px;
}
```

在本例中：
虽然只定义了 3 列轨道值，但剩的 item 都与 item-1、2、3 在同一列轨道上，所以 item-4 的列宽等于 item-1，类似 table 的列宽设置。

[DEMO 2-2](https://codepen.io/zhonglimh/pen/zXYXJW)

#### 2.3 fr单位(Fgrid Spanr Unit)

`<track-size>` 除了使用常规的`px`,`%`,`em`等长度单位之外，还可以使用`fr`。

`fr`为fraction(片段/一小部分)的缩写，表示网格容器中空间的一小部分。是一个伸缩/柔性长度(Flexible Lengths)单位，响应剩余的空间，该单位的使用让网格创建更为灵活，但`fr`不能在`calc()`等函数中使用。

```css
.container {
  display: grid;
  grid-template-columns: 60px 1fr 2fr 25%;
}
```

在本例中：
第2列为`1fr`，第3列为`2fr`，表示第3列所占空间是第2列的两倍。由于`fr`是基于剩余空间计算，所以要先减去第1列、第4列的固定尺寸的大小。
网格容器宽度为 500px，即：1fr = ((容器宽度：500) - (第1列宽度：60) - (第4列宽度：500*25%)) / 3 = 105

[DEMO 2-3](https://codepen.io/zhonglimh/pen/oONrvL)

#### 2.4 网格线命名(Naming lines)

可以通过`[]`给网格线指定名称，并且每条网格线允许有多个名字，只需要用空格`[*-name1 *-name2]`隔开即可。

在构建复杂的网格布局时，给网格线命名会变的很有必要，其主要目的是为了便于网格项的定位，此时会让代码更易读也更容易被使用。

```css
.container {
  display: grid;
  grid-template-rows: [row-first] 1fr [row-second] 1fr [row-third] 1fr [row-last];
  grid-template-columns: [col-first] 1fr [col-first-end col-second] 1fr [col-third] 1fr [col-last];
}
```

在本例中：
由于是 3x3 的九宫格布局，所以水平和垂直方向各有 4 条线，因此有 8 个命名，并且纵向的第2根网格线有两个名称。

[DEMO 2-4](https://codepen.io/zhonglimh/pen/vMNydw)

#### 2.5 最大和最小网格轨道(Minimum and Maximum Grid Track Sizes)

`minmax()`函数可以接受 2 个参数，第一个参数表示`最小值`，第二个参数表示`最大值`。除了设置固定长度外，值还可以设置为`auto`，允许轨道根据内容的大小延长或拉伸。

但要注意的是，最小值不能是一个`Flexible Lengths`单位，例如`fr`。

```css
.container {
  display: grid;
  grid-template-rows: minmax(100px, auto);
  grid-template-columns: minmax(auto, 50%) 1fr 50px;
}
```

在本例中：
创建了1行3列的网格布局。
第1行最小行高：`100px`，最大行高：`auto`，允许高度随内容而增长。
第1列宽度：`auto`，但最大不超过`50%`；第2列宽度：`1fr`；第3列宽度：50px。

[DEMO 2-5](https://codepen.io/zhonglimh/pen/WWNBjZ)

### 3. 隐式网格(Implicit Grid)

显示网格和隐式网格，在网格布局中是个十分重要的概念。

当网格项被放置在显示网格之外时，就会创建隐式网格。将自动生额外轨道的（空）网格轨道，把没有显式放置的网格项放入其中，此时对应的网格轨道和网格项，也就称为`隐式网格轨道`、`隐式网格项`。最直白的解释就是，没有使用`grid-template-rows`、`grid-template-columns`、`grid-template-area`属性定义过的行和列。

#### 3.1 隐式网格轨道大小(Track Sizes)

使用`grid-auto-rows`、`grid-auto-columns`属性，可以定义隐式网格轨道的行高、列宽，如果不设置它们的值，则和显示网格一样，大小由自身内容而定。

```css
.container {
    display: grid;
    grid-template-rows: 50px;
    grid-template-columns: 1fr 1fr 1fr;
    grid-auto-rows: 100px;
}
```

在本例中：
为了更好辨认，结构上去除了1个网格项，现在只有8个。
然后，创建了个`1行3列的显示网格`，第1行高度为 50px 但只够放置 item-1、2、3 这三个网格项。
那么剩余的 item-4、5、6、7、8，将被放入自动创建的额外轨道（隐式网格轨道）内，成为隐式网格项，隐式网格轨道的行高根据定义为 100px。

注：由此示例可以发现，隐式网格并非单纯指类似指最后个空的网格项，也包含已被填充的网格项，即未被显示放置的所有网项。

[DEMO 3-1](https://codepen.io/zhonglimh/pen/QPKWzG)

#### 3.2 隐式网格项排列(Item Direction)

使用`grid-auto-flow`属性，可以控制隐式网格项的放置（排列）方式。默认`row`，但也可以设置 `column`。

```css
.container {
    display: grid;
    grid-template-columns: 100px 150px;
    grid-auto-columns: 1fr;
    grid-auto-flow: column;
}
```

在本例中：
创建了2列显示网格轨道，分别放置了 item-1、2 两个网格项。并设置了隐式网格轨道列宽为 1fr，放置顺序为纵向排列。

[DEMO 3-2](https://codepen.io/zhonglimh/pen/ROGWre)

### 4. 重复网格轨道(Repeating Grid Tracks)

如果我们要创建N个同样大小的网格轨道，按照之前的写法就需要写N次才行，使用`repeat()`就可以非常方便的简化这个操作。

#### 4.1 基本示例(Basic Example)

`repeat()`函数可以接受两个参数，第一个参数表示重复次数，第二个参数表示重复的值。

```css
.container {
  display: grid;
  grid-template-rows: repeat(3, 100px);
  grid-template-columns: repeat(3, 1fr);
}
```


在本例中：
定义了行高为`100px`并重复`3`次，定义了列宽为`1fr`并重复`3`次。
如果按照常规写法，行高和列宽的值分别要写成`100px, 100px, 100px`和`1fr, 1fr, 1fr`。

[DEMO 4-1](https://codepen.io/zhonglimh/pen/rbVaxe)

#### 4.2 扩展示例(Extended Example)

`repeat()`第二个参数还可以定义多个值，与其他长度单位一起使用，组成特定规则来使用。

```css
.container {
  display: grid;
  grid-template-columns: 50px repeat(2, 30px 1fr) 50px;
}
```

在本例中：
前后轨道列宽各为`50px`，中间根据所定的`repeat()`规则进行重复布局。

[DEMO 4-2](https://codepen.io/zhonglimh/pen/mgJKdg)

#### 4.3 auto-fill关键字(auto-fill Keyword)

`auto-fill`为自动填充模式。如果不想把轨道数量定死，想据容器的宽度来自动填充（实现自适应布局），在可用空间内尽可能容纳多的轨道，防止网格项溢出容器，就需要用到该关键字（接近 float 布局模式）。

注：同时还有一种填充方式`auto-fit`，两者关系较为特殊，此处暂不扩展阐述。可以查看[auto-fill vs auto-fit](https://codepen.io/SaraSoueidan/pen/JrLdBQ/)示例进行对比。

```css
.container {
  display: grid;
  grid-template-columns: repeat(auto-fill, 100px);
}
```

在本例中：
由于我们网格容器整体宽度为`500px`，并定义了每个列宽为`100px`，所以自动填充了`5`列，超出部分自动换行。

[DEMO 4-3](https://codepen.io/zhonglimh/pen/mgrVOe)

### 5. 网格间隔(Grid Gaps)

水渠（间隔）大小可以是任何非负数的`px`,`%`,`em`等长度值，用于分割轨道。

#### 5.1 基本示例(Basic Example)

`grid-row-gap`、`grid-column-gap`属性，用于设置横向/纵向轨道间的间隔，也就是`行间距`和`列间距`。

```css
.container {
  display: grid;
  grid-template-columns: repeat(3, 1fr);
  grid-row-gap: 40px;
  grid-column-gap: 20px;
}
```

在本例中：
行间距为`40px`，列间距为`20px`。

[DEMO 5-1](https://codepen.io/zhonglimh/pen/dLogKE)

#### 5.2 属性简写-双值(Shorthand Property - Two value)

`grid-gap`是`grid-row-gap`和`grid-column-gap`的简写属性语法，第一个值表示行间距，第二个值表示列间距。

注：新标准中，间隔属性的 grid- 前缀被移除（grid-row-gap => row-gap，grid-column-gap => column-gap，grid-gap => gap），Chrome 68、Safari 11.2 Release 50 和 Opera 54 已经支持无前缀的属性。

```css
.container {
  display: grid;
  grid-template-columns: repeat(3, 1fr);
  grid-gap: 40px 20px;
}
```

在本例中：
等价于`grid-row-gap: 40px; grid-column-gap: 20px;`。

[DEMO 5-2](https://codepen.io/zhonglimh/pen/gyawLz)

#### 5.3 属性简写-单值(Shorthand Property - One value)

```css
.container {
  display: grid;
  grid-template-columns: repeat(3, 1fr);
  grid-gap: 20px;
}
```

在本例中：
如果只写一个值，则表示行间距与列间距为都`20px`，等价于`grid-gap: 20px 20px;`，类似 border-width 的简写属性语法。

[DEMO 5-3](https://codepen.io/zhonglimh/pen/mgerZa)

### 6. 网格定位(Positioning Items)

#### 6.1 根据网格线定位(Items by Grid Lines)

网格项可以根据`网格线序号`和`网格线名称`来指定其所在容器内的位置。

`grid-row-start`和`grid-column-start`分别表示`水平网格线的起始位置`和`垂直网格线的起始位置`。

`grid-row-end`、`grid-column-end`分别表示`水平网格线的结束位置`和`垂直网格线的结束位置`。

##### 6.1-1 根据编号(by Number)

每根网格线从起始位开始到结束位，都会有一个自增长的编号，类似ID。

同一轴线方向，只指定单个位置时，默认为跨单个（行/列）轨道，类似“移动”。

```css
.container {
  display: grid;
  grid-template-columns: 1fr 1fr 1fr;
}
.item-1 {
  grid-row-start: 2;
  grid-column-end: 3;
}
```

在本例中：
item-1横轴`向下`移动到第 2 根网格线(line-2)位置，纵轴`向右`移动到第 3 根网格线(line-3)位置结束。
即：横向移动 1 个轨道，纵向移动 1 个轨道。

[DEMO 6.1-1](https://codepen.io/zhonglimh/pen/JVYErP)

##### 6.1-2 根据名称(by Name)

网格项使用`网格线名称`定位，相比`网格线编号`更语义化，在复杂网格内，更方便调用。

```css
.container {
  display: grid;
  grid-template-rows: [row-line-1] 1fr [row-line-2] 1fr [row-line-3] 1fr [row-line-4];
  grid-template-columns: [col-line-1] 1fr [col-line-2] 1fr [col-line-3] 1fr [col-line-4];
}
.item-1 {
  grid-row-start: row-line-2;
  grid-column-end: col-line-3;
}
```

在本例中：
使用网格线名称代替了网格线编号，通过合理的命名规范，更容易理解。
同样也可以使用`grid-row`属性来简写。

[DEMO 6.1-2](https://codepen.io/zhonglimh/pen/JVYOoz)

##### 6.1-3 跨越轨道(Across The Tracks)

同一轴线方向，分别指定起始和结束位置时，可以跨越多个（行/列）轨道，类似“移动并合并”。

```css
.container {
  display: grid;
  grid-template-columns: 1fr 1fr 1fr;
}
.item-1 {
  grid-row-start: 3;
  grid-row-end: 5;
  grid-column-start: 2;
  grid-column-end: 4;
}
```

在本例中：
item-1`所占空间`为左起第 3 根线到第 5 根线，上起第 2 根线到第 4 根线。
即：item-1 偏移并占据到，横向 3 和 4 轨道，纵向的 2 和 3 轨道。

[DEMO 6.1-3](https://codepen.io/zhonglimh/pen/MRapGa)

##### 6.1-4 span关键字(span Keyword)

`span`关键词表示`跨越/跨度`的意思。

```css
.container {
  display: grid;
  grid-template-columns: 1fr 1fr 1fr;
}
.item-1 {
  grid-row: 3 / span 2;
  grid-column: 2 / span 2;
}
```

在本例中：
`span`表示第一根线的位置。
即，轨道行：第 3 根线开始，往后跨越 2 根线；轨道列：第 2 根线开始，往后跨越 2 根线。

[DEMO 6.1-4](https://codepen.io/zhonglimh/pen/EJVXYO)

##### 6.1-5 简写(Shorthand Property)

`grid-row`是`grid-row-start`和`grid-row-end`的简写属性语法。

`grid-column`是`grid-column-start`和`grid-column-end`的简写属性语法。

注：如果有 2 个值，那么值与值之间必须用`/`隔开。

```css
.container {
  display: grid;
  grid-template-columns: 1fr 1fr 1fr;
}
.item-1 {
  grid-row: 3 / 5;
  grid-column: 2 / 4;
}
```

在本例中：
同上个示例一样，只是简化了语法。

[DEMO 6.1-5](https://codepen.io/zhonglimh/pen/dLYWZw)

#### 6.2 根据网格区域定位(Items by Grid Areas)

网格项除了使用`网格线名称`、`网格线编号`，也可以根据`网格区域`来定位。

##### 6.2-1 根据编号((by Number)

`grid-area`属性，是`grid-row-start`、`grid-column-start`、`grid-row-end`、`grid-column-end`的简写属性语法。

```css
.container {
  display: grid;
  grid-template-columns: 1fr 1fr 1fr;
}
.item-1 {
  grid-area: 3 / 2 / 5 / 4;
}
```

在本例中：
和网格线定位一样，实现了 item-1 占据4个单元格，并移动到最后。

[DEMO 6.2-1](https://codepen.io/zhonglimh/pen/MRadvJ)

##### 6.2-2 根据名称(by Name)

与网格线一样，网格区域可以使用`grid-template-areas`属性，给网格区域命名。

同一行命名用单引号或双引号包裹起来，名称间用空格隔开（有点类似 markdown 的表格绘制）。然后就可以根据，所创建的区域名称，指定网格项所在轨道的位置。

注：每个区域的起始网格线会自动命名为`区域名-start`，结束网格线会自动命名为`区域名-end`。

```css
.container {
  display: grid;
  grid-template-columns: 1fr 1fr 1fr;
  grid-template-areas: "header header header"
                       "main   main   sidebar"
                       "footer footer footer";
}
.item-1 {
  grid-area: header;

  /* 等价于 */
  grid-row:    footer;
  grid-column: footer;

  /* 等价于 */
  grid-row-start:    header;
  grid-row-end:      header;
  grid-column-start: header;
  grid-column-end:   header;
}
.item-2 {
  grid-area: main;
}
.item-3 {
  grid-area: sidebar;
}
.item-4 {
  grid-area: footer;
}
```

在本例中：
网格项改为4个，分别对应`header`、`main`、`sidebar`、`footer`四个区域，通过`grid-area`指定引用的区域名称。现在，一个常规网页布局已经完成。

[DEMO 6.2-2](https://codepen.io/zhonglimh/pen/wZKPrq)

##### 6.2-3 空单元格(Null Cell)

如果要改变下网格结构，例如`header`两边需要留空，则可以使用`.`号，代表一个单元格，属于隐式网格(Implicit Grid)。

```css
.container {
  display: grid;
  grid-template-columns: 1fr 1fr 1fr;
  grid-template-areas: ". header ..."
                       "main   main   sidebar"
                       "footer footer footer";
}
.item-1 {
  grid-area: header;
}
.item-2 {
  grid-area: main;
}
.item-3 {
  grid-area: sidebar;
}
.item-4 {
  grid-area: footer;
}
```

在本例中：
网格项改为5个，并且不定义 item-5 的位置。
`header`区域的两边使用`.`进行单元格占位，`...`因为之间没有空格等同于`.`，还是一个独立的单元格，并表示该轨道的单元格没有被使用（占位留空）。
但如果有多余未分配的网格项，且未做处理则会自动填充进空位。

[DEMO  6.2-3](https://codepen.io/zhonglimh/pen/pBjmEy)

### 7 网格项放置算法(Items Placement Algorithm)

#### 7.1 遇到问题(Problems)

在不设置网格项定位的时候，网格项会通常会规则、紧密的排列在一起。可一但设置过定位（试图跨越轨道），后续网格项会进入新的轨道项进行排序，从而导致布局看起来层次不齐。

```css
.container {
  display: grid;
  grid-template-columns: 1fr 1fr 1fr;
}
.item-1 {
  grid-column: 1 / 3;
}
.item-2 {
  grid-column: 1;
}
.item-3 {
  grid-column: 1 / 3;
}
```

在本例中：
可以看到，虽然 item-2、3 但设置了网格项定位之后，导致了第二、第三行结尾处出现“空网格项”。

[DEMO 7-1](https://codepen.io/zhonglimh/pen/NmRRya)

#### 7.2 dense关键字(dense Keyword)

`dense`关键字，则会自动尝试让剩余的网格项，优先填充至空网格项中。

注：`dense`会引起网格项的不规则排序，并且是根据轨道大小进行填充。可以尝试下将 item-1 宽度设置为`width: 600px`看看会有什么效果。

```css
.container {
  display: grid;
  grid-template-columns: 1fr 1fr 1fr;
  grid-auto-flow: dense;
}
.item-1 {
  grid-column: 1 / 3;
}
.item-2 {
  grid-column: 1;
}
.item-3 {
  grid-column: 1 / 3;
}
```

在本例中：
表示根据“先行后列”的规则进行填充，并尽可能的紧密填满。也可以写成`row dense` `column dense`，官方语法为`grid-auto-flow: [ row | column ] || dense`。

[DEMO 7-2](https://codepen.io/zhonglimh/pen/oOzzdN)

#### 7.3 网格项目分层(Layering)

网格项在必要时可以结合`z-index`进行分层/堆叠，针对显示/隐式网格线定位、网格区域定位都有效果。

```css
.container {
  display: grid;
  grid-template-columns: 1fr 1fr 1fr;
}
.item-1,
.item-2 {
  grid-row-start:  1;
  grid-column-end: span 2;
}
.item-1 {
  background: rgba(234,42,122,.5);
  grid-column-start: 1;
  z-index: 1;
}
.item-2 {
  grid-column-start: 2
}
```

在本例中：
item-1 和 item-2 都设置为跨越 2 个轨道，并且两者进行了重叠。在默认情况下 item-2 会覆盖在 item-1 之上，此时通过`z-index`进行层级定位。

[DEMO 7-3](https://codepen.io/zhonglimh/pen/PgGGvK)

### 8. 网格对齐(Grid Alignment)

CSS Grid 允许项目纵轴和横轴进行对齐。

HTML文档中行，行内元素的默认方向是从左到右，块级元素默认方向是从上到下。为了便于理解，直接用横(X)轴表示`行`，纵(Y)轴表示块`块`。

#### 8.1 网格容器的对齐(Aligning The Grid)

网格容器的对齐属性，主要有以下几种值：
* normal: 默认值，正常对齐
* start: 轴线的起始位置
* end: 轴线的结束位置
* center: 水平居中
* stretch: 自动拉伸，适应容器大小
* space-around: 平均分布，两端间隔是相邻元素间隔的一半
* space-between: 平均分布，两端对齐
* space-evenly: 均匀分布，轨道间隔相等

##### 8.1-1 纵轴对齐(align-items)
`align-items`属性，控制网格项在当前行的纵轴方向上的对齐方式。
```css
/* 语法 */
align-items: center | start | end | stretch
```

##### 8.1-2 横轴对齐(justify-items)
`justify-items`属性，控制网格项在当前行的横轴方向上的对齐方式。

```css
/* 语法 */
justify-items: center | start | end | stretch
```

##### 8.1-3 属性简写(place-items)
`place-items`属性，是`align-items`、`justify-items`属性的简写属性语法。

```css
/* 语法 */
place-items: <align-items> / <justify-items>
```

##### 8.1-3 子元素纵轴对齐(align-content)
`align-content`属性，控制子元素在纵轴方向上的对齐方式。

```css
/* 语法 */
align-content: center | start | end | space-between | space-around | space-evenly
```

##### 8.1-3 子元素横轴对齐(justify-content)
`justify-content`属性，控制子元素在横轴方向上的对齐方式。

```css
/* 语法 */
justify-content: center | start | end | space-between | space-around | space-evenly
```

##### 8.1-3 属性简写(place-content)
`place-content`属性，是`align-content`、`justify-content`属性的简写属性语法。

```css
/* 语法 */
place-content: <align-content> / <justify-content>
```

#### 8.2 网格项的对齐(Aligning The Grid Items)

网各项的对齐属性，主要有以下几种值：
* normal: 默认值，正常对齐
* start: 轴线的起始位置
* end: 轴线的结束位置
* center: 水平居中
* stretch: 自动拉伸，适应网格项大小

##### 8.2-1 纵轴对齐(align-self)
`align-self`属性，控制网格项在纵轴（侧轴）方向上的对齐方式。

```css
/* 语法 */
align-self: center | start | end | stretch
```

##### 8.2-2 横轴对齐(justify-self)
`justify-self`属性，控制网格项在横轴（主轴）方向上的对齐方式。

```css
/* 语法 */
justify-self: stretch | start | end | center
```

##### 8.2-3 属性简写(place-self)
`place-self`属性，是`align-self`、`justify-self`属性的简写属性语法。

```css
/* 语法 */
place-self: <align-self> / <justify-self>;
```

### 9. 布局属性简写(Shorthand Property)

#### 9.1 网格布局(grid-template)

`grid-template`是`grid-template-columns`、`grid-template-rows`、`grid-template-areas`的简写属性语法。

注：`grid-template`不会重置隐式网格(Implicit Grid)属性`grid-auto-columns`、`grid-auto-rows`、`grid-auto-flow`，所以大部分情况下还是建议使用`grid`来代 `grid-template`。

```css
grid-template: 20vh 1fr 20vh / 15vw 1fr 10vw;

/* 等价于 */
grid-template-rows: 20vh 1fr 20vh;
grid-template-columns: 15vw 1fr 10vw;
grid-template-areas: none; /* 默认值 */
```

#### 9.2 网格布局(grid)

`grid`是`grid-template-rows`、`grid-template-columns`、`grid-template-areas`、`grid-auto-rows`、`grid-auto-columns`、`grid-auto-flow`的简写属性语法。

```css
grid: auto-flow 20vh / repeat(3, 30vw);

/* 等价于 */
grid-template-rows: auto-flow 20vh;
grid-template-columns: repeat(3, 30vw);
grid-template-areas: none; /* 默认值 */
grid-column-gap: 0; /* 默认值 */
grid-row-gap: 0; /* 默认值 */
```

## 四、兼容性(Browser Support)

桌面(Desktop) 浏览器

| Chrome | Firefox |       IE       | Edge | Safari | Opera |
| :----: | :-----: | :------------: | :--: | :----: | :---: |
|  57.0  |  52.0   | 10(旧语法规范) | 16.0 |  10.1  |  44   |

手机(Mobile) / 平板(Tablet)浏览器

| iOS Safari | Android Browser | Chrome for Android |
| :--------: | :-------------: | :----------------: |
|    10.3    |       67        |         71         |

> 其实最早提出 CSS Grid 的是微软，基于微软的尿性，其他浏览器的兼容性反而比 IE 更快。

## 五、参考资料(Reference Materials)

[Learn CSS Grid](https://learncssgrid.com/) - by Jonathan Suh
[A Complete Guide to Grid](https://css-tricks.com/snippets/css/complete-guide-grid/) - by Chris House
[CSS Grid Tutorial](https://www.quackit.com/css/grid/tutorial/) - by Quackit
[A Complete Guide to CSS Grid](https://tympanus.net/codrops/css_reference/grid/) - by Codrops
[CSS Grid Layout](https://developer.mozilla.org/en-US/docs/Web/CSS/CSS_Grid_Layout) - by MDN
[Grid 布局教程](http://www.ruanyifeng.com/blog/2019/03/grid-layout-tutorial.html) - by 阮一峰

## 六、补充(PS)

1. 文本作为自学笔记，已同步至[Github - Ublue前端手记](https://github.com/zhonglimh/Ublue-Notes)。
2. 文章内容不定期更新，Github 上单篇文章 URL 后期可能有所调整，收藏请点 `star` 关注仓库。