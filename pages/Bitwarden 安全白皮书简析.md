- 人的记忆能力是有限的，我从 2016 年开始用密码管理相关的工具，到现在已经有超过 380+ 的密码了，如果每个网站或软件的密码都是不一样，我想应该没有人可以用脑袋记下来吧。
- > Bitwarden 是一款自由且开源的密码管理服务，用户可在加密的保管库中存储敏感信息（例如网站登录凭据）。Bitwarden 平台提供有多种客户端应用程序，包括网页用户界面、桌面应用，浏览器扩展、移动应用以及命令行界面。Bitwarden 提供云端托管服务，并支持自行部署解决方案。 -- 维基百科
- 如果不使用密码管理软件，我想大部分人的网站密码都是那一个特定的一串字符，然后在此基础上做一些修改，有些网站需要大写那就首字母大写，需要数字那后面就补个 `123`，需要特殊字符 那就再补个 `.`, `$`, `#`。 eg：`example`, `Example`, `example123`, `example123.`。
- 以上的密码就特别容易被攻击，举个栗子：如果一个网站被黑客攻击且你也是这个网站的用户，而网站又没有对密码做混淆处理，黑客将数据公布到了互联网之中，可能会被一些有心人利用，用此密码在其他网站上进行登陆。
- ## Bitwarden 的核心概念
- 主密码（Master Password）
  id:: 62284063-13f8-4a4b-bbd0-4128d90b9a80
	- 用户唯一需要记住的密码，也是不可忘记的密码（一旦忘记会导致所有的密码丢失），泄漏将导致所有密码都变得不安全，所以此密码只记录与自己的大脑中。
- 主密钥（Master Key）
  id:: 622840fb-f301-43db-adc6-e9bff63019a1
	- 通过 [[PBKDF2]] 算法对主密码进行哈希，得到一个固定长度的数据。
- 扩展主密钥（Stretched Master Key）
  id:: 622849ad-2b8c-495b-bd6e-b303850d4c0f
	- 通过 [[HKDF]] 算法对主密钥进行拉伸，进一步加强密钥的安全性。
- 对称密钥（Symmetric Key）
	- 用于加密所有用户数据的密钥。
- 非对称密钥（RSA Key Pair）
	- 用于对组织对称密钥加密。
- ## 用户创建
- 步骤一：浏览器会使用 [[PBKDF2]] 算法对用户的 [主密码](((62284063-13f8-4a4b-bbd0-4128d90b9a80))) + 邮箱进行 100000 次的哈希迭代，得到 [主密钥](((622840fb-f301-43db-adc6-e9bff63019a1)))。
	- 邮箱：yaku.mioto@gmail.com
	- [主密码](((62284063-13f8-4a4b-bbd0-4128d90b9a80))) ：s9qn#UhhaDir5V2B
	- [主密钥](((622840fb-f301-43db-adc6-e9bff63019a1))) ：VTEkR02r8xcCrJLRrJmbx78Vqp5mjH9tAM3YDpIzmsA=
- 步骤二：浏览器会使用 [[HKDF]] 算法对用户的 [主密钥](((622840fb-f301-43db-adc6-e9bff63019a1))) 进行扩展，得到 [扩展主密钥](((622849ad-2b8c-495b-bd6e-b303850d4c0f)))。
	- [主密钥](((622840fb-f301-43db-adc6-e9bff63019a1))) ：VTEkR02r8xcCrJLRrJmbx78Vqp5mjH9tAM3YDpIzmsA=
		- Encryption Key：2YmTGyHlY5856okt3xnUbe2ogCEl6F2rZxKjReV7noY=
		- MAC Key：4Cx22FeKfTSOZ+6Mnr9Una36rJheGxFqI5Bw1evTuc8=
	- [扩展主密钥](((622849ad-2b8c-495b-bd6e-b303850d4c0f)))：2YmTGyHlY5856okt3xnUbe2ogCEl6F2rZxKjReV7nobgLHbYV4p9NI5n7oyev1SdrfqsmF4bEWojkHDV69O5zw==
- 步骤三：浏览器会调用系统的密码安全伪随机生成函数 [[CSPRNG]] 生成一个长度为 256 位的