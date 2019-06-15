
---

title: 设计模式之mvp设计模式

date: 2019-06-15 18:03:17 +0800

tags: 设计模式,mvp,android设计模式

categories: 设计模式

---


<a name="ea6f3b87"></a>
# 参考链接
<a name="MnP5e"></a>
# 参考链接

[《android-architecture》-googlesample](https://github.com/googlesamples/android-architecture/tree/todo-mvp/)

<a name="a1b97d97"></a>
# mvp设计模式

<a name="f411d0f1"></a>
## 说明

  和传统的mvc不同的的是，原先controller的概念变为presenter，原意为“代理”的意思，mvp设计模式中，model和view的交互完全由Presenter进行代理，简单理解就是，View请求Model时，是先发送给Presenter，Presenter收到请求，再发送请求给Model，Model响应数据回Presenter，Presenter再响应回View，此时完成交互。Model请求View也是差不多的过程。<br />  mvp设计模式在Android最为常用。

<a name="58f3538c"></a>
## 传统MVC和MVP之间的图示比较

![图片.png](https://cdn.nlark.com/yuque/0/2019/png/244275/1560593554660-b181f163-839a-46c0-83e0-536e45f829de.png#align=left&display=inline&height=305&name=%E5%9B%BE%E7%89%87.png&originHeight=305&originWidth=612&size=85572&status=done&width=612)

![图片.png](https://cdn.nlark.com/yuque/0/2019/png/244275/1560593560943-ae9636cb-3452-4a4f-ba70-41c2c60bea63.png#align=left&display=inline&height=189&name=%E5%9B%BE%E7%89%87.png&originHeight=189&originWidth=663&size=55824&status=done&width=663)<br /><!--more-->
<a name="a8ea117a"></a>
## google mvp

![图片.png](https://cdn.nlark.com/yuque/0/2019/png/244275/1560593660939-242397ee-9ab1-46cc-b4ca-1ec9a2fc134e.png#align=left&display=inline&height=374&name=%E5%9B%BE%E7%89%87.png&originHeight=374&originWidth=552&size=233997&status=done&width=552)

<a name="BasePresenter"></a>
### BasePresenter

```java
public interface BasePresenter {
    //可以用来初始化相关的数据
    void start();
}
```

<a name="BaseView"></a>
### BaseView

```java
public interface BaseView<T> {
    //view层可以引用Presenter
    void setPresenter(T presenter);
}
```

<a name="module"></a>
### module

<a name="contract"></a>
#### contract
这个是根据具体模块，抽象更多功能的类

```java
public interface TasksContract {
    interface View extends BaseView<Presenter> {
//可以再设计更多抽象方法···············
        void showTasks(List<Task> tasks);
    }
    interface Presenter extends BasePresenter {
//可以再设计更多抽象方法···············
        void loadTasks(boolean forceUpdate);
    }
}
```

<a name="View"></a>
#### View

```java
public class TasksFragment extends Fragment implements TasksContract.View {
    private TasksContract.Presenter mPresenter;
//其他方法略····················
    @Override
    public void showTasks(List<Task> tasks) {
        //Presenter向model获取数据后，将回调此方法，返回有数据的tasks给View层
    }
    @Override
    public void setPresenter(@NonNull TasksContract.Presenter presenter) {
    //Presenter实例化后回调TasksContract.View 中setPresenter()方法，从而View层也获取到Presenter的引用
        mPresenter = checkNotNull(presenter);
    }
}
```

<a name="Presenter"></a>
#### Presenter

```java
public class TasksPresenter implements TasksContract.Presenter {
//数据层data包中的封装类
    private final TasksRepository mTasksRepository;
//view层
    private final TasksContract.View mTasksView;
    public TasksPresenter(@NonNull TasksRepository tasksRepository, @NonNull TasksContract.View tasksView) {
        mTasksRepository = checkNotNull(tasksRepository, "tasksRepository cannot be null");
        mTasksView = checkNotNull(tasksView, "tasksView cannot be null!");
//使View层得到Presenter的引用
        mTasksView.setPresenter(this);
    }
}
```

<a name="Activity"></a>
### Activity

在Activity中初始化Presenter的实现类，即可完成MVP模式的分层,view层只做视图展示，model层负责查询数据，Presenter负责View和model的代理。

```java
// Create the presenter
mTasksPresenter = new TasksPresenter(
        Injection.provideTasksRepository(getApplicationContext()), tasksFragment);
```

