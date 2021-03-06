# ContentProvider 详解

---

> ContentProvider 内容提供者为不同的应用之间数据共享，提供统一的接口，我们知道安卓系统中应用内部的数据是对外隔离的，要想让其它应用能使用自己的数据（例如通讯录）这个时候就用到了 ContentProvider。

## 目录

-[CRUD](#CRUD)

-[ContentObserver](#ContentObserver)

## CRUD

ContentProvider 通过 uri 来标识其它应用要访问的数据，通过 ContentResolver 的 query,update,insert,delete.（CRUD）方法实现对共享数据的操作。还可以通过注册 ContentObserver 来监听数据是否发生了变化来对应的刷新页面，接下来我们就来介绍一下

**onCreate**
在创建 ContentProvider 时使用

**query**
用于查询指定 uri 的数据返回一个 Cursor

**insert**
用于向指定 uri 的 ContentProvider 中添加数据

**delete**
用于删除指定 uri 的数据

**update**
用户更新指定 uri 的数据

**getType**
用于返回指定的 Uri 中的数据 MIME 类型

ContentResolver 通过 uri 来定位自己要访问的数据，uri 的格式：[scheme:][//host:port][path][?query]

CRUD 实例代码如下：

```

ContentResolver resolver = getContentResolver();
Uri uri = Uri.parse("content://com.madreain.provider.myprovider/tablename");

//添加一条记录
ContentValues values = new ContentValues();
values.put("name", "madreain");
values.put("age", 26);
resolver.insert(uri, values);

//获取tablename表中所有记录
Cursor cursor = resolver.query(uri, null, null, null, "tablename data");
while(cursor.moveToNext()){
   Log.i("ContentTest", "tablename_id="+ cursor.getInt(0)+ ", name="+ cursor.getString(1));
}

//把id为1的记录的name字段值更改新为zhang1
ContentValues updateValues = new ContentValues();
updateValues.put("name", "zhang1");
Uri updateIdUri = ContentUris.withAppendedId(uri, 2);
resolver.update(updateIdUri, updateValues, null, null);

//删除id为2的记录，即字段age
Uri deleteIdUri = ContentUris.withAppendedId(uri, 2);
resolver.delete(deleteIdUri, null, null);

```

com.madreain.provider.myprovider 需要在 AndroidManifest.xml 中进行注册

```
<provider android:name="MyProvider" 
          android:authorities="com.madreain.provider.myprovider" 
          android:enabled="true"
          android:exported="true"/>
```

## ContentObserver

介绍一下 ContentProvider、ContentResolver、ContentObserver 之间的关系：
ContentProvider——内容提供者， 在 android 中的作用是对外共享数据，也就是说你可以通过 ContentProvider 把应用中的数据共享给其他应用访问，其他应用可以通过 ContentProvider 对你应用中的数据进行添删改查。
ContentResolver——内容解析者， 其作用是按照一定规则访问内容提供者的数据（其实就是调用内容提供者自定义的接口来操作它的数据）。
ContentObserver——内容观察者，目的是观察(捕捉)特定 Uri 引起的数据库的变化，继而做一些相应的处理，它类似于数据库技术中的触发器(Trigger)，当 ContentObserver 所观察的 Uri 发生变化时，便会触发它。

内容观察者，观察特定 Uri 引起的数据库的变化，继而做一些相应的处理，当 ContentObserver 所观察的 Uri 发生变化时，便会触发它回调 onChange 方法

继承 ContentObserver,实现 onChange 方法

```
public class MObserver extends ContentObserver{
        public MObserver(Handler handler){
            super(handler);
        }


        @Override
        public void onChange(boolean selfChange) {
            super.onChange(selfChange);
            queryDb();
        }
    }
```

在 AActivity 中注册和注销

```
//注册
MObserver mContentObserver = new MObserver(new Handler(),this);
getContentResolver().registerContentObserver(Madreain.CONTENT_URI_DELETE,true, mContentObserver);

//注销
getContentResolver().unregisterContentObserver(mContentObserver);

```
