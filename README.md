## CCKS 2019 中文短文本的实体链指
* 时间：2019.04.19-2019-07-25
* 名次：15/346
* 最终得分：
    * F1:0.780048270313757 P:0.7710536779324055 R:0.7892551892551892  
    *第一名成绩：F1:0.80143* 


### 介绍
> 背景介绍:  
近年来，随着深度学习的重燃以及海量大数据的支撑，NLP领域迎来了蓬勃发展，百度拥有全球最大的中文知识图谱，
拥有数亿实体、千亿事实，具备丰富的知识标注与关联能力，不仅构建了通用知识图谱，
还构建了汉语语言知识图谱、关注点图谱、以及包含业务逻辑在内的行业知识图谱等多维度图谱。
我们希望通过开放百度的数据，邀请学界和业界的青年才俊共同推进算法进步，激发更多灵感和火花。
面向中文短文本的实体识别与链指，简称ERL（Entity Recognition and Linking），是NLP领域的基础任务之一，
即对于给定的一个中文短文本（如搜索Query、微博、用户对话内容、文章标题等）识别出其中的实体，并与给定知识库中的对应实体进行关联。ERL整个过程包括实体识别和实体链指两个子任务。 
传统的实体链指任务主要是针对长文档，长文档拥有在写的上下文信息能辅助实体的歧义消解并完成链指。
相比之下，针对中文短文本的实体链指存在很大的挑战，主要原因如下：  
（1）口语化严重，导致实体歧义消解困难；   
（2）短文本上下文语境不丰富，须对上下文语境进行精准理解；  
（3）相比英文，中文由于语言自身的特点，在短文本的链指问题上更有挑战。  
竞赛任务:  
输入：  
输入文件包括若干行中文短文本。  
输出：  
输出文本每一行包括此中文短文本的实体识别与链指结果，需识别出文本中所有mention（包括实体与概念），每个mention包含信息如下：mention在给定知识库中的ID，mention名和在中文短文本中的位置偏移。

[比赛官网](https://biendata.com/competition/ccks_2019_el/)

### 工程运行
* 配置
    * 运行*pip install -r requirements.txt*安装大部分必要的包
    * 自行下载bert的*chinese_L-12_H-768_A-12*放在*bert/bertModel*下
    * 请自行下载官方数据：train.json、kb_data，并将其放在*ml/data*目录下
* 执行
    * 运行*ml/pre_process.py*对数据进行简单的预处理，删掉一些实体重合的数据
    * 运行*ml/data_reduce.py*打上BIO标签
    * 运行*ml/my_bert_crf_model.py*训练提取实体模型，自行设置模型的保存路径和加载路径
    * 运行*ml/my_bert_findId.py*运行消歧模型，从kb_data中找到对应的kb_id，自行设置模型的保存路径和加载路径
* 交互界面
    * 套用了https://github.com/zhaoyingjun/chatbot 中的聊天机器人界面
    * 在*app.py*中配置监听端口，运行后通过浏览器访问
    * 预览：![image](https://user-images.githubusercontent.com/25412051/61956626-1a506900-aff0-11e9-84dc-b83967581235.JPG)
    
### 模型介绍
* 提取实体模型
    * bert + crf 
    * 线下提取实体F1值：0.8233
* 消歧模型
    * bert分类  
    * 线下纯消歧F1值：0.8949
   
(关于keras模型的搭建参考苏剑林同学的[博客](https://spaces.ac.cn/))
* crf++  
*ml/data/crfpp_erl.py*
    * 该模型没有采用到比赛中，线下实体提取准确率大概为0.76。虽然效果比bert差，
    但是训练快，只需几百秒就可以训练完。  
    
(crf++参考的是达观比赛官方的baseline)

### 后处理 (效果提升不是很大)
*ml/back_process.py*
* 提取一句话中所有的相同的实体
* 粗粒度处理：若两个实体相邻，并且结合起来的实体在kb_data中，则选取结合起来的实体
    
    




