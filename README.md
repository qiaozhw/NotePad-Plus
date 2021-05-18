# NotePad添加时间戳和搜索功能

------



## Sample导入

NotePad是早期Android版本的Sample，笔者使用了GitHub上面的代码，源码：[GitHub](https://github.com/llfjfz/NotePad)

------

### 项目简析

##### 主要的类

1. NotesList类 —— 应用程序的入口，笔记本的首页面会显示笔记的列表
2. NoteEditor类 —— 编辑笔记内容的Activity
3. TitleEditor类 —— 编辑笔记标题的Activity
4. NotePadProvider类 —— 这是笔记本应用的ContentProvider，也是整个应用的关键所在
5. NotesLiveFolder ContentProvider的LiveFolder（实时文件夹），这个功能在Android API 14后被废弃，不再支持。因此代码中所有涉及LiveFolder的内容将不再阐述。

##### 主要的布局文件

1. note_editor.xml —— 笔记主页面布局
2. note_search.xml —— 笔记内容查询布局
3. notelist_item.xml —— 笔记主页面每个列表项布局
4. title_editor.xml —— 修改笔记主题布局

##### 主要的菜单文件

1. editor_options_menu.xml —— 编辑笔记内容的菜单布局
2. list_context_menu.xml —— 笔记内容编辑上下文菜单布局
3. list_options_menu.xml —— 笔记主页面可选菜单布局

##### 数据装配

NoteList使用SimpleCursorAdapter来装配数据

1. 查询数据库内容，这里使用ContentProvider默认的URI：

   ```java
   Cursor cursor = managedQuery(
            getIntent().getData(),           
            PROJECTION,                      
            null,                             
            null,                             
            NotePad.Notes.DEFAULT_SORT_ORDER);
   ```

   

2. 通过SimpleCursorAdapter装配：

   ```java
   // Creates the backing adapter for the ListView.
   SimpleCursorAdapter adapter
       = new SimpleCursorAdapter(
       this,                             // The Context for the ListView
       R.layout.noteslist_item,          // Points to the XML for a list item
       cursor,                           // The cursor to get items from
       dataColumns,
       viewIDs
       );
   ```

##### 页面跳转

不管是可选菜单、上下文菜单中的操作，还是单击列表中的笔记条目，其相应的页面跳转都是通过Intent的Action+URI进行。比如startActivity(new Intent(Intent.ACTION_EDIT, noteUri))，这里就会寻找能够进行EDIT的Activity跳转，同时传递出URI数据。所有的Intent过滤规则都在AndroidManifest.xml中定义。

##### 详细解析

移步CSDN博客——[Android Sample--NotePad解析_llfjfz的专栏-CSDN博客](https://blog.csdn.net/llfjfz/article/details/67638499)

------

### 项目结构

![项目结构](https://github.com/ZW-Q/NotePad-Plus/blob/master/images/image-20210517143734535.png)

------

### 项目导入

1. File—Open—选择项目文件夹

2. 导入项目报错

3. ![sync报错](https://github.com/ZW-Q/NotePad-Plus/blob/master/images/image-20210517144720221.png)

4. 此时不能按照Android Studio的提示`Install missing platform(s) and sync project`,该操作仍然会报错

   由于NotePad使用的是Android-23版本，版本太旧，而笔者使用的Android Studio 3.5.2 的AndroidSDKVersion版本为30，因此我们要对NotePad进行gradle项目更新

5. 首先新建一个项目`HelloWorld`，该项目的AndroidSDKVersion是符合AS的版本

   展开Gradle Scripts文件夹，`build.gradle(Projtect:HelloWorld)`、`build.gradle(Module:app)`、`gradle-wrapper.properties(Gradle Version)`三个gradle文件包含了当前AndroidSDKVersion![新项目Gradle](https://github.com/ZW-Q/NotePad-Plus/blob/master/images/image-20210517150737354.png)

6. 打开NotePad项目，选择`Project Files`模式

   app下的`build.gradle`对照HelloWorld项目中的`build.gradle(Module:app)`进行修改

   wrapper下的`gradle-wrapper.properties`对照HelloWorld项目中的`gradle-wrapper.properties(Gradle Version)`进行修改

   gradle下的`build.gradle`对照HelloWorld项目中的`build.gradle(Projtect:HelloWorld)`进行修改![NotePad结构](https://github.com/ZW-Q/NotePad-Plus/blob/master/images/image-20210517151528598.png)

7. 将NotePad `build.gradle`四个地方更改为和HelloWorld相同的版本![app-gradle](https://github.com/ZW-Q/NotePad-Plus/blob/master/images/image-20210517152401181.png)

   更改前：

   ![app-gradle](https://github.com/ZW-Q/NotePad-Plus/blob/master/images/image-20210517152545215.png)

   更改后：

   ```java
   android {
       compileSdkVersion 30
       buildToolsVersion "30.0.3"
   
       defaultConfig {
           applicationId "com.example.android.notepad"
           minSdkVersion 15
           targetSdkVersion 30
           versionCode 1
           versionName "1.0"
   
           testApplicationId "com.example.android.notepad.tests"
           testInstrumentationRunner "android.test.InstrumentationTestRunner"
       }
   ```
   
8. NotePad下的`build.gradle`

   更改前：

   ![image-20210517153602411](https://github.com/ZW-Q/NotePad-Plus/blob/master/images/image-20210517153602411.png)

   更改后：

   ```java
   buildscript {
       repositories {
           google()
           jcenter()
       }
       dependencies {
           classpath 'com.android.tools.build:gradle:3.5.2'
       }
   }
   ```

9. NotePad下的`gradle-wrapper.properties`

   更改前：

   ![image-20210517153742824](https://github.com/ZW-Q/NotePad-Plus/blob/master/images/image-20210517153742824.png)

   更改后：

   ```Java
   #Mon Dec 28 10:00:20 PST 2015
   distributionBase=GRADLE_USER_HOME
   distributionPath=wrapper/dists
   zipStoreBase=GRADLE_USER_HOME
   zipStorePath=wrapper/dists
   distributionUrl=https\://services.gradle.org/distributions/gradle-5.4.1-all.zip
   ```

10. 修改完成后保存重新启动Android Studio，发现可以同步成功！

    ![image-20210517153937446](https://github.com/ZW-Q/NotePad-Plus/blob/master/images/image-20210517153937446.png)

11. 运行Android虚拟机，效果展示

    ![image-20210517154129814](https://github.com/ZW-Q/NotePad-Plus/blob/master/images/image-20210517154129814.png)

    ------

## 添加时间戳

1. 时间戳的位置应该在NotePad主页面的每个笔记列表项中添加，因此在 res—layout—`notelist_item.xml`布局文件中添加`TextView`

2. `notelist_item.xml`源代码：

   ```java
   <TextView xmlns:android="http://schemas.android.com/apk/res/android"
       android:id="@android:id/text1"
       android:layout_width="match_parent"
       android:layout_height="?android:attr/listPreferredItemHeight"
       android:textAppearance="?android:attr/textAppearanceLarge"
       android:gravity="center_vertical"
       android:paddingLeft="5dip"
       android:singleLine="true"
   />
   ```

   修改后代码：

   ```java
   <LinearLayout
       xmlns:android="http://schemas.android.com/apk/res/android"
       android:orientation="vertical"
       android:layout_width="wrap_content"
       android:layout_height="wrap_content">
       >
       <TextView xmlns:android="http://schemas.android.com/apk/res/android"
           android:id="@android:id/text1"
           android:layout_width="match_parent"
           android:layout_height="?android:attr/listPreferredItemHeight"
           android:textAppearance="?android:attr/textAppearanceLarge"
           android:gravity="center_vertical"
           android:paddingLeft="5dip"
           android:singleLine="true"
           />
   
       <TextView
           android:id="@android:id/text2"
           android:layout_width="match_parent"
           android:layout_height="wrap_content"
           android:textAppearance="?android:attr/textAppearanceLarge"
           android:gravity="center_vertical"
           android:paddingLeft="5dp"
           android:singleLine="true" />
   </LinearLayout>
   ```

   PS：添加新的`TextView`时应添加垂直约束——`android:gravity="center_vertical"`

3. 在NoteList类的`PROJECTION`中添加`COLUMN_NAME_MODIFICATION_DATE`字段(该字段在NotePad中有说明)

   更改前：

   ```java
   /**
    * The columns needed by the cursor adapter
    */
   private static final String[] PROJECTION = new String[] {
           NotePad.Notes._ID, // 0
           NotePad.Notes.COLUMN_NAME_TITLE, // 1
   };
   ```

   更改后：

   ```java
   /**
    * The columns needed by the cursor adapter
    */
   private static final String[] PROJECTION = new String[] {
           NotePad.Notes._ID, // 0
           NotePad.Notes.COLUMN_NAME_TITLE, // 1
           NotePad.Notes.COLUMN_NAME_MODIFICATION_DATE,//在这里加入了修改时间的显示
   };
   ```

4. 修改适配器内容，在NoteList类增加`dataColumns`中装配到`ListView`的内容，所以要同时增加一个ID标识来存放该时间参数

   更改前：

   ```java
   // The names of the cursor columns to display in the view, initialized to the title column
   String[] dataColumns = { NotePad.Notes.COLUMN_NAME_TITLE } ;
   
   // The view IDs that will display the cursor columns, initialized to the TextView in
   // noteslist_item.xml
   int[] viewIDs = { android.R.id.text1 };
   ```

   更改后：

   ```java
   // The names of the cursor columns to display in the view, initialized to the title column
   String[] dataColumns = { NotePad.Notes.COLUMN_NAME_TITLE ,
           NotePad.Notes.COLUMN_NAME_MODIFICATION_DATE  } ;
   
   // The view IDs that will display the cursor columns, initialized to the TextView in
   // noteslist_item.xml
   int[] viewIDs = { android.R.id.text1 ,android.R.id.text2};
   ```

5. 在NoteEditor类的updateNote方法中获取当前系统的时间，并对时间进行格式化

   更改前：

   ```java
   // Sets up a map to contain values to be updated in the provider.
   ContentValues values = new ContentValues();
   values.put(NotePad.Notes.COLUMN_NAME_MODIFICATION_DATE, System.currentTimeMillis());
   ```

   更改后：

   ```java
   // Sets up a map to contain values to be updated in the provider.
   ContentValues values = new ContentValues();
   Long now = Long.valueOf(System.currentTimeMillis());
   SimpleDateFormat sf = new SimpleDateFormat("yy/MM/dd HH:mm");
   Date d = new Date(now);
   String format = sf.format(d);
   values.put(NotePad.Notes.COLUMN_NAME_MODIFICATION_DATE, format);
   ```

6. 时间戳效果展示

   ![时间戳](https://github.com/ZW-Q/NotePad-Plus/blob/master/images/image-20210517164028794.png)

------

## 添加搜索框

1. 搜索组件在主页面的菜单选项中，因此在res—menu—`list_options_menu.xml`布局文件中添加搜索功能，新增`menu_search`

   ```java
   <item
       android:id="@+id/menu_search"
       android:icon="@android:drawable/ic_menu_search"
       android:title="@string/menu_search"
       android:showAsAction="always" />
   ```

2. 在res—layout中新建一个查找笔记内容的布局文件`note_search.xml`

   ```java
   <?xml version="1.0" encoding="utf-8"?>
   <LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
       android:layout_width="match_parent"
       android:layout_height="match_parent"
       android:orientation="vertical">
       <SearchView
           android:id="@+id/search_view"
           android:layout_width="match_parent"
           android:layout_height="wrap_content"
           android:iconifiedByDefault="false"
           />
       <ListView
           android:id="@+id/list_view"
           android:layout_width="match_parent"
           android:layout_height="wrap_content"
           />
   </LinearLayout>
   ```

3. 在NoteList类中的`onOptionsItemSelected`方法中新增search查询的处理(跳转)

   ```java
   case R.id.menu_search:
       //查找功能
       //startActivity(new Intent(Intent.ACTION_SEARCH, getIntent().getData()));
       Intent intent = new Intent(this, NoteSearch.class);
       this.startActivity(intent);
       return true;
   ```

4. 新建一个NoteSearch类用于search功能的功能实现

   ```java
   package com.example.android.notepad;
   import android.app.Activity;
   import android.content.Intent;
   import android.database.Cursor;
   import android.database.sqlite.SQLiteDatabase;
   import android.os.Bundle;
   import android.widget.ListView;
   import android.widget.SearchView;
   import android.widget.SimpleCursorAdapter;
   import android.widget.Toast;
   
   public class NoteSearch extends Activity implements SearchView.OnQueryTextListener
   {
       ListView listView;
       SQLiteDatabase sqLiteDatabase;
       /**
        * The columns needed by the cursor adapter
        */
       private static final String[] PROJECTION = new String[]{
               NotePad.Notes._ID, // 0
               NotePad.Notes.COLUMN_NAME_TITLE, // 1
               NotePad.Notes.COLUMN_NAME_MODIFICATION_DATE//时间
       };
   
       public boolean onQueryTextSubmit(String query) {
           Toast.makeText(this, "您选择的是："+query, Toast.LENGTH_SHORT).show();
           return false;
       }
   
       @Override
       protected void onCreate(Bundle savedInstanceState) {
           super.onCreate(savedInstanceState);
           setContentView(R.layout.note_search);
           SearchView searchView = findViewById(R.id.search_view);
           Intent intent = getIntent();
           if (intent.getData() == null) {
               intent.setData(NotePad.Notes.CONTENT_URI);
           }
           listView = findViewById(R.id.list_view);
           sqLiteDatabase = new NotePadProvider.DatabaseHelper(this).getReadableDatabase();
           //设置该SearchView显示搜索按钮
           searchView.setSubmitButtonEnabled(true);
   
           //设置该SearchView内默认显示的提示文本
           searchView.setQueryHint("查找");
           searchView.setOnQueryTextListener(this);
   
       }
       public boolean onQueryTextChange(String string) {
           String selection1 = NotePad.Notes.COLUMN_NAME_TITLE+" like ? or "+NotePad.Notes.COLUMN_NAME_NOTE+" like ?";
           String[] selection2 = {"%"+string+"%","%"+string+"%"};
           Cursor cursor = sqLiteDatabase.query(
                   NotePad.Notes.TABLE_NAME,
                   PROJECTION, // The columns to return from the query
                   selection1, // The columns for the where clause
                   selection2, // The values for the where clause
                   null,          // don't group the rows
                   null,          // don't filter by row groups
                   NotePad.Notes.DEFAULT_SORT_ORDER // The sort order
           );
           // The names of the cursor columns to display in the view, initialized to the title column
           String[] dataColumns = {
                   NotePad.Notes.COLUMN_NAME_TITLE,
                   NotePad.Notes.COLUMN_NAME_MODIFICATION_DATE
           } ;
           // The view IDs that will display the cursor columns, initialized to the TextView in
           // noteslist_item.xml
           int[] viewIDs = {
                   android.R.id.text1,
                   android.R.id.text2
           };
           // Creates the backing adapter for the ListView.
           SimpleCursorAdapter adapter
                   = new SimpleCursorAdapter(
                   this,                             // The Context for the ListView
                   R.layout.noteslist_item,         // Points to the XML for a list item
                   cursor,                           // The cursor to get items from
                   dataColumns,
                   viewIDs
           );
           // Sets the ListView's adapter to be the cursor adapter that was just created.
           listView.setAdapter(adapter);
           return true;
       }
   }
   ```

   PS：切记在res—values—`strings.xml`中添加`menu_search`字段

5. 搜索框效果展示

   

   ![搜索框](https://github.com/ZW-Q/NotePad-Plus/blob/master/images/image-20210517165119923.png)![搜索](https://github.com/ZW-Q/NotePad-Plus/blob/master/images/image-20210517165200326.png)

   

