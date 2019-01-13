---
title: GreenDao的使用以及一些细节的总结
date: 2017-02-20 03:49:11
tags:
	- GreenDao
	- sqlite
	- 框架
	- android
categories:
	- android
	- 框架
---
# GreenDao
## 一、简介
GreenDao是一个对象映射数据解决方案的快速开发框架，很多sql语句直接变换简单的代码。
## 一、greenDao的配置
### project/build.gradle
```
buildscript {
    repositories {
        jcenter()
    }
    dependencies {
        classpath 'org.greenrobot:greendao-gradle-plugin:3.2.1'
        // NOTE: Do not place your application dependencies here; they belong
        // in the individual module build.gradle files
    }
}
```
### app/build.gradle
```
apply plugin: 'org.greenrobot.greendao'
android {
………………
}
dependencies {

………………
    //数据库操作
    compile 'org.greenrobot:greendao-generator:3.2.0'
    compile 'org.greenrobot:greendao:3.2.0'
}
```
<!--more-->

##二、使用
### 配置注解映射实体类
greendao是插件化开发的，所以配置完注解，直接整个工程rebuild一下，就会自动生成Dao部分代码
```
@Entity(nameInDb = "t_book")
public class Book {
    @Id(autoincrement = true)
    @Property(nameInDb = "id")
    private Long id;
    @Property(nameInDb = "f_book")
    private String book;
    @ToMany(referencedJoinProperty = "bookId")
    private List<Unit> units;
    public Long getId() {
        return id;
    }

    public void setId(Long id) {
        this.id = id;
    }

    public String getBook() {
        return book;
    }

    public void setBook(String book) {
        this.book = book;
    }
    @Generated//一定要标识这个@Generated，不要标识@Keep
    public List<Unit> getUnits() {
    return units;
    }
    @Generated//一定要标识这个@Generated，不要标识@Keep
    public void setUnits(List<Unit> units) {
        this.units = null;
    }
}
```

```
@Id(autoincrement = true)
    @Property(nameInDb = "id")
    private long id;
    @Property(nameInDb = "f_book_id")
    private long bookId;//书籍id
    @ToOne(joinProperty = "bookId")//请注意：joinProperty这是变量名，不是数据库字段
    private Book book;
    @Property(nameInDb = "f_unit")
    private String unit;//单元
    public long getId() {
        return id;
    }

    public void setId(long id) {
        this.id = id;
    }

    public long getBookId() {
        return bookId;
    }

    public void setBookId(long bookId) {
        this.bookId = bookId;
    }
    @Generated(hash = 8805314)//一定要标识这个@Generated，不要标识@Keep
    public Book getBook() {
	    return book;
    }
    @Generated//一定要标识这个@Generated，不要标识@Keep
    public void setBook( Book book) {
	    this.book=book;
    }
```
@Entity：标识这是一个数据库映射类
@Id(autoincrement = true)：标识主键，autoincrement = true设置主键自增
@Property(nameInDb = "id")：标识为数据库字段，nameInDb = "id"表示对应数据库的字段名。如果不使用nameInDb，将默认采用变量名作为数据库的字段名。
@ToMany(referencedJoinProperty = "bookId")：表示一对多的关系，referencedJoinProperty = "bookId"，表示引用关联实体的属性“bookId”，此属性名对应引用实体类的变量名。
@ToOne(joinProperty = "bookId")：表示一对一，或者多对一的关系，joinProperty = "bookId"，表示分享属性“bookId”，此属性名对应本类的变量名。
### 获取DaoSession对数据进行操作

```
DaoMaster.DevOpenHelper mHelper =new DaoMaster.DevOpenHelper(App.getInstance(), DB_NAME, null);//DB_NAME，可以设置为已经存在的外部数据库，如果没有，会自动创建
SQLiteDatabase db = mHelper.getWritableDatabase();
DaoSession mDaoSession = mDaoMaster.newSession();//同步操作时，需要使用DaoSession
AsyncSession mAsyncSession= mDaoSession.startAsyncSession();//异步处理时，需要使用AsyncSession 
```
查询数据

```
//直接查询全部
UnitDao unitDao = GreenDaoHelper.getDaoSession().getUnitDao();
        unitDao.loadAll();
//根据条件查询
UnitDao unitDao = mDaoSession .getUnitDao();
        WhereCondition eq = unitDao.Properties.Id.eq(1);
        WhereCondition eq5 = unitDao.Properties.BookId.eq(1);
        List<Unit> list = unitDao.queryBuilder().where(eq1, eq2, eq3, eq4).build().list();
```

```
//异步数据查询
StudyWordScoreDao studyWordScoreDao = GreenDaoHelper.getDaoSession().getStudyWordScoreDao();
        WhereCondition eq1 = StudyWordScoreDao.Properties.BookId.eq(bookId);
        WhereCondition eq2 = StudyWordScoreDao.Properties.UnitId.eq(unitId);
        WhereCondition eq3 = StudyWordScoreDao.Properties.UnitId.eq(userId);
        WhereCondition eq4 = StudyWordScoreDao.Properties.UnitId.eq(recordId);
        Query<StudyWordScore> build = studyWordScoreDao.queryBuilder().where(eq1, eq2, eq3, eq4).build();
//        WhereCondition and = wordDao.queryBuilder().and(eq, eq1);
        asyncSession.queryList(build);
        asyncSession.setListenerMainThread(asyncOperationListener);
```

## 
##细节问题
### 凡是涉及ID的一定要使用Long类型的包装类
 凡是涉及ID的一定要使用Long类型的包装类，不然，autoincrement = true会失效，我之前就是这样，插入的数据会直接赋为“0”
```
//根据官网的方法，说主键赋值为空，主键就会自增，但是编译器不通过，报错，因为初始为0，不是null，所以我就直接不设置主键值，果然就可以了
new Book(null,"必修一");
```
### 关联实体的类的get/set方法一定要使用@Generate

```
//只有使用了Generated，greenDao的插件才会将getset修改成以下代码，如果使用@keep那就保留getset代码，也就没有关联效果了
@Generated(hash = 835179934)
    public List<Unit> getUnits() {
        if (units == null) {
            final DaoSession daoSession = this.daoSession;
            if (daoSession == null) {
                throw new DaoException("Entity is detached from DAO context");
            }
            UnitDao targetDao = daoSession.getUnitDao();
            List<Unit> unitsNew = targetDao._queryBook_Units(id);
            synchronized (this) {
                if (units == null) {
                    units = unitsNew;
                }
            }
        }
        return units;
    }

    /** Resets a to-many relationship, making the next get call to query for a fresh result. */
    @Generated(hash = 121816020)
    public synchronized void resetUnits() {
        units = null;
    }
```

暂时只总结部分，后续会继续更新此文章
