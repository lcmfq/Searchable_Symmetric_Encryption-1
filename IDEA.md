# 状态检测逻辑
1. 如果目录不存在 --> 项目不存在 --> 执行项目创建函数
2. 如果项目目录和子目录plain_text都为空 --> 明文集不存在 --> 创建plain_texts目录，供用户放入明文数据
3. 如果不存在密钥文件 --> 未执行gen()函数，执行gen()函数保存密钥文件
4. 存在明文集和密钥文件，但不存在密文集和加密后的索引文件 --> 未执行enc()函数进行文档和索引的加密操作
5. 如果存在密文集 --> 未上传到服务器 --> 执行upload()函数上传到服务器
6. 哈希对比错误 --> 服务器密文文件损坏 --> 重新构建项目
7. 否则，进入搜索操作

# 目录结构
┌ plain_texts  明文集(0.txt, 1.txt, ...)
├ cipher_texts 密文集(0.enc, 1.enc, ...)
├ xxx(项目名).key 项目密钥
├ xxx(项目名).config 项目配置文件
└ index.enc 加密后的索引文件

# 客户端和服务器如何交互？
1. 用户执行python client.py -p <项目名>，如果项目不存在(状态检测逻辑-1)，创建项目。
2. 执行状态检测逻辑-2到6，判断当前项目的状态，如果全部通过，则进行搜索操作；否则，根据当前项目状态告知用户进行相应的操作
3. 检测逻辑2不通过 --> 告知用户需要将文档放进plain_texts目录中，程序退出；
4. 检测逻辑3不通过 --> 告知用户需要执行gen过程，生成并保存密钥，程序退出；
> python client.py -p <项目名> --gen

5. 检测逻辑4不通过 --> 告知用户是否已经将所有的明文集组织好了，并需要执行enc过程，对索引和文档进行加密操作，随后程序退出；
> python client.py -p <项目名> --enc

6. 检测逻辑5不通过 --> 告知用户是否将加密后的数据上传到服务器上了，需要执行upload过程，程序退出。
> python client.py -p <项目名> --upload

7. 检测逻辑6不通过 --> 告知用户哈希值比较不一致，项目需要重新构建，程序退出.
> python client.py -p <项目名>  --rebuild

8. 执行搜索操作
> python client.py -p <项目名> --search <关键词  	>

# 服务器逻辑
1. 收到客户端的请求，提取项目名和搜索陷门
2. 执行search过程，

# 目前问题
- 怎样读取已有的k值和l值，从而初始化的时候不用指定?