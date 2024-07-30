之前发了So-VITS-SVC 4.0的整合包和训练/推理教程（BV1H24y187Ko），两天内收到几十条私信和两百多条评论，大多都是各种环节报错出问题了，其中又有大多都是类似的问题，实在回复不过来了，所以我总结了几条常见的报错和解决方法，以供自查。文章末尾还有一些重要的Q&A，即便你没遇到这些问题，也建议去看一下。

还有，有问题尽量不要给我发私信，我不一定看得到……直接在视频底下评论就好，说不定有热心人能帮到你。


报错：UnicodeDecodeError: 'utf-8' codec can't decode byte 0xd0 in position xx

答：数据集文件名中不要包含中文或日文等非西文字符。


报错：页面文件太小，无法完成操作。

答：调整一下虚拟内存大小，具体的方法各种地方一搜就能搜到，不展开了。


报错：UnboundLocalError: local variable 'audio' referenced before assignment

答：上传的推理音频需要是16位整数wav格式，用Au转换一下就好。或者装个ffmpeg一劳永逸地解决问题。


报错：AssertionError: CPU training is not allowed.

答：非N卡跑不了的。


报错：torch.cuda.OutOfMemoryError: CUDA out of memory

答：爆显存了，试着把batch_size改小，改到1还爆的话建议云端训练。


报错：RuntimeError: DataLoader worker (pid(s) xxxx) exited unexpectedly

答：把虚拟内存再调大一点。


报错：NotImplementedError: Only 2D, 3D, 4D, 5D padding with non-constant padding are supported for now

答：数据集切片切太长了，5-10秒差不多。


报错：CUDA error: CUBLAS_STATUS_NOT_INITIALIZED when calling 'cublasCreate(handle)'

答：爆显存了，基本上跟CUDA有关的报错大都是爆显存……


报错：torch.multiprocessing.spawn.ProcessExitedException: process 0 terminated with exit code 3221225477

答：调大虚拟内存，管理员运行脚本


报错：'HParams' object has no attribute 'xxx'

答：无法找到音色，一般是配置文件和模型没对应，打开配置文件拉到最下面看看有没有你训练的音色


报错：The expand size of the tensor (768) must match the existing size (256) at non-singleton dimension 0.

答：把dataset/44k下的内容全部删了，重新走一遍预处理流程


报错：Given groups=1, weight of size [xxx, 256, xxx], expected input[xxx, 768, xxx] to have 256 channels, but got 768 channels instead

答：v1分支的模型用了vec768的配置文件，如果上面报错的256的768位置反过来了那就是vec768的模型用了v1的配置文件


Q&A:

Q: 跑这个的最低配置要求是啥啊？

A: 支持CUDA的6G显存以上的N卡，硬盘也留足一点空间。

Q: A卡真的跑不了吗哭哭

A: 理论上可以在Ubuntu或Linux环境下通过ROCm来实现，但是比较麻烦，小白建议放弃直接去云端。

Q: 我的显卡达不到最低要求，云端又心疼钱，真的没法训练了吗？

A: 建议去看DDSP-SVC项目（BV1qM411W7ft），效果差一点但也能听，最重要的是对低配非常友好。

Q: 用UVR5分离人声的时候声音会失真，还有什么更给力的工具吗？

A: 理论上UVR5已经是目前最强的人声分离工具了，原曲如果伴奏声音太大轨道太复杂是一定会有失真的，建议选原曲的时候选择伴奏简单人声清楚的效果会好很多。

Q: Audio Slicer 切出来的音频有的长达几十秒甚至几分钟，是怎么回事？

A: 切片长度建议5-15秒，训练时过长部分会被自动丢弃。切出来过长的音频可以调整一下slicer里的maximum silence length这一条，改成500或者更低。还有过长的音频就自己用Au之类的手动切一下啦。

Q: 我怎么判断模型有没有训练好？

A: 数据集数量正常的情况下（几百条），可以每隔几千步（是总步数不是epoch）跑出来的模型推理听一下，你觉得ok就ok，一般一万步就可以有一个不错的效果了。或者有代码基础的可以用tensorboard查看一下损失率收敛趋势。

Q: 那么问题来了，tensorboard怎么用？

A: python38\Scripts\tensorboard.exe --logdir logs\44k

Q: 我在训练途中按 CTRL+C 暂停训练，继续训练的时候为什么从头开始/步数掉了很多呢？

A: 视频里说的有点歧义，其实是从你上一个保存的模型的进度开始的，比如保存的一个模型是G_8000, 即使你训练到了第8799步，只要下一个模型还没保存，继续训练的时候都是从第8000步开始的。同理，如果一个模型也没保存，那就是从头开始训练。

Q: 如果我在训练中途想要追加一些数据集该怎么办呢？

A: 需要重新预处理并重新训练。

Q: 我为什么没有聚类模型啊？

A: 罚你重看一遍视频。

Q: 训练聚类模型的时候显卡根本没占用是怎么会是呢？

A: 聚类模型训练吃的是你的CPU，看一下python进程在占用CPU就是在训练，等就行了。

Q: 我实在是太懒了，只想让AI帮我读稿子，不想自己录原声再推理，有啥办法吗？

A: 出门右转隔壁 VITS 项目，最近有个 VITS fast fine-tuning 的方法（BV1Jg4y1E7df），几分钟的素材就能练出比较相似的声音，虽然效果没那么好但它实在是太方便了。

Q: 云端训练好的模型怎么在本地用？

A: 下载G模型和对应的config文件，放到本地的对应文件夹就行（.\logs\44k和.\configs）

Q: 我实在不会搞了，请问UP能代训练吗？有偿的那种。

A: 不能。

Q: 我训练和推理都很顺利！现在已经做了一首翻唱了，想上传到B站，有什么注意事项吗？

A: 请勿在视频简介标注项目仓库和整合包作者信息。请标注视频中所使用的输入源和训练集音声来源。 

作者：羽毛布団 https://www.bilibili.com/read/cv22206231/ 出处：bilibili
