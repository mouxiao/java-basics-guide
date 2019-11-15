### javap

javap的用法格式：<br/>
javap <options> <classes><br/>
其中classes就是你要反编译的class文件。<br/>
在命令行中直接输入javap或javap -help可以看到javap的options有如下选项：
>-hep  --hep  -?        输出此用法消息<br/>
-version                 版本信息，其实是当前javap所在jdk的版本信息，不是cass在哪个jdk下生成的。<br/>
-v  -verbose             输出附加信息（包括行号、本地变量表，反汇编等详细信息）<br/>
-                         输出行号和本地变量表<br/>
-pubic                    仅显示公共类和成员<br/>
-protected               显示受保护的/公共类和成员<br/>
-package                 显示程序包/受保护的/公共类 和成员 (默认)<br/>
-p  -private             显示所有类和成员<br/>
-c                       对代码进行反汇编<br/>
-s                       输出内部类型签名<br/>
-sysinfo                 显示正在处理的类的系统信息 (路径, 大小, 日期, MD5 散列)<br/>
-constants               显示静态最终常量<br/>
-casspath &t;path>        指定查找用户类文件的位置<br/>
-bootcasspath &t;path>    覆盖引导类文件的位置<br/>
-l 会输出行号和本地变量表信息<br/>