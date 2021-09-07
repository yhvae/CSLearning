



### war包

war包是一种打包格式。java web工程都是打成war包，进行发布。将war包放在tomcat容器的webapps下，启动服务，即可运行该项目，该war包会自动解压出一个同名的文件夹。

war包中主要是有一个WEB-INF，WEB-INF下面主要有一个lib文件夹，主要存放需要用到的外部依赖的jar包。classes下存放的是带包名结构的java类；classes下还存放有配置文件以及static中的前端资源。

