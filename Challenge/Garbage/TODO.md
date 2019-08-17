**项目思路：**

**1. 资料检索**

参考相关项目，完善项目思路

- <https://www.flyai.com/d/Garbage>

**2. 数据集扩增**

- [ ] 修改拼接已有数据集<https://www.kaggle.com/techsash/waste-classification-data>
- [ ] 爬虫制作数据集

**3. 数据增强**

- [ ] 翻转，裁剪等操作扩增数据集

**4. 修改模型**

- [ ] 本地训练模型，查找测试结果差原因
- [ ] 减少网络层数，防止过拟合



ResNet50:

- 去掉classes参数
- base layer 后面几层可训练，慢慢增加可训练层数
- 增加一个FC层，添加dropout防止过拟合