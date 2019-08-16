<https://developer.huaweicloud.com/competition/competitions/1000007620/introduction>

**【赛题说明】**

​       本赛题采用深圳市垃圾分类标准，赛题任务是对垃圾图片进行分类，即首先识别出垃圾图片中物品的类别（比如易拉罐、果皮等），然后查询垃圾分类规则，输出该垃圾图片中物品属于可回收物、厨余垃圾、有害垃圾和其他垃圾中的哪一种。

模型输出格式示例：

{

​    " result ": "可回收物/易拉罐"

}

**【垃圾分类标准】**

​        可回收物指适宜回收和资源利用的废弃物，包括废弃的玻璃、金属、塑料、纸类、织物、家具、电器电子产品和年花年桔等。

​        厨余垃圾指家庭、个人产生的易腐性垃圾，包括剩菜、剩饭、菜叶、果皮、蛋壳、茶渣、汤渣、骨头、废弃食物以及厨房下脚料等。

​        有害垃圾指对人体健康或者自然环境造成直接或者潜在危害且应当专门处理的废弃物，包括废电池、废荧光灯管等。

​        其他垃圾指除以上三类垃圾之外的其他生活垃圾，比如纸尿裤、尘土、烟头、一次性快餐盒、破损花盆及碗碟、墙纸等。

**【数据说明】**

​        本次比赛提供的训练集中包含了生活中常见垃圾，参赛者可自行划分用于模型调优用的验证集和测试集。[**点击此处**](https://modelarts-competitions.obs.cn-north-1.myhuaweicloud.com/garbage_classify/dataset/garbage_classify.zip)可以下载数据garbage_classify.zip。

解压后得到的目录结构如下：

| 目录名                     | 说明                                                         |
| -------------------------- | ------------------------------------------------------------ |
| train_data                 | 训练集目录，包含垃圾图片和对应的标签文件（.txt）             |
| garbage_classify_rule.json | 垃圾分类规则字典，key值是id，value是“垃圾种类/具体物品名”。例如训练数据标签文件img1.txt的内容是“img_1.jpg, 0”，表示img_1.jpg这张图中的垃圾是“其他垃圾/一次性快餐盒” |

说明：除本次大赛提供的训练数据之外，参赛选手也可以使用其他来源的垃圾图片数据。

**【评分标准】**

本次大赛采用识别准确率（recognition accuracy）作为评价指标。

如上文模型输出格式示例中，模型预测的物品类别是“易拉罐”，如果图片的真实类别是易拉罐，则这张图片预测正确，否则预测错误。评价指标的计算方式是：

​       **识别准确率 = 识别正确的图片数 / 图片总数**

识别准确率的数值即为最终的模型评分。

**【提交说明】**

所有参赛选手需使用华为云一站式开发平台ModelArts来开发模型，然后在ModelArts平台上将开发好的模型发布给大赛评审账号，最后在竞赛平台上提交作品、查看成绩。

**【提交方法】**

​        **步骤一 在ModelArts平台上发布模型**

在ModelArts上提交前，需要在“模型管理”中导入您训练好的模型，**然后将模型部署为在线服务验证模型的可用性和准确性**，最后将模型“发布”给大赛评审账号d8126a20db13499c82e060007d1e8348。

1） 参考ModelArts用户指南中[模型管理](https://support.huaweicloud.com/engineers-modelarts/modelarts_23_0054.html)，将生成的模型导入至ModelArts模型管理。

2） 参考ModelArts用户指南中[部署为在线服务](https://support.huaweicloud.com/engineers-modelarts/modelarts_23_0060.html)，将导入的模型部署为在线服务。

3） 参考ModelArts用户指南中[图片预测](https://support.huaweicloud.com/engineers-modelarts/modelarts_23_0062.html#modelarts_23_0062__section1666533761611)，上传图片测试模型的输出是否正确。如果不正确，则需要根据日志中报错信息进行问题的排查。

4） 在ModelArts左侧导航栏中选择“模型管理”，然后单击页面右侧操作栏中“市场发布”。

![img](https://dsfile.devcloud.huaweicloud.com/CompetitionUploadService/v1/noauth/competition/downLoad/image?uuid=af26903e8fc8476096160b54a44b4c8f)

5） 在发布模型页面填写参数，其中“发布到”选择“个人”，填写评审账号ID：d8126a20db13499c82e060007d1e8348，然后单击“添加”。

![img](https://dsfile.devcloud.huaweicloud.com/CompetitionUploadService/v1/noauth/competition/downLoad/image?uuid=ac2d246871054e64a4c25db44d3fa515)

6） 单击“确定”，完成模型在ModelArts平台的提交。

​    **步骤二 在竞赛平台上提交作品**

在大赛平台上点击提交作品-上传作品，选择已发布给大赛指定账号的模型，其中“提交作品”页面需报名比赛后才会显示。

![img](https://dsfile.devcloud.huaweicloud.com/CompetitionUploadService/v1/noauth/competition/downLoad/image?uuid=fc7d93e2253e4fe6b7e4f9b0f5399db7)

**步骤三 查看成绩**

1） 模型提交完成后，等待一定时间（判分系统进行判分需一定时间，运行时长与选手提交的模型有关），判分系统完成判分后，可在竞赛平台“提交作品”中查看得分、反馈信息。

2） 排行榜每天更新一次，第一天提交的模型得分排名会在第二天更新。

**【提交时间】**

本次比赛初赛提交时间段为：**8月6日~ 9月10日**，参赛选手可以多次提交模型。所提交的模型得分可在大赛平台页面“提交作品”中实时查询。

本次比赛决赛提交时间段为：9月11日~ 9月19日。

**【模型规范】**

1） 所提交的模型必须满足赛题说明中的模型输出格式；

2） 评分系统使用ModelArts批量服务加载选手提交的模型，对比赛评分专用的测试集图片（此部分图片不公开）进行批量预测，后台会根据预测结果自动计算识别准确率；

3） ModelArts模型管理中的模型创建后，不会自动更新，如果您有了更好的模型需要提交，要重新导入模型，然后再重新发布模型、提交作品；

4） 排名靠前选手所提交的模型，会被择优发布到ModelArts AI市场。

**【参考资料】**

**赛题Baseline**

本次大赛针对赛题提供了Baseline文档及相关代码文件，**建议所有参赛选手先按照文档在ModelArts中完成模型的训练、部署并测试以及模型的提交，**以快速熟悉ModelArts开发平台、体验参赛答题涉及到的各个环节。

Baseline代码文件中的模型训练代码是基于Resnet实现的垃圾图片分类模型，参赛选手可以选择在此基础上进一步进行模型调优，也可以使用自己熟悉深度学习框架开发模型。

Baseline文档及代码下载地址：[点击下载  ](https://bbs.huaweicloud.com/blogs/9e1b7c52b36e11e9b759fa163e330718)   

**模型包规范及示例代码**

大赛建议参赛者使用TensorFlow、PyTorch或MXNet框架开发模型，这样可以确保大赛自动评分系统能够有效对接参赛者所提交的模型。Baseline代码仅提供了使用TensorFlow的代码示例，如您使用其他框架，请点击[参考](https://support.huaweicloud.com/engineers-modelarts/modelarts_23_0091.html)，准备好配置文件和推理代码，保证您提交的模型符合ModelArts模型包规范。

**其他资料**

​      ·**ModelArts****常见FAQ-V1.0**

该文档包含了使用ModelArts时常见FAQ，如在ModelArts实际开发模型时遇到问题，**参赛者优先去该文档寻找答案**，如未能解决，再向大赛组寻求支撑。

ModelArts常见FAQ-V1.0下载地址：[点击前往下载](https://bbs.huaweicloud.com/blogs/b4ad5ce3b1ec11e9b759fa163e330718)。

· **ModelArts****用户指南**

该文档包含ModelArts开发环境、训练、模型管理以及部署上线等各个功能模块介绍，如在使用ModelArts对相关操作使用存在问题或疑问，可翻阅该文档。

ModelArts用户指南链接为：[点击查看](https://support.huaweicloud.com/engineers-modelarts/modelarts_23_0001.html)。

注意：为避免造成资源浪费，使用完ModelArts Notebook、TensorBoard、在线服务，务必要及时停止作业以释放资源，否则可能会导致账号欠费。

ModelArts计费项请[点击查看详情](https://support.huaweicloud.com/price-modelarts/modelarts_07_0002.html)。





