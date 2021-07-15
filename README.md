# 						***\*概要设计说明书\****

**1.** ***\*引言\****

随着技术经济的发展和城市化进程的加快，安防监控逐渐遍及小区、超市、校园以及其他很多公共场合。为了更好的保障人民的安全、服务人民群众的生活，政府、企业和学者都投身于对行人追踪的研究。此款软件可以帮助用户解决根本问题，可以对单个视频进行追踪处理并返回实时数据，有较大的参考意义。

 

***\*1.1\*******\*编写目的\****

该篇以及后两篇文档均适用于使用这款软件的用户或企业，目的是为了更好的帮助使用这款软件的人群能有更好的使用体验。

 

***\*1.2项目背景\****

l 此次赛题由百度飞桨设置，免费提供Tesla V100 GPU算力，要求选手使用百度AI Studio平台进行训练，基于百度飞桨PaddlePaddle框架进行开发，设计一款可以实现行人跟踪的软件。

l 此款软件由长春工业大学团队独立开发制作，耗时三个月，由本校计算机科学与工程学院教授张丽娟老师指导完成。软件可以脱机运行同时性能上相对较好，在一定程度上可以完成复杂的行人检测以及数据分析。

**2.** ***\*任务概述\****

***\*2.1目标\****

完成模型训练，并最终封装成可以实现交互的运行软件。用户可以上传视频完成对视频的行人检测并同步输出行人数和结果的置信度。

 

***\*2.2运行环境\****

Windows10 64bit 
cuda11.1 cudnn8.0.5
python3.8
python包paddlepaddle-gpu matplotlib x2paddle flask

 

***\*2.3条件与限制\****

因共同使用了Vue和Flask前端模板，导致整合过程中出现不少问题。前后端耦合性较强，不容易对代码进行大规模修改。

**3.** ***\*数据描述\****

***\*3.1\**** ***\*JSON数据\****

JSON是当前行业内使用最为广泛的数据传输格式之一，用于实现前后端之间的数据交互。对于本软件前端均使用JSON格式向后端发送数据并响应处理。而后端处理完成后也以JSON数据的方式返回给前端。

 

***\*3.2\**** ***\*动态数据\****

本软件的动态数据为模型对检测视频后返回的两个结果，分别为行人数和置信度。

 

***\*3.\*******\*3 静态数据\****

前端采用VUE模板，静态数据即为data（）域中所给定的数据，后端采用Python  Flask框架，静态数据即为所设置的本地变量。

 

***\*3.\*******\*4 模型采集数据\****
<img width="800" src="https://github.com/qianjingning/mot/blob/343ec468111df948c70ce8e87447e6f86e0259fe/%E5%9B%BE%E7%89%87/%E5%9B%BE%E7%89%87%203.png">


<img width="800" src="https://github.com/qianjingning/mot/blob/73d268d41f7ee925613e54955ebc03194e3b1d7f/%E5%9B%BE%E7%89%87/%E5%9B%BE%E7%89%87%201.png">

***\*用户界面设计\****

![img](file:///./图片/图片 1.png) 

 

 页面布局采用ElementUI中的导航页模板，分为header，footer，naviBar，center四个模块。header为页面上方条形区域，显示了软件的名称。footer显示了长春工业大学校徽以及开发团队。naviBar为左侧区域，展示了软件的一些基本介绍和功能文档。center模块即为中间功能区域，放置了行人检测样例的图片的轮播器以及用户上传视频入口，同时设置了五个阈值的设置框。用户可以自行设置。

 

![img](file:///./图片/图片 2.png) 

界面最下方这两个区域分别是回显的帧处理视频以及回显的每帧数据结果。上传视频并手动刷新后即可分别看到经检测后的视频以及数据。

 

**5.** ***\*制定规范\****

 

**5.1** ***\*设计原则\*******\*
\****	采用前后端分离模式开发，保证开发阶段前后端互不干涉彼此行为。开发完成后统一整合到后端，保证程序运行只有唯一端口。方便用户体验。

 

**5.2** ***\*代码规范\****

VUE中严格按照ES6代码规范，Flask中按照python代码的规范书写，以代码行对齐和行相错识别层级。

 

**5.3** ***\*命名规则\****

命名均以实现的功能操作命名，通俗易懂，命名方法采用驼峰命名法。

 

 

 

 

# 						***\*详细设计说明书\**** 

**1.** ***\*总体设计\****

***\*1\*******\*.1需求概述\****

**l** ***\*前端界面\****

**l** ***\*后端控制功能部件\****

**l** ***\*行人检测训练模型\****

 

***\*1\*******\*.2软件结构\**** 

如给出软件系统的结构图。

![img](file:///./图片/C9F754DE-2CAD-44b6-B708-469DEB6407EB-1.png) 

 

**2.** ***\*程序描述\****

 

***\*2\*******\*.1\**** ***\*软件中各模块介绍说明\****

(1) VUE

①　Build

该模块下放置build.js用于设置创建VUE框架的基本参数以及对框架内部书写代码结	构的定义，还有webpack.dev.conf.js等对打包环境的参数设置的文件，属于项目根文	件。

②　Config

该模块下对不同的开发环境的参数进行不同设置，分为dev（开发阶段），prod（生产阶段）以及test（测试阶段），同时index.js文件作为内置服务器核心配置文件，设置了包括端口，主机号以及定义了对文件路径检索的规则。

③　Src

该模块下用于放置开发阶段所有的源代码，包括静态文件（CSS，JPG），脚本文件（JS），前端HTML文件，交互请求参数的封装与设置，路由，ElementUI可视化工具等。

l App.vue

是前端项目的唯一入口，作为标签最外层，通过路由设置将对应的template模板插入并显示在浏览器。

l FrameShown.vue

Template模板文件，真正用于显示在前端页面的文件。根据Vue文件格式，可同时写入HTML，JS，CSS等文件。

l Main.js

创建一个实际意义上的Vue对象，用来将视图页面挂载其下，从而调用内置服务器运行。

(2) Flask

l App.py

后端代码，完成各种功能操作（视频上传，帧处理，向前端发送数据），同时包含视图函数。

 

***\*2\*******\*.2算法\*******\*详解\****

(1) VUE  FrameShown.vue

①　submitParameters()& resetForm(formName)

   ![img](file:///./图片/图片 1(1).png)

submitParameters（）函数被绑定在提交按钮上，即点击按钮出发此函数。本地变量data（）中给定义了formLabelAlign字典，里面分别设定了五个阈值，并且在用户设置阈值时可以在输入框中获取输入值，并将他们赋值给另外已设定好的阈值变量。而resetForm（formName）的目的是为了提交完成以后自动清除用户在输入框中输入的值以便下次输入。因此代码中才需要两个相同的阈值变量，一个接收，一个负责传值。而接收变量的值会在提交申请触发后被清除。

②　beforeUploadVideo()& uploadViedeoProcess()& handleVideoSuccess()

![img](file:///./图片/图片 3.png) 

该三个函数名是VUE插件ElementUI中上传组件的自带响应函数，分别表示上传前所进行的操作，上传过程中所进行的操作以及上传成功后所进行的操作。上传前会调用beforeUploadVideo()函数进行对文件格式和大小的检测，如果不符合条件即会显示提示消息。file参数即为在前端界面所获取的上传视频对象。上传过程中会调用uploadViedeoProcess()函数控制上传进度条的进度，根据文件已上传的大小和文件大小的比值实时在前端显示进度。上传成功后会调用handleVideoSuccess()，首先将进度条置为100%，同时将上传状态置为成功，这样用户在界面上会看到显示绿色箭头。文件的上传操作访问地址已在标签中提前设置好，即为标签属性action所设置的地址值。此时后端Flask中对应的路由函数被调用，将视频上传保存后返回状态码resCode，handleVideoSuccess()判断状态码，若正确则将一开始用户提交的五个阈值封装为JSON数据通过get请求发送给对应后端的路由函数。

![img](file:///./图片/图片 4.png) 

(2) Flask	app.py

①　三个路由模板函数

![img](file:///./图片/图片 5.png) 

Index.html为前端VUE项目打包形成的主页面，后端Flask在python下创建了两个独立的HTML模板data.html,Frame.html分别用于作为输出结果显示和视频帧显示的界面。render_template方法即指定在对应的路由下渲染哪一个HTML文件。注意data（）路由函数渲染时多带了一个data参数，此为返回的每帧的输出结果，并可以一直获取数据而不是一次性。

②　def upload()& def directory()

 ![img](file:///./图片/图片 6.png)



![img](file:///./图片/图片 7.png)



由上VUE函数介绍，这两个函数为前后端交互函数。由VUE发送上传视频请求到upload路由后，upload函数获取到视频文件，同属对其上传路径进行修改，为了保证每次上传的文件名唯一，选用了uuid组件对视频名进行随机匹配，同时将开始设置好的基本地址url（C://user/Videos）和文件名拼接组成新地址，视频格式统一改为MP4格式。保存文件后发送状态码给前端，由于前后端分离开发，端口不同，存在跨域交互数据。故发送需要CORS认证，即发送请求时加上请求头。directory则接收的是前端发送过来的用户输入的五个阈值，分别获取后保存在本地全局变量中，以供后面进行帧处理使用。

③　def Trackback() & def video_feed()

 

④　def create_data_info() & main

![img](file:///./图片/图片 10.png) 

create_data_info()函数作用即为获取输出结果，并持续向上述的data（）路由函数传递数据。main函数为程序主函数，设定运行端口并同时开启debug功能。

 

***\*2\*******\*.3程序逻辑\*******\*图\****：

![img](file:///./图片/图片 14.png) 

 

 
## 目标追踪部分算法详细设计
目标追踪算法分为两个部分，目标检测部分，追踪部分，前者使用yolov5实现，后者使用deepsort实现。其中yolov5使用的结构如下图：


deepsort中深度特征提取部分使用的结构如下图：



yolov5检测部分由./detect.py和./model.py组成，model模块包含网络的定义，使用ONNXModel(paddle.nn.Layer)类实现，通过在yolov5的yolov5s6模型上进行微调得到的模型转换成onnx模型并使用x2paddle转换成paddlepaddle模型得到paddle模型的权重。在paddlepaddle中重现输入输出接口。方便起见，所有yolo相关代码防御flask的入口文件app.py中。

    class ONNXModel(paddle.nn.Layer)
	    该类为模型结构定义，可用的方法有：
	    

     - `model.set_dict(params）`
    
           input:通过paddle.load加载的模型权重
           output:true
      
    
     - `model(source)`
    
    	    input:要进行检测的图像或者视频
    	    output:预测结果

  
---------
    class LoadImages()
        	该类为模型的输入类，可以为视频或者图片或者图片目录，实际使用中读入一张图片并所放缩到合适尺寸以及调用letterbox进行填充和通道变换。
    
---------    
    fun letterbox(img, new_shape,stride,scaleup)
    	    该函数对图片进行缩放操作
    
     input : 
    
     - img
     - new_shape
     - stride
     - scaleup
	
	output:
	

	 - img
	 - radio
	 - dw
	 - dh

----------

    fun non_max_suppression(bboxes, scores, score_thresh, nms_thresh)
		    非极大值抑制函数，通过给定的置信度阈值和iou阈值过滤多余的绑定箱
	input:
	
	- bboxes
	- scores
	- score_thresh
	- nms_thresh

	output:
	- bboxes
	- scores
	

----------

    fun plot_one_box(x, im)
    - 输入预测箱x以及图片im
    - 输出绘制了预测箱的图片
    
    
----------

    fun box_iou_xyxy(box1, box2)
        计算两个预测箱的iou大小，输入格式为xyxy.
        
----------
    fun scale_coords
        用于将预测坐标映射回原图尺寸
        
    
    
----------
deepsort追踪部分位于deepsort子包，主要有特征提取的deep部分和匹配相关的sort部分，入口模块为deepsort,主要包含deepsort类：

    class DeepSort(object)


-----------
deep子包包含深度特征提取器，其模型文件位于model模块下。特征提取定义在feature_extractor模块下的Extractor类：

    class Extractor(object):

--------
特征提取网络定义在model模块下的Net类中。

    class Net(nn.Module)
    该模型以固定尺寸[1,2,640,640]作为输入。


-------------
sort子包包含了跟踪器定义，跟踪器之间的级联匹配以及卡尔曼滤波算法。


--------------------------------------

track模块用于将yolo模型与deepsort模型进行结合，将yolo的检测结果传递给deepsort并进行跟踪器更新。



 

 

 

 

 

 

 

 

 

 

 

 

# 				***\*软件需求及操作使用说明书\**** 

**1.** ***\*功能需求\****

 

***\*1\*******\*.1功能划分\****

l 完成模型训练并上传到AI Studio评测入口得到分数

l 可以实现单摄像头目标追踪

l 最终成型的产品是可交互的运行软件

l 用户可以使用软件上传视频，实现单摄像头下的目标追踪

l 软件可以实时输出图像中的人数并可以设置结果输出的置信度

l 软件可运行在Windows或Linux平台

 

***\*1\*******\*.2功能描述\****

l 

l 训练模型将会对每帧进行图像处理，完成对行人的识别，同时计算出行人每帧的坐标并对识别出的行人进行框出标记。

l 软件使用前后端分离模式独立开发，可以实现前后端交互数据，达到输出结果可以正确的在用户界面显示。

l 用户在前端界面上传合适格式的视频并设置阈值，后端接收后进行视频保存和处理。训练模型计算出每帧行人数和对是否为行人的概率判断（置信度）并返回结果，前端每隔相同帧间隔刷新一次返回结果，达到实时动态同步数据更新。

l 每帧处理的结果会返回行人数和置信度两个参数，通过局部界面实时刷新达到持续更新输出数据。

l 软件可以在Windows下运行，经测试，360 IE7版本及以上，Google chrome 等主流浏览器均可正常运行用户界面。

 

 

**2.** ***\*性能需求\****

 

***\*2\*******\*.1数据精确度\****

 

***\*2\*******\*.2\**** ***\*时间特性\****

l 更新结果数据时间：25ms

l 帧间隔：25ms

l 视频上传时间：上传一个15s视频大约需要0.45s

l 运行时间：略大于视频时间（每帧处理需要时间，但时间可以忽略不计）

 

**3.** ***\*运行需求\****

360 IE7及以上，google chrome等主流浏览器均可兼容

 

**4.** ***\*使用说明\****

 

***\*4\*******\*.1安装和初始化\****

给出程序的操作命令、反馈信息及其做含意、表明安装完成的测试实例以及安装所需的软件工具等。

 

***\*4\*******\*.2输入\****

●  数据背景

软件中用户可以自行设置阈值以达到自己想要输出的结果。（若不输入即为默认的处理结果）

● 数据格式

(1) 上传视频支持的文件格式必须为avi,mp4,ogg,wmv,rmvb,flv。

## 相关输入参数介绍

 - conf-thres :置信度阈值用于滤除不相关的目标，置信度越低表示将目标分类为人的可能性越低，通过该值调整输出的目标的数量
 - iou-thres：iou阈值用于调整非极大值抑制过程中预测箱的过滤，大于该阈值的预测箱表示同一目标，会将这些预测箱丢弃，可以防止同一个目标出现多个预测箱的情况。
 - source：表示输入的视频源地址。



 

***\*4\*******\*.3输出\****

## 相关输出参数介绍

 - center(x,y)：目标人物位置中心在图片上的坐标
 - (w,h)：目标任务预测箱的宽高大小
 - id：每个人在视频中的唯一id,相同id表示同一个人。
 - num：视频中总的人物数量
 - conf：每个人的置信度大小
 

***\*4\*******\*.4出错和恢复\****

①　若上传视频时提示“请上传正确格式”，用户应注意自己上传的视频格式，此软件所支持的视频格式可查看4.2数据格式栏。

②　若上传视频时没有没有任何消息提示，用户可以尝试重新上传一遍。

③　若打开页面和上传出现明显卡顿并且上传提醒十分缓慢，建议检查电源是否松动，尽量保证插电情况下运行软件。

 

***\*4\*******\*.5求助查询\****

用户将所需检测的视频拖拽至视频文件框或是点击上传按钮查找目录选择视频文件上传。当显示文件上传成功的消息以后用户需点击浏览器左上角的刷新按钮进行一次整体页面的刷新。此时即释放了视频帧和数据的同步请求渲染，用户即可以实时观察到对行人的动态检测。

 

**5.** ***\*用户操作举例\****

 点击PedestrianSearching.exe文件打开软件，exe文件绑定了localhost:10000端口打开浏览器输入以上地址进行访问。进入页面显示，设置五个阈值（可以不全设/也可以完全不设），点击提交。然后点击视频上传弹出文件目录，选择需要上传的视频，点击确定。当看见提示视频上传成功的消息后，点击浏览器左上角的刷新按钮即可。

 
