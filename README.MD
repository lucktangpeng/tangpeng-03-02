一、简答题

1、请简述 Vue 首次渲染的过程。

- 首先定义了vue的构造函数，在构造函数中调用了_init的方法，后再initMixin函数中定义了_init方法
- 然后调用了initGlobalAPI方法初始化定义了vue的一系列静态成员
- web/runtime.js  这文件中初始化了一些跟平台相关的内容，一些指令和组件，最后给vue挂载了__patch__和$mount方法
- 开始vue的init方法执行，进行一些options的合并之后，就进入$mount函数的执行，如果是完整版的vue
- 会进入到 entry-runtime-with-compiler 方法重写了$mount方法，并将运行版的$mount保存下来
- 在重写的$mount的方法中，对template模板转换成render函数并传入到el.render中
- 最后调用运行版本的$mount中调用mountcomponent方法，首先进行判断是否含有render
- 然后将定义了updateComponent方法，在updateComponent方法中调用的了_update方法并传入了_render函数
- 然后创建了Watcher对象，并传入了updateComponent方法，然后调用get()方法，执行updateComponent方法
- 在updateComponent中的_update方法执行结束后，首次渲染就结束了

2、请简述 Vue 响应式原理。

- 为属性对象添加getter和setter方法并在getter方法中添加当前属性的Watcher观察者对象到dep的subs数组中，在setter方法中完成新旧赋值操作后调用dep的notify方法执行subs数组中的Watcher对象的update方法进行页面的刷新

3、请简述虚拟 DOM 中 Key 的作用和好处。

- Key的作用是标识当前dom的一个身份的作用，好处是在对比的时候不用进行频繁的做dom操作，更好的提升dom渲染的性能

4、请简述 Vue 中模板编译的过程。

- 模板编译过程首先是接受用户传入的template模板和options属性对象，
- 然后通过createCompiler传入baseoptions对象给compile，
- 又通过createCompilerCreator传入核心函数baseCompile给compile, 
- 在最后compileToFunctions将接收到的用户options函数和template模板也传入给compile
- 最后compile在compileToFunctions中开始正式执行
- 首先将用户options和baseoptions进行合并
- 将合并后的对象，连同接收到的template一同传入给baseCompile函数进行处理
- baseCompile函数中首先对template用parse方法转换成ast抽象语法树
- 后通过optimize函数对ast语法树中的内容进行标识静态节点和静态根节点来优化模板编译的性能
- 最后通过generate把抽象语法树生成为字符串形式的render js代码，到此baseCompile执行完毕
- compileToFunctions中把生成的render js代码通过ceateFunction方式转换成render函数
- 后通过templte为键存储一份render函数缓存，最后返回render函数
- 最后加载到Vue的options.render中