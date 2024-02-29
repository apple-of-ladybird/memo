# memo
The memory bank component developed for playing virtual roles in a specific scenario for large-scale models.
本组件为大模型扮演虚拟角色这一特定情境开发的记忆库组件。
其基本逻辑是构建角色四维记忆体系架构，同时不断学习用户和大模型之间的对话，生成新记忆，筛选后将其存入记忆库中。
记忆库内部建立倒排索引，通过关键词去定位相应记忆，提取到prompt中辅助大模型输出。
记忆库内部通过记忆遗忘、关键词概率等机制，为记忆匹配确定优先级。

下面逐一说明代码功能
check_similar:相似度模型部署，简单的bert二分类微调模型
check_style：角色风格/身份信息筛选模型部署，基于特定角色发言（正样本）和其他角色发言（负样本）训练的bert二分类模型，正样本来自下文的基础记忆和核心记忆
clean：内部去重，通过相似度bert模型删除倒排索引中语义相同的记忆
get_keyword：keybert模型部署
get_simi_word：word2vec模型部署，对于某关键词，得到它的近义词群，作为倒排索引的key
get_temp_base：基础记忆生成临时JSON文件，基于角色原作品高质量对话台词，千条左右
get_temp_core：核心记忆生成临时JSON文件，基于self_config标记，百条左右
initiate：一键完成前期模型和数据库部署
main：完成初始化后，程序入口，输入用户发言，返回记忆提取结果
model_test：测试各个模型
write：将JSON临时文件内容写入sqlite数据库

程序基于torch 1.11.0 cuda 11.3 开发 如果你的torch版本超过2 那么应该是用不了torch的

想要使用这个工具，你需要做的事：
1.收集特定角色的核心记忆、基础记忆，然后利用get_temp_base、get_temp_core将其生成符合项目要求的JSON形式
2.基于这些数据，训练特定角色的风格/身份信息筛选模型，以保证你数据库中的数据质量
3.看我的drive文件夹下，将空白的模型全部部署，具体可以查看里面的readme文档
4.在database中建立一个停用词文档，以删除倒排索引中你不需要的关键词

或者，可以稍等一段时间，我会尽快完善现有的问题
学习记忆功能已经开发完成，但学习记忆需要大模型适配才能得到输出，我会在测试完毕后尽快提交相关代码
更具体的技术说明 可以去b站搜索“魔女多萝茜” 有视频讲解 希望能对你有帮助！ 希望和你交流并期待你宝贵的建议
