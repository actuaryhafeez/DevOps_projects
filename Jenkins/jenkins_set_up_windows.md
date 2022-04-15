Download JDK8

 * x86 Installer : https://download.oracle.com/otn/java/jdk/8u321-b07/df5ad55fdd604472a86a45a217032c7d/jdk-8u321-windows-i586.exe
 * x64 Installer : https://download.oracle.com/otn/java/jdk/8u321-b07/df5ad55fdd604472a86a45a217032c7d/jdk-8u321-windows-x64.exe
 

Set the JAVA_HOME Variable
 1. Locate your Java installation directory -> C:\Progra~1\Java\jdk1.8.0_65
 2. Windows 10 – Search for Environment Variables then select Edit the system environment variables
 3. Click the Environment Variables button.
 4. Under System Variables, click New.
 5. In the Variable Name field, enter -> JAVA_HOME
 6. In the Variable Value field, enter your JDK -> C:\Progra~1\Java\jdk1.8.0_65
 7. Click OK and Apply Changes as prompted
 8. go to cmd windows key + R -> type ->
 9.     echo %JAVA_HOME% 
 10.     java --version
