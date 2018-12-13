# face_recognition
在线人脸识别

参考基础：https://github.com/Hironsan/BossSensor 有一些不同：
* 1. 识别人脸的可扩展性。原有的demo只能识别一个人（他老板），新的demo在数据集的扩展性上有修改，理论上可以识别无数人的脸，只要你有足够多的数据；
* 2.神经网络设计更加简单。新的demo在卷积神经网络的设计上比原有的卷积神经网络更加简单和容易理解，没有搞得那么复杂，更加适合入门；
* 3.代码结构更清晰。程序设计更加模块化，把能拆分出来的尽量拆分出来，基本上每个文件都可以单独拿出来测试和使用；
* 4.详尽的中文注释。你可以看很详细很详细的中文注释、不用看日文和英文的注释了......

 

### Step 1  环境配置
       环境配置这块，网上的资料非常多，总体来说我给大家的建议是：使用集成python环境  Anaconda，里面已经集成了很多有用的安装包比如说numpy、scipy，免去了自己配置的痛苦；通过Anaconda安装新的计算包也非常方便，具体就不再赘述了，网上可以找到很多教程，多百度、多Google。我可以再提醒一句的是，好像Anaconda的theano还有tensorflow的包都有点小问题，需要删了然后重新下载配置，网上也有教程。

简而言之，使用Anaconda，安装好必要的科学计算包：numpy,scipy,sklearn,keras,opencv。贴一个别人的环境配置教程,仅供参考：

http://machinelearningmastery.com/setup-python-environment-machine-learning-deep-learning-anaconda/

### Step 2 获得训练数据集
       第二步就是要获得数据训练集，你可以从网上找训练集，也可以用自己或者朋友的照片来做素材（顺手练习下opencv和os），具体操作办法：

* 打开pick_face.py 这个程序，里面有一个函数readPicSaveFace(sourcePath,objectPath,*suffix)，sourcePath是存储图像源的文件夹，objectPath是存储识别出的人脸的文件夹，看这个函数的备注：

前面是你存储源文件图片的文件夹地址，后面那个是你存储剪裁好、处理好的人脸图片的地址，最后是源文件图片的格式（文件后缀）。

通过这个函数你就可以很方便地把各种图片中的人脸给扣出来并保存下来了。像我的人脸就放在了“/home/hezhiqiang/PycharmProjects/pictures/source-jerry”这个文件夹中

### Step 3 构建模型和进行模型训练
* 当你建立好你的的数据集之后，应该会有一个dataset的总文件夹，dataset下会分几个文件夹，你想让你的模型识别几张人脸就建立几个子文件夹，每个子文件夹里面应该有至少几十张格式一致的照片（训练素材越多越好）。然后进入train_model.py，输入你的数据集地址，我的数据集地址是 “/home/hezhiqiang/PycharmProjects/pictures/dataset”，文件夹名字以需要显示在人脸框上的文字对应，然后建立模型、读取训练集、训练模型、评估模型，最后存储模型,训练好的模型会存储在Model这个类中写死的一个固定位置中.

### Step 4 打开摄像头验证模型效果
* 模型训练好之后，打开read_camera.py，这个文件中有一个Camera_reader的类，在模型初始化的时候就加载你之前训练好的模型：
* 然后在建立一个Camera_reader的实例之后调用build_camera（）的方法，该方法打开摄像头，并对视频流中读取到的人脸交给model进行识别：
* 模型的predict()函数会返回两个参数，第一个是概率最高的label的index,第二个参数为对应概率，我们会对概率进行下判定，如果高于70%我们就认为模型是可靠的（这个值自己可以调整观察带来的影响），可以显示具体的label，反之我们认为识别出的人脸是一个Stranger。

### 总结
         这个人脸识别系统是具有自己学习能力的，你给它喂的数据越多，它就可以识别越多的人而且准确度会不断提高，希望大家可以自己测试和研究。这个demo也有几个问题需要提醒大家：
* 1. 模型有过拟合的问题。在训练的时候如果epoch太高，会发现accuray虽然逐步提高甚至到达95%以上，但是实际test的准确度要比training的时候低很多，这个就说明模型出现了过拟合的问题。解决办法，增加样本类别和样本数，这个需要花费很多功夫；调整模型，但是在样本数量比较小的时候不是很显著。
* 2. openCV的人脸识别精度还是有点问题，这个读者可以在使用pick_face.py进行素材抓取的时候感受到，你放入的生活照片大概只有70%-80%可以读取人脸素材出来，有的照片明明有你的脸，但是openCV认为那不是一张脸，这个就很尴尬了有没有；关于openCV的进阶可以找本书看看，我推荐机械工业出版社出的《OpenCV 3计算机视觉Python语言实现（原书第2版）》。

