+++
showonlyimage = false
draft = false
#image = "img/portfolio/a4-paper.jpg"
date = "2020-01-6T18:25:22+05:30"
title = "potato机器人使用webhook https"
writer = "练法术的兔子"
categories = [ "web"]
weight = 1
+++
### potato机器人webhook 使用https简易教程
#### 步骤
##### 生成自签名证书 使用openssl
- 生成私钥 openssl genrsa -out private.key 2048
- 生成公钥 openssl req -new -x509 -key private.key -out public.crt -days 3650
- 注意生成公钥的时候有选项设置，其他的可以不填，Common Name (e.g. server FQDN or YOUR name) 这一项必须填写自己机器人服务的域名,不支持ip
- 完成后得到 public.crt 和 private.key 密钥对
##### 上传公钥到potato
- 将公钥拖进自己的机器人聊天窗口
- 使用 https://api.sydney.im/<bot_token>/getUpdates 得到公钥的 file_id

##### 机器人编码
- 使用 https://api.sydney.im/<bot_token>/setWebhook 接口设置webhook，参数如下，certificate为公钥的file_id

```
{
    "url": "https://mydomain:port/router",
    "certificate": "00050000321djau921030213"
}
```
- go 语言例子主要代码

```
func IndexHandler(w http.ResponseWriter, r *http.Request) {
	bytes, _ := ioutil.ReadAll(r.Body)
	var updates []bot.Update
	json.Unmarshal(bytes, &updates)
	for _,update := range updates {
		str, _ := json.Marshal(update)
		log.Printf(string(str))
		// TODO your logic
	}
}
http.HandleFunc("/router", IndexHandler)
address := fmt.Sprintf("%s:%d","0.0.0.0",port)
http.ListenAndServeTLS(address, "public.crt", "private.key", nil)
```


