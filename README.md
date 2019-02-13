# jar.sh
linux 下 jar包启动脚本

# 1、运行脚本：sh processing.sh start    
如果运行shell脚本报错：    
syntax error near unexpected token \`{    
`usage() {    

可能是因为windows下编辑的文件，在linux下格式不一致的问题，
# 2、可以运行以下命令查看文件：    
cat -v processing.sh    
如果出现以下情况，说明文件格式有问题：    
.....    
is_exist(){^M    
  pid=`ps -ef|grep $APP_NAME|grep -v grep|awk '{print $2}'`^M    
  #M-eM-&M-^BM-fM-^^M-^\M-dM-8M-^MM-eM--M-^XM-eM-^\M-(M-hM-?M-^TM-eM-^[M-^^1M-oM-<M-^LM-eM--M-^XM-eM-^\M-(M-hM-?M-^TM-eM-^[M-^^0^M    
  if [ -z "${pid}" ]; then^M    
   return 1^M    
  else^M    
    return 0^M    
  fi^M    
}^M    
^M    
....      
# 3、类似上面出现很多^M 代表有问题，需要进行将文件转换成linux下的文件格式，可以用以下命令：    
dos2unix processing.sh    
如果报错：bash:dos2unix:command not found    
需要安装dos2unix    
# 4、yum -y install dos2unix    
运行完即代表安装成功，部分如下所示：
sinjdoc-0.5-9.1.el6.x86_64 has missing requires of java-gcj-compat >= ('0', '1.0.70', None)    
tomcat6-6.0.24-49.el6.noarch has missing requires of java    
wsdl4j-1.5.2-7.8.el6.noarch has missing requires of java    
wsdl4j-1.5.2-7.8.el6.noarch has missing requires of jaxp_parser_impl    
xml-commons-apis-1.3.04-3.6.el6.x86_64 has missing requires of java-gcj-compat    
xml-commons-apis-1.3.04-3.6.el6.x86_64 has missing requires of java-gcj-compat    
xml-commons-resolver-1.1-4.18.el6.x86_64 has missing requires of java-gcj-compat    
xml-commons-resolver-1.1-4.18.el6.x86_64 has missing requires of java-gcj-compat    
xml-commons-resolver-1.1-4.18.el6.x86_64 has missing requires of jaxp_parser_impl    
  Installing : dos2unix-3.1-37.el6.x86_64                                   1/1     
  Verifying  : dos2unix-3.1-37.el6.x86_64                                   1/1     
    
Installed:    
  dos2unix.x86_64 0:3.1-37.el6                                                      
    
Complete!    

# 5、继续运行命令：    
dos2unix processing.sh    
显示如下表示成功：    
dos2unix: converting file processing.sh to UNIX format ...    

# 6、用cat 查看文件格式，命令如下：    
cat -A processing.sh    
文件中结尾没有^M 表示转换成功，    
# 7、运行sh processing.sh 成功运行，显示：    
Usage: sh ִprocessing.sh [start|stop|restart|status]


备注：经测试，发现linux上用户权限不够的话，可能start还会报错，生成log.out文件没有权限。所以执行该脚本需要对当前目录有权限的linux用户
可以用命令    su username(linux账号)        切换到有权限的账号下执行该脚本

# 8、配置assembly 打 tar包    
pom.xml添加配置：    

< !-- 将jar包和外部配置等文件整体打包(zip,tar,tar.gz等) -->     

            <plugin>    
                <groupId>org.apache.maven.plugins</groupId>    
                <artifactId>maven-assembly-plugin</artifactId>    
                <executions>    
                    <execution>    
                        <id>full</id>    
                        <!-- 绑定到package生命周期阶段上 -->    
                        <phase>package</phase>    
                        <goals>    
                            <!-- 只运行一次 -->    
                            <goal>single</goal>    
                        </goals>    
                        <configuration>  
                            <!-- 生成tar包时，不在tar包名后面追加assembly id -->    
                            <appendAssemblyId>false</appendAssemblyId>    
                            <!--描述文件路径-->    
                            <descriptors>    
                                <descriptor>src/main/assembly/assembly.xml</descriptor>    
                            </descriptors>    
                        </configuration>     
                    </execution>     
                </executions>     
            </plugin>         
       
参考assembly/assembly.xml   工程demo使用springboot，故assembly.xml是已springboot为例。    
            
            
