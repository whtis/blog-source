title: GUI练习之二-密码管理器
date: 2016-02-20 21:31:46
categories: java
tags: [java,原创,小练习]
---

### 功能说明及演示
~~**Swing不是线程安全的，因此使用过程中应避免多开程序，防止出现莫名奇妙的错误。**~~
北邮人论坛大神给了解决方案，[链接在这](http://bbs.byr.cn/#!article/Java/47997)，以下内容为复制粘贴：
> Swing不是线程安全的，但也不是大问题。只要把所有的对Swing窗体的的操作都放在专门的线程（event dispatch thread）里做就行了。有一个模式：
```java
SwingUtilities.invokeLater(new Runnable() {
    public void run() {
        createAndShowGUI();
    }
});
```
> 这样就线程安全了。这里有详细说明： http://docs.oracle.com/javase/tutorial/uiswing/concurrency/index.html

#### 说明
综合学习的GUI图形设计和输入输出的知识，实现了一个简易的密码管理器，主要功能如下：
- 可以自己选择密码文件的存放位置
- 可以选择生成的密码是否可以用文本查看器查看
- 可以连续的写入新数据
- 可以根据关键字和密码条件生成相应的密码字符，并提供MD5的显示与查看功能
- 可以根据网站/网址名进行简单的查看功能
- 可以同步显示所有存在的密码文件列表
- 可以选择是否在执行生成按钮后刷新页面，方便直接书写下一个密码文件
- 其他小功能

#### 实现思路
1、存储密码的同时，将生成密码的条件设置为常量，存储在一个整数数组中（waysOfWirte），并将这值附在生成的密码最后，取值时同时取出来，但是不显示在用户界面上。
2、利用文件后缀名的不同区别密码文件的存储格式（二进制存储方式后缀为.w，文本可读后缀名为.wf）。

#### 演示
- 初始化设置，程序会在同目录下创建一个名为`.pmconfig.w`的文件，该文件将密码文件的存放路径写入供以后使用：
![初始化操作.gif](http://7xnttb.com1.z0.glb.clouddn.com/%E5%AF%86%E7%A0%81%E7%AE%A1%E7%90%86%E5%99%A8%E5%88%9D%E5%A7%8B%E5%8C%96%E6%93%8D%E4%BD%9C.gif)
- 主界面的操作：包括密码文件的生成、查看、更新以及删除等操作：
![主界面操作.gif](http://7xnttb.com1.z0.glb.clouddn.com/%E5%AF%86%E7%A0%81%E7%AE%A1%E7%90%86%E5%99%A8%E4%B8%BB%E7%95%8C%E9%9D%A2%E6%93%8D%E4%BD%9C.gif)

### 问题记录
- 如何回车后使光标出现在下一个输入框中：
  使用`requestFocus（）`方法
- 如何创建一个文件夹:
  ```java
  File fileDir = new File("C:\\Users\\ht\\IdeaProjects\\PasswordGenerated\\PasswdGenetated\\");
  if (!fileDir.isDirectory()) {
	  fileDir.mkdir();
  }
  ```
- 如何获取系统剪切板并设置内容
```java
Clipboard clipboard = Toolkit.getDefaultToolkit().getSystemClipboard();
//复制到剪切板上
StringSelection ss = new StringSelection(password);
clipboard.setContents(ss, null);
```
- 如何获取当前系统的目录树并选取目录
```java
	JFileChooser fileChooser = new JFileChooser();
    fileChooser.setFileSelectionMode(JFileChooser.DIRECTORIES_ONLY);
    int i = fileChooser.showOpenDialog(null);
    if (i == fileChooser.APPROVE_OPTION) {
        String path = fileChooser.getSelectedFile().getAbsolutePath();
        String name = fileChooser.getSelectedFile().getName();
        System.out.println(path + name);
    }
```
- 为了保证跨平台使用，将windows路径中的"\\"替换为"/"
```java
s.replaceAll("\\\\","/");
```
- 如何实现初始化界面设置密码文件位置后隐藏该界面，继而出现主界面
PasswordManger类中initFrame和mainFrame的先后设置。
- 如何使JList能够实时显示密码文件的个数
  + 使用`setModel（javax.swing.ListModel<E> model）`方法。该方法要求传入一个Model对象，ListModel的使用用例如下：
  ```java
  public class FileModel extends AbstractListModel {

    private String[] files;

    public FileModel(String[] files) {
        this.files = files;
    }

    @Override
    public Object getElementAt(int index) {
        return (index + 1) + "." + files[index++];
    }

    @Override
    public int getSize() {
        return files.length;
    }
  }
  ```
  + ListModel是一个接口，如果实现该接口，需要实现一些不必要的方法：`void addListDataListener(ListDataListener l)`和`void removeListDataListener(ListDataListener l)`,但是Java提供了一个便捷类AbstractListModel，继承该类，就可以避免实现以上方法。
  + 除了AbstractListModel类，还有其他抽象类如DefaultListModel，但是DefaultListModel实现了ListModel的所有方法，因此可以做的更改比较小。
  + 为了监听FileModel的变化，可以采用这种方法：在每次JList内容发生变化时重新创建一个FIleModel，传入的是更新后的数组，再使用JList的`updateUI()`方法，就可以动态显示JList的内容了。
  *可以给FileModel添加一个监听器类`ListDataListener`,并实现该接口的抽象方法。（网上介绍，未做）*
  ```java
  private void updateJList(String fileDirPath) {
        String[] files = new File(fileDirPath).list();
        FileModel fileModel = new FileModel(files);
        mainFrame.getJlFileName().setModel(fileModel);
        mainFrame.getJlFileName().updateUI();
  }

- 根据指定条件生成密码
  + 我使用如下方法，返回的字符都是字符'A'
  ```java
  private static char generateChar(char aChar, int low, int high) {
        char c = 'A';
        int a = aChar + 3;
        if (a >= low && a <= high) {
            c = (char) a;
  //          return c;
        } else if (a < low) {
  //          return generateChar((char) (a + 6), low, high);
  			  generateChar((char) (a + 6), low, high);
        } else if (a > high) {
  //          return generateChar((char) (a - 30), low, high);
  		      generateChar((char) (a - 30), low, high);
        }
        return c;
    }
  
  ```
  问题原因：递归不设出口，每次递归返回的值都在递归自己这一层，无法返回到最初的一层
  解决方法：使用注释掉的三条语句，删除相应的错误语句。
  **递归不设出口毫无意义**
  + 异常
  `java.lang.stackOverflow` 堆栈溢出，原因是递归太深
- 生成密码的MD5值
```java
private static MessageDigest md5 = null;
    static {
        try {
            md5 = MessageDigest.getInstance("MD5");
        } catch (Exception e) {
            System.out.println(e.getMessage());
        }
    }

private static String generateMD5(String password) {
        byte[] bs = md5.digest(password.getBytes());
        StringBuilder sb = new StringBuilder(40);
        for (byte x : bs) {
            if ((x & 0xff) >> 4 == 0) {
                sb.append("0").append(Integer.toHexString(x & 0xff));
            } else {
                sb.append(Integer.toHexString(x & 0xff));
            }
        }
        return sb.toString();
    }
```
- 初始选取配置文件路径时若该路径存在其他文件，应该避免出现在JList中

### 源码
见我的GitHub：[PasswordManager](https://github.com/whtis/PasswordManager/tree/master/src)

---
<div align="center" style="color:red;width=80px;height:90px;" onmouseout="this.style.border='1px solid blue'" onmouseover="this.style.border='none'">
<p style="font-weight:bold;font-style:italic;">本文章首发[www.whtis.com](http://www.whtis.com)，转载请注明出处</p>
</div>