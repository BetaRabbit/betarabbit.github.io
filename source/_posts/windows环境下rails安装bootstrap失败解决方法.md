title: "Windows环境下Rails安装Bootstrap失败解决方法"
date: 2012-12-16 00:12:40
tags: [Rails]
---

Windows环境下，Rails安装Bootstrap总会失败，提示therubyracer无法安装。

这是因为Bootstrap使用的less文件依赖therubyracer实时执行js将less转换成css，而therubyracer这个gem并没有对应的Windows版本。

[Hiran Peiris](https://github.com/hiranpeiris)在github上提供了提供了[解决方案](https://github.com/hiranpeiris/therubyracer_for_windows)，他编译了所有的dll和gem。这下，我们终于可以在Windows下用Bootstrap啦。