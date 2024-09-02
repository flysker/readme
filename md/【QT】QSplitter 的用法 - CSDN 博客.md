> 本文由 [简悦 SimpRead](http://ksria.com/simpread/) 转码， 原文地址 [blog.csdn.net](https://blog.csdn.net/ffffffeiyu/article/details/136459403)

**1 QSplitter 的用途**

    QSplitter 使得用户可以通过拖动子窗口之间的边界来控制它们的大小，例如

![](https://i-blog.csdnimg.cn/blog_migrate/889026309b4974ad6a4ee5b906b66ab8.png)

                                     图 1 窗口拆分示意图

**2 QSplitter 的添加方法**

    QSplitter 的添加方法有 2 种：a) 通过 [Qt](https://so.csdn.net/so/search?q=Qt&spm=1001.2101.3001.7020) Creator 的界面设计工具添加；b) 直接使用 C++ 代码添加。其中，方法 a 最为直观和方便，本文将重点介绍这种方法，而方法 b 可以见参考资料 [1]。

**2.1 通过 Qt Creator 添加 QSplitter 控件**

    与 Push Button 等控件的添加方法不同，在 “设计” 视图左侧的控件列表中，并没有对应的 QSplitter 控件，而是在上方面的工具栏中，如下图红色圈住的位置所示：

![](https://i-blog.csdnimg.cn/blog_migrate/8017d7df396a2188456ffbf9de182aa6.png)

                                                       图 2.1 设置窗口拆分的按钮

    与上图中左侧的 Push Button 等控件的使用方法不同的是，QSplitter 不可以直接使用拖放的方式将其添加到界面中。根据[参考资料](https://so.csdn.net/so/search?q=%E5%8F%82%E8%80%83%E8%B5%84%E6%96%99&spm=1001.2101.3001.7020) [2][3]的说明可知，使用 QSplitter 之前，需要先添加将被分裂的两个控件，然后同时选中它们，这时候上图的 QSplitter 按钮变成可用状态，点击 “水平分裂器” 即可将它们进行水平的布局。

**2.2 取消 QSplitter**

    取消上述的分裂布局的方法是，同时选中已经被分裂的控件，然后点击工具栏上方的 “打破布局(B)” 按钮即可，如下图所示：

![](https://i-blog.csdnimg.cn/blog_migrate/703fe66ac0c4a3923584d1a78a2854b1.png)

                                          图 2.2 取消窗口拆分的按钮

**3 动态改变子窗口大小**

    默认情况下，使用鼠标拖动分割子窗口间的边界时，子窗口会动态的改变其大小。然而，如果希望在松开鼠标时才改变其大小，可以设置下面的参数，取消其勾选状态即可：

![](https://i-blog.csdnimg.cn/blog_migrate/42cf2c29542909a50feb1641605b0411.png)

**4 子窗口最小尺寸**

    在拖动子窗口间的边界线时，有时我们并不希望子窗口的宽度或者高度被缩小到零，因此可以设置子窗口的最小尺寸：

![](https://i-blog.csdnimg.cn/blog_migrate/6d253e9c4f2cc5c3222aeaea88d72713.png)

                              图 4.1 子窗口属性截图

    然而，就算是设置了上述值，还不行，还需要将下面的选择去掉勾选状态

![](https://i-blog.csdnimg.cn/blog_migrate/9379fe72784273db2546f9b427c5a01a.png)

                               图 4.2 QSplitter 属性截图

**5 子窗口比例**

    默认情况下，QSplliter 中各个子窗口的大小等比例的，但是很多时候我们并不希望这样，因此参考资料 [5][6][7][8] 都提到如何解决这个问题，但都是直接通过 C++ 代码的方式去实现的。这里主要介绍如何通过 Qt [Creator](https://so.csdn.net/so/search?q=Creator&spm=1001.2101.3001.7020) 的 “设计” 界面来达到同样的目的。

    选中 QSplitter 中的子窗口，然后设置其 sizePolicy 属性如下图所示

![](https://i-blog.csdnimg.cn/blog_migrate/aeaf6c525ce767cc8221fa66611f1de2.png)

                           图 5 缩放因子设置

    分别将 QSplliter 中各子窗口的 “水平伸展” 值设置为非零的值。此值越大，表示对应的子窗口在 QSplliter 中的分割比例越大(分割效果要运行程序时才呈现出来)。

**代码实现：**

**QSplitter 类简介**  
QSplitter 类实现了一个拆分部件。一个拆分器 splitter 允许用户通过拖动它们之间的边界来控制子部件的大小。任何数量的部件都可以由单个拆分器 splitter 控制。QSplitter 的典型用途是创建多个部件并使用 insertWidget() 或 addWidget() 添加它们。

以下示例将并排显示 QListView、QTreeView 和 QTextEdit，并带有两个分隔：

```
    QSplitter *splitter = new QSplitter();
    QListView *listview = new QListView();
    QTreeView *treeview = new QTreeView();
    QTextEdit *textedit = new QTextEdit();
    splitter->addWidget(listview);
    splitter->addWidget(treeview);
    splitter->addWidget(textedit);
    splitter->setWindowTitle("Splitter Test");
    splitter->show();
```

 ![](https://i-blog.csdnimg.cn/blog_migrate/c68905cba32d8b0d1729db2005328aed.gif)

如果在调用 insertWidget() 或 addWidget() 时部件已经在 QSplitter 中，它将移动到新位置。 这可用于稍后在拆分器中重新排序部件。 可以使用 indexOf()、widget() 和 count() 来访问拆分器内的小部件。

默认的 QSplitter 是 水平（并排）布置其子项如上例； 可以使用 setOrientation(Qt::Vertical) 将其子项垂直放置。默认情况下，所有部件都可以根据用户的意愿在部件 minimumSizeHint()（或 minimumSize()）和 maximumSize() 之间或大或小。默认情况下，QSplitter 会动态调整其子项的大小。

部件之间大小的初始分布是通过将初始大小乘以拉伸因子来确定的。 您还可以使用 setSizes() 来设置所有部件的大小。 函数 size() 返回用户设置的大小。 或者，您可以分别使用 saveState() 和 restoreState() 从 QByteArray 保存和恢复部件的大小。

当 hide() 一个子部件时，它的空间将分配给其他子部件。 当您再次 show() 它时，它将被恢复。

**简单 Demo 和说明：**

****![](https://i-blog.csdnimg.cn/blog_migrate/d3dd85f4ec43bc33ae15ea8b9321ead5.gif)****

```
#include "mainwindow.h"
#include <QApplication>
#include <QSplitter>
#include <QTextEdit>
 
int main(int argc, char *argv[])
{
    QApplication a(argc, argv);
    QFont font("微软雅黑",15);
    a.setFont(font); // 设置字体
 
    // 主分割窗口
    // 新建一个 QSplitter 对象 作为主分割窗口，水平分割窗口
    QSplitter *splitterMain = new QSplitter(Qt::Horizontal,nullptr); // 水平布置
 
    // 新建 一个 QTextEdit 对象  并将其加入 主分割窗口中
    QTextEdit *textLeft = new QTextEdit(QObject::tr("Left Widget"),splitterMain);
//    QTextEdit *textLeft = new QTextEdit(QObject::tr("Left Widget"));
//    splitterMain->addWidget(textLeft);
    textLeft->setAlignment(Qt::AlignCenter); // 设置 QTextEdit 对齐方式   AlignCenter 居中对齐
 
    // 右分割窗口
    QSplitter *splitterRight = new QSplitter(Qt::Vertical,splitterMain); // 垂直布置 其父窗口为 splitterMain
//    QSplitter *splitterRight = new QSplitter(Qt::Vertical);
//    splitterRight->setParent(splitterMain);
 
    // 调用 setOpaqueResize 用于设定分割窗口的分割条在拖拽是是否实时更新显示。
    // true 实时显示，false则拖拽时只显示一条灰色的粗线，拖拽到位并释放鼠标后显示分割条。默认 为true
    splitterRight->setOpaqueResize(false);
 
 
    QTextEdit *textTop= new QTextEdit(QObject::tr("Top Widget"),splitterRight);
    textTop->setAlignment(Qt::AlignCenter);
    QTextEdit *textBottom= new QTextEdit(QObject::tr("Bottom Widget"),splitterRight);
    textBottom->setAlignment(Qt::AlignCenter);
 
    splitterMain->setStretchFactor(1,1);
    splitterMain->setWindowTitle(QObject::tr("Splitter Test"));
    splitterMain->show();
 
//    MainWindow w;
//    w.show();
    return a.exec();
}
```

**`QSplitter::setStretchFactor(int index, int stretch)`**

```
void QSplitter::setStretchFactor(int index, int stretch)
Updates the size policy of the widget at position index to have a stretch factor of stretch.
stretch is not the effective stretch factor; the effective stretch factor is calculated by taking the initial size of the widget and multiplying it with stretch.
This function is provided for convenience. It is equivalent to
 QWidget *widget = splitter->widget(index);
 QSizePolicy policy = widget->sizePolicy();
 policy.setHorizontalStretch(stretch);
 policy.setVerticalStretch(stretch);
 widget->setSizePolicy(policy);
See also setSizes() and widget().
 
```

更新位置索引处的部件的大小策略以具有拉伸因子。参数 stretch 不是有效拉伸系数； 有效拉伸因子是通过取部件的初始大小并将其乘以拉伸来计算的。

位置索引按插入的先后次序从 0 起依次编号。在这里 setStretchFactor(1,1); 中第一个 1 是位置索引表示第二个插入 splitterMain 的 · 部件也就是 splitterRight, 第二个 1 表示此控件也就是 splitterMain 可伸缩控件。也就是当整个对话框的宽度发生改变时，左部分的文件编辑框宽度保持不变，右部分的分割窗口宽度随整个对话框大小的改变进行调整。

[QSplitter 的用法（Qt Creator 用法及代码示例）](https://blog.csdn.net/ffffffeiyu/article/details/136255493?spm=1001.2014.3001.5501 "QSplitter的用法（Qt Creator用法及代码示例）")