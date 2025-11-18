ipad1（期中实验）
📝 Modern Notes - 现代记事本
一个功能丰富、设计精美的Android笔记应用，采用现代Android开发架构，提供完整的笔记管理和待办事项功能。
ps：观察到原先代码的标题和内容容易混淆，标题无法显示等严重缺陷，由于希望美化界面，代码结构和基本框架我参照ai给出的建议，做出了巨大改动，和源代码有很大差距。

现在我们先来看看功能实现的截图。
我的应用叫现代记事本，可以看见已经创建了两个笔记，上面有时间戳，代办和类型
<img width="588" height="1302" alt="image" src="https://github.com/user-attachments/assets/2c253e1e-deba-4e65-a483-9f7d305f60fa" />
搜索功能
<img width="594" height="1303" alt="image" src="https://github.com/user-attachments/assets/341ef461-c6d9-46e3-a1e3-759d6b47d515" />
点击右下加号进入新建笔记页面，可以看到代办标记和标题内容输入
<img width="602" height="1311" alt="image" src="https://github.com/user-attachments/assets/53d3fa0e-0ae6-4ca1-844a-a4057a90889f" />
点击工作我们还可以加标签
<img width="589" height="1310" alt="image" src="https://github.com/user-attachments/assets/28aaafce-9a44-41e8-8ebb-e531f5a7b78c" />

✨ 功能特性
📱 核心功能
笔记管理：创建、编辑、删除笔记，支持富文本内容
时间戳记录：自动记录笔记创建/修改时间，格式化显示
智能搜索：实时搜索标题和内容，无需点击搜索按钮
待办事项：将笔记转换为待办事项，支持完成状态管理
标签分类：通过category字段实现笔记分类管理
个性化外观：支持为每个笔记设置不同颜色
🎨 界面设计
Material Design：采用Google Material Design设计语言
卡片式布局：使用CardView展示笔记，视觉效果优雅
响应式交互：支持点击编辑、长按删除、勾选完成等操作
实时更新：基于LiveData的数据观察，界面自动刷新
🏗️ 技术架构
架构模式
采用 MVVM (Model-View-ViewModel) 架构模式：

Model层：Note实体 + NoteDao + NoteRepository
View层：MainActivity + NoteActivity + 自定义Adapter
ViewModel层：NoteViewModel处理业务逻辑
核心技术栈
开发语言：Java
UI框架：Android原生View + Material Design Components
数据库：Room持久化库 (SQLite ORM)
架构组件：Android Jetpack (ViewModel, LiveData)
列表组件：RecyclerView + 自定义Adapter
数据模型设计


@Entity(tableName = "notes")
public class Note {
    @PrimaryKey(autoGenerate = true)
    private int id;
    private String title;          // 标题
    private String content;        // 内容
    private long timestamp;        // 时间戳
    private String category;       // 分类标签
    private int color;            // 背景颜色
    private boolean isTodo;       // 是否为待办事项
    private boolean isCompleted;   // 是否已完成
}
📦 依赖库
核心依赖


// Room数据库
implementation("androidx.room:room-runtime:2.5.0")
annotationProcessor("androidx.room:room-compiler:2.5.0")

// ViewModel和LiveData
implementation("androidx.lifecycle:lifecycle-viewmodel:2.7.0")
implementation("androidx.lifecycle:lifecycle-livedata:2.7.0")

// Material Design
implementation("com.google.android.material:material:1.10.0")

// RecyclerView
implementation("androidx.recyclerview:recyclerview:1.3.1")

// CardView
implementation("androidx.cardview:cardview:1.0.0")
🔧 项目结构

app/
├── java/com/example/ipad/
│   ├── MainActivity.java           # 主界面
│   ├── NoteActivity.java          # 笔记编辑界面
│   ├── NoteAdapter.java           # RecyclerView适配器
│   ├── Note.java                  # 数据实体
│   ├── NoteDao.java               # 数据访问对象
│   ├── NoteViewModel.java         # 视图模型
│   ├── NoteRepository.java        # 数据仓库
│   └── AppDatabase.java           # 数据库配置
├── res/
│   ├── layout/                    # 布局文件
│   ├── values/                    # 资源文件
│   └── drawable/                  # 图片资源
└── AndroidManifest.xml            # 应用配置
🚀 核心功能实现
数据库操作


@Dao
public interface NoteDao {
    @Insert
    void insert(Note note);
    
    @Update
    void update(Note note);
    
    @Delete
    void delete(Note note);
    
    @Query("SELECT * FROM notes ORDER BY timestamp DESC")
    LiveData<List<Note>> getAllNotes();
    
    @Query("SELECT * FROM notes WHERE title LIKE :query OR content LIKE :query")
    LiveData<List<Note>> searchNotes(String query);
    
    @Query("SELECT * FROM notes WHERE category = :category")
    LiveData<List<Note>> getNotesByCategory(String category);
    
    @Query("SELECT * FROM notes WHERE isTodo = 1")
    LiveData<List<Note>> getTodoNotes();
}
响应式数据更新


// MainActivity中的数据观察
noteViewModel.getAllNotes().observe(this, notes -> {
    adapter.setNotes(notes);
});

// 实时搜索
searchEditText.addTextChangedListener(new TextWatcher() {
    @Override
    public void onTextChanged(CharSequence s, int start, int before, int count) {
        noteViewModel.searchNotes(s.toString()).observe(MainActivity.this, notes -> {
            adapter.setNotes(notes);
        });
    }
});
🎯 使用说明
基本操作
添加笔记：点击右下角浮动按钮创建新笔记
编辑笔记：点击任意笔记项进入编辑界面
删除笔记：长按笔记项即可删除
搜索笔记：在顶部搜索框输入关键词实时搜索
待办管理：勾选笔记左侧checkbox标记完成状态
高级功能
设置颜色：在编辑界面为笔记选择个性化颜色
添加标签：通过category字段为笔记分类
时间查看：每个笔记显示创建/修改时间
📱 系统要求
最低Android版本：Android 7.0 (API 24)
目标Android版本：Android 14 (API 36)
开发工具：Android Studio Hedgehog | 2023.1.1 或更高版本
🧪 测试
项目包含完整的测试套件：

单元测试：使用JUnit测试业务逻辑
UI测试：使用Espresso测试用户交互
📄 许可证
本项目采用 MIT 许可证 - 查看 LICENSE 文件了解详情

🤝 贡献
欢迎提交Issue和Pull Request来改进这个项目！
