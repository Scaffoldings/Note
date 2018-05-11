## 资料内容参考 
  - react文档：`http://reactjs.cn/react/docs/getting-started-zh-CN.html `;

  - react-router 文档地址 :`https://reacttraining.com/react-router/web/guides/quick-start `;

  - react-router 中文版参考：`http://www.uprogrammer.cn/react-router-cn/index.html`;

  - redux文档参考：`http://redux.js.org/`;

  - redux中文文档：`http://cn.redux.js.org/`; 
  
  - 微信小程资料：`http://www.php.cn/xiaochengxu-388965.html`
  
  - 学习手册：`http://www.php.cn/xiazai/shouce`
  
  - 日历 Calendar - 蚂蚁设计平台：`http://design.alipay.com/develop/web/components/calendar/`
  
  - ES2017 新特性：Async Functions (异步函数)：`http://www.css88.com/archives/7731`
  
  -  TypeScript 中文文档(v2.4):`http://www.css88.com/doc/typescript/`


## yeoman生成自己项目文件脚手架(react)
  -  `http://zhangzhaoaaa.iteye.com/blog/2231330`;
  ```
  准备做React+Backbone的集成开发，同事之前已经做了自定义Backbone的脚手架，我今天来做React的。

准备工作，安装Node,yeoman就不多说了，什么？不会？你做什么自定义脚手架啊，先自行google去吧

1.安装目录 我的目录是：home/mike/mywork/study/gitcode/

2.安装generator 安装:
在命令行输入：npm install -g generator-generator
说明： yo generator 生成器一个新的生成器的向导
yo generator:subgenerator Name 一个以Name为名字的子生成器

步骤一：
在命令行输入yo generator
按照向导提示输入，我的generator的名字叫reacttiny 第一步写github名字（不填写也可以），第二步写上reacttiny即可，然后其他默认 之后就等待生成对应的generator-reacttiny目录 目录内容如下：

.
├── generators
│   └── app
│       ├── index.js
│       └── templates
│           ├── _bower.json
│           ├── _package.json
│           ├── editorconfig
│           └── jshintrc
├── .editorconfig
├── .gitattributes
├── .gitignore
├── .jshintrc
├── .travis.yml
├── .yo-rc.json
├── package.json
├── README.md
└── test
    └── test-app.js
步骤二： 这时候，cd 进入generator-reacttiny目录， 我们在命令行输入:npm link，就可以在命令行使用这个生成器了

步骤三： 此时，命令行输入：yo reacttiny 尝试一把看看，就可以看到yeoman大爷了。。。

我们当然可以直接在app的index里开发我们的生成器了， 使用的时候直接使用yo reacttiny即可，但是我要做好几个自定义生成器那怎么办呢？

步骤四： 命令行目录还在generator-reacttiny下， 在命令行输入 yo generator:subgenerator react
注意， yo generator:subgenerator是命令，react就是我们子生成器的名字。之后会在generators目录下自动生成相应的模板文件，和app目录一模一样，此时文件夹名称叫做react

步骤五： 此时就可以在react目录下开发了，其实脚手架目的是根据用户在控制台的输入编写模板文件，最后生成模板文件。

3.用到的包有：handlerbars,open,react,rimraf,chalk,mocha,yosay,yeoman-generator

4.generator方法的执行顺序

对于那些会自动执行的函数，他们是有一个优先顺序的，下面这些函数是按顺序的一个一个执行的。

initializing - 你的初始化函数，就是构造函数，主要就是检查一下参数什么的
prompting - 给用户展示你的菜单，选点东西什么的
configuring - 保存配置信息，创建类似.editorconfig的文件
default - 就是默认，只要不在这个列表里的函数都在这个位置执行
writing - 创建模板文件
conflicts - 处理异常和冲突
install - 装npm和bower依赖什么的
end - 打个命令行祝贺使用者成功了
5.上传到npm 如果想将此包上传到npm上，则需要在命令行中的generator-reacttiny目录下输入
npm adduser 无注册用户，按提示步骤先增加个用户
npm login 有注册，用户先登录
npm publish --access=publish 发布即可

PS:在grunt中加入自动编译Jsx文件插件 前提使用yeoman构建了项目

1.在Gruntfile.js文件目录下，安装grunt-react和grunt-browserify
命令行输入：npm install grunt-react --save
npm install grunt-browserify --save

2.在Gruntfile.js的watch中增加以下代码，自动监听编译jsx代码为js代码 指定react的jsx目录

watch: {
      react:{
        files: ['<%= config.app %>/react/{,*/}*.jsx'],
        tasks: ['react:dynamic_mappings', 'react:single_js_files']
      },
      browserify:{
        files: ['<%= config.app %>/react/{,*/}*.jsx'],
        tasks: ['browserify:options']
      }
}
3.在Gruntfile.js的initConfig中增加react和browserify 指定react的jsx目录和js的输出目录

 grunt.initConfig({
react: {
      single_js_files: {
        files: {
          '<%=config.app%>/react/output/hello.js': '<%=config.app%>/react/hello.jsx'
        }
      },
      combined_file_output: {
        files: {
          'path/to/output/dir/combined.js': [
            'path/to/jsx/templates/dir/input1.jsx',
            'path/to/jsx/templates/dir/input2.jsx'
          ]
        }
      },
      dynamic_mappings: {
        files: [
          {
            expand: true,
            cwd: '<%=config.app%>/react',
            src: ['**/*.jsx'],
            dest: '<%=config.app%>/react/output',
            ext: '.js'
          }
        ]
      }
    },
    browserify: {
      options: {
        transform: [ require('grunt-react').browserify ]
      },
      app: {
        src: '<%= config.app %>/react/hello.jsx',
        dest: '<%= config.app %>/react/output/dist/hello.js'
      }
    },
});
4.在Gruntfile.js中加入以下两行，加载两个任务

grunt.loadNpmTasks('grunt-react');
grunt.loadNpmTasks('grunt-browserify');
5.在grunt:serve这里加上 'react'和'browserify'

grunt.registerTask('serve', 'start the server and preview your app', function (target) { if (target === 'dist') { return grunt.task.run(['build', 'browserSync:dist']);
    }

    grunt.task.run([ 'clean:server', 'wiredep', 'concurrent:server', 'postcss', 'browserSync:livereload', 'watch', 'react', 'browserify'  ]);
  });
6.在默认任务里加上，可以使用grunt --verbose --force命令查看文件生成细节

grunt.registerTask('default', [
    'newer:eslint',
    'test',
    'build',
    'react',
    'browserify'
  ]);
  
  
  ```
  
