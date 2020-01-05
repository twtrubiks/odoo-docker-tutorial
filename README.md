# odoo-docker-tutorial

利用 docker 快速建立 odoo 環境，

* [Youtube Tutorial - 透過 docker 快速建立 odoo 環境 - 從無到有](https://youtu.be/uqxzq4Td6aU)

## 目錄

1. [odoo 簡介](https://github.com/twtrubiks/odoo-docker-tutorial#%E7%B0%A1%E4%BB%8B) - [Youtube Tutorial - 透過 docker 快速建立 odoo 環境 - 從無到有](https://youtu.be/uqxzq4Td6aU)

2. [利用 docker 快速建立 odoo 環境](https://github.com/twtrubiks/odoo-docker-tutorial#%E6%95%99%E5%AD%B8) - [Youtube Tutorial - 透過 docker 快速建立 odoo 環境 - 從無到有](https://youtu.be/uqxzq4Td6aU)

3. [odoo12 如何開啟 odoo developer mode](https://github.com/twtrubiks/odoo-docker-tutorial#odoo12-%E5%A6%82%E4%BD%95%E9%96%8B%E5%95%9F-odoo-developer-mode) - [Youtube Tutorial - odoo12 如何開啟 odoo developer mode](https://youtu.be/fUtqWQHbt1I)

4. [如何安裝第三方 addons](https://github.com/twtrubiks/odoo-docker-tutorial#%E5%A6%82%E4%BD%95%E5%AE%89%E8%A3%9D%E7%AC%AC%E4%B8%89%E6%96%B9-addons) - [Youtube Tutorial - odoo 如何安裝 addons](https://youtu.be/aN35xlUBKwc)

5. [如何進入 odoo Database 管理界面](https://github.com/twtrubiks/odoo-docker-tutorial#%E5%A6%82%E4%BD%95%E9%80%B2%E5%85%A5-odoo-database-%E7%AE%A1%E7%90%86%E7%95%8C%E9%9D%A2) - [Youtube Tutorial - 如何進入 odoo Database 管理界面](https://youtu.be/JIRcz1WDLT0)

6. [如何使用 pgadmin4 連接 odoo](https://github.com/twtrubiks/odoo-docker-tutorial#%E5%A6%82%E4%BD%95%E4%BD%BF%E7%94%A8-pgadmin4-%E9%80%A3%E6%8E%A5-odoo) - [Youtube Tutorial - 如何使用 pgadmin4 連接 odoo](https://youtu.be/afuB8wnozo8)

## 簡介

什麼是 Odoo，他可以吃嗎 :question:

[Odoo](https://www.odoo.com/) 是一套 ERP 系統，使用 Python 撰寫。

Odoo 有兩種版本，分別是

免費社群版 [Odoo-Community](https://www.odoo.com/page/community)，和收費企業版 Odoo-Enterprise。

Odoo-Community 以及 Odoo-Enterprise 的功能差異可以看 [HERE](https://www.odoo.com/page/editions)。

至於為什麼要有 Odoo-Enterprise，原因很簡單，就是要養工程師:smile:

如果大家有興趣，可以去官網看一下 Odoo 的文章，有說會這樣做是因為公司差點 GG。

Odoo Community 的 source code 都在 github 上，可以點這裡 [odoo](https://github.com/odoo/odoo)，

其實如果你看他的 branch，你會發現他有很多版本，Odoo8，Odoo9，Odoo10，

Odoo11，最新的 Odoo12，以及快要推出的 Odoo13。

先簡單介紹到這邊就好，讓我們先來看看什麼是 Odoo:satisfied:

## 教學

這邊為了快速建立環境，將使用 docker 來建立，

但是如果你想要開發 addons，我會建議使用 source code 的方式來建立 Odoo，

這部分如果大家有興趣有機會我再拍影片教大家:smirk:

如果你不懂 docker，可參考我之前寫的 [Docker 基本教學 - 從無到有 Docker-Beginners-Guide](https://github.com/twtrubiks/docker-tutorial)。

Odoo 的 docker images 我使用 [odoo docker](https://hub.docker.com/_/odoo)，

先來看 docker-compose.yml

```yml
version: '3.5'
services:
  web:
    image: odoo:12.0
    depends_on:
      - db
    ports:
      - "8069:8069"
    volumes:
      - odoo-web-data:/var/lib/odoo
      - ./config:/etc/odoo
      - ./addons:/mnt/extra-addons
    # command:
    #   odoo -r odoo -w odoo -i addons -d odoo
  db:
    image: postgres:10.9
    # ports:
    #   - "5432:5432"
    environment:
      - POSTGRES_DB=postgres
      - POSTGRES_USER=odoo
      - POSTGRES_PASSWORD=odoo
      - PGDATA=/var/lib/postgresql/data/pgdata
    volumes:
      - odoo-db-data:/var/lib/postgresql/data/pgdata

volumes:
  odoo-web-data:
  odoo-db-data:
```

主要有兩個 services，

先來看 web，這個主要就是 Odoo，這邊我們使用 `odoo:12.0`，然後這邊順便提一下，

Odoo 每天都會在 [Odoo Nightly builds](http://nightly.odoo.com) build，

你可以到 [odoo-docker-github](https://github.com/odoo/docker) 這邊去看這個 image 是用幾月幾號的 Odoo Source code build 的，

如果你覺得太舊了，你也可以利用他的 Dockerfile 去 build 更新的 Odoo，這部分一樣有機會再教大家:smirk:

volumes 中的 `odoo-web-data:/var/lib/odoo` 是儲存 Odoo 中的資料。

volumes 中的 `./config:/etc/odoo` 則是 Odoo 的 config 設定 ( 等等會說明 )。

volumes 中的 `./addons:/mnt/extra-addons` 則是 Odoo 的第三方 addons，

我在 [addons](https://github.com/twtrubiks/odoo-docker-tutorial/tree/master/addons) 裡面放了 manay-addons-folder 檔案，代表說這邊就是要放 addons。

( 因為 github 不能傳空的資料夾 :wink:)

這邊我說明一下，其實 Odoo 是由很多 addons 組成的，

可以看這邊 [https://github.com/odoo/odoo/tree/12.0/addons](https://github.com/odoo/odoo/tree/12.0/addons)，

一個資料夾就是一個 addons，當然，既有的 addons 可能無法滿足需求，

這時候就會有客製化以及第三方 addons 的存在，

這時候你可能會問，除了原生的 addons，哪裡有第三方 addons 呢:question:

我提供幾個地方給大家參考，主要有兩個，首先是 [OCA](https://github.com/OCA)，裡面超多 addons，

再來是 [Odoo Apps](https://www.odoo.com/apps/modules)，裡面有免費和付費的。

這篇文章不會和大家說要如何自己撰寫一個 addons，下次有機會會再拍影片:smirk:

command 中的 `odoo -r odoo -w odoo -i addons -d odoo`，這段我是註解掉的，

原因是不需要，什麼時候你會需要他 :question:

當你需要透過 command line 安裝或更新 addons 的時候就可以這樣寫，

odoo 中的 command line 說明可參考 [Command-line interface:odoo-bin](https://www.odoo.com/documentation/12.0/reference/cmdline.html)，

( 一般的情況是要使用 odoo-bin，不過在 docker 中使用 odoo 即可 )

回頭再來看 [odoo.conf](https://github.com/twtrubiks/odoo-docker-tutorial/tree/master/config)

```conf
[options]
addons_path = /mnt/extra-addons
data_dir = /var/lib/odoo
```

`addons_path` 就是 addons 的位置，通常都會有很多，使用逗號隔開即可。

`data_dir` 保存 odoo 資料夾。

這裡面其實還有超級多的參數設定 ( 暫時跳過 ) :smirk:

再來看 db，

Odoo 是使用 postgres，ports 的部分我是註解掉的，但這樣本機就無法訪問他，

( 為什麼 ports 註解掉本機就無法連接，這邊如果你不太了解，

可參考 [docker-compose ports 和 expose 差異](https://github.com/twtrubiks/docker-tutorial#docker-compose-ports-%E5%92%8C-expose-%E5%B7%AE%E7%95%B0) )

如果你想要用 pgadmin4 之類的軟體連接到 postgres，請將註解取消，這樣你才可以連到資料庫

( pgadmin4 是一套免費的工具，連接 postgres，可參考 [利用 docker 快速建立 pgadmin4](https://github.com/twtrubiks/docker-pgadmin4-tutorial) )

## 執行 odoo

```cmd
docker-compose up
```

然後可以瀏覽 [http://localhost:8069](http://localhost:8069)，

![alt tag](https://i.imgur.com/fpmc67U.png)

你應該會看到下圖，

![alt tag](https://i.imgur.com/6FS6NPw.png)

設定完之後等耐心等候，

![alt tag](https://i.imgur.com/BJFU12e.png)

## odoo12 如何開啟 odoo developer mode

* [Youtube Tutorial - odoo12 如何開啟 odoo developer mode](https://youtu.be/fUtqWQHbt1I)

在 odoo 中, 開啟 developer mode 是很重要的:thumbsup:

他可以幫助你很多事情, 那要如何開啟呢:question:

點選 Settings

![alt tag](https://i.imgur.com/qiTHdhI.png)

你會發現右邊有兩個選項, 分別是,

Activate the developer mode -> 通常我都選這個。

Activate the developer mode (with assets) -> 這個事前端 debug 用的。

![alt tag](https://i.imgur.com/Cs5j6Vj.png)

點選下去之後, 你會發現網址多了 `?debug` 的字,

![alt tag](https://i.imgur.com/jadcuLD.png)

其實你後來會發現, 直接將網址的 web 字串後加上 `?debug` 就可以進入 debug 了。

如果你還是很懶, 也有瀏覽器擴充可以幫忙,

像是 Odoo Debug - Google Chrome 之類的 , Firefox 也有。

## 如何安裝第三方 addons

* [Youtube Tutorial - odoo 如何安裝 addons](https://youtu.be/aN35xlUBKwc)

使用 [Row Number in tree/list view](https://apps.odoo.com/apps/modules/12.0/rowno_in_tree/) 這個 addons 做測試,

請記得選對版本, 這邊選擇 odoo12, 下載後, 請解壓縮

![alt tag](https://i.imgur.com/MQDJZGQ.png)

接著放入 addons 資料夾

![alt tag](https://i.imgur.com/bN0J7Ho.png)

我會建議這兩步驟也要操作,

將 addons 資料夾給最大權限 ( Linux 用戶 )

```cmd
sudo chmod -R 777 addons
```

如果對 Linux 指令不熟 , 可參考 [紀錄一些 linux 的指令📝](https://github.com/twtrubiks/linux-note)

![alt tag](https://i.imgur.com/9pGcFjy.png)

然後重起 server ( 這步驟很重要, 有時候沒重起真的會遇到很怪的問題:joy: )

![alt tag](https://i.imgur.com/glw83qW.png)

記得開 debug, 點選 Update Apps List

![alt tag](https://i.imgur.com/TBdGFh3.png)

找到 addons, addons 的名稱就是資料夾的名稱

![alt tag](https://i.imgur.com/HftVAvZ.png)

點選 Install 即可。

## 如何進入 odoo Database 管理界面

* [Youtube Tutorial - 如何進入 odoo Database 管理界面](https://youtu.be/JIRcz1WDLT0)

odoo 的登入頁面, 下方會有個 Manage Database，

![alt tag](https://i.imgur.com/bFLtf4N.png)

點下去你會看到下圖，

![alt tag](https://i.imgur.com/yr7fTG1.png)

網址是 [http://0.0.0.0:8069/web/database/manager](http://0.0.0.0:8069/web/database/manager)，你也可以直接輸入網址進入，

在這邊可以管理 odoo db，包含建立, 刪除, 還原, 備份。

![alt tag](https://i.imgur.com/Z4OSyLG.png)

有注意到 Set Master Password 嗎:question:

(可參考 [Master Password](https://www.pgadmin.org/docs/pgadmin4/development/master_password.html))

他其實是算多一層保護:relieved:

當我們如果設定他之後， 在頁面上做任何操作，

都會需要密碼。

這個 Set Master Password 也可以在 `odoo.conf` 中設定

```conf
[options]
addons_path = /mnt/extra-addons
data_dir = /var/lib/odoo

admin_passwd = 666666
; list_db = False
```

(`;` 代表註解)

重起後就會生效。

舉個例子，如果我要刪除 db，他會要你輸入你的 admin_passwd，

![alt tag](https://i.imgur.com/sCSN9f9.png)

你可能會問我, 這樣任何 user 都可以進去, 不會很危險嗎:question:

（當然危險:joy:）

所以這個頁面可以關閉 (將 `list_db` 設為 `False` )

```conf
[options]
addons_path = /mnt/extra-addons
data_dir = /var/lib/odoo

admin_passwd = 666666
list_db = False
```

這樣就無法進去這個頁面了，

![alt tag](https://i.imgur.com/ydnI8V8.png)

但這樣連 admin 也無法進去這頁面了:scream:

所以比較好的方法是搭配 Nginx 把 url block 掉:smile:

( 設定只有特定 ip 可以訪問頁面 )

## 如何使用 pgadmin4 連接 odoo

* [Youtube Tutorial - 如何使用 pgadmin4 連接 odoo](https://youtu.be/afuB8wnozo8)

如果不知道什麼是 pgadmin4 以及安裝方法, 可參考我之前寫的

[利用 docker 快速建立 pgadmin4 以及 Ubuntu 本機如何安裝 pgadmin4](https://github.com/twtrubiks/docker-pgadmin4-tutorial)

記得把 db 的 port 打開

![alt tag](https://i.imgur.com/3jRmI1M.png)

輸入連線資訊 ( 如果你使用我的範例, 都會是 odoo )

![alt tag](https://i.imgur.com/vGJRvAS.png)

連進去你會看到很多 db , 像我這邊有建立了 odoo 以及 odoo1

![alt tag](https://i.imgur.com/ySsgUCG.png)

找到 table

![alt tag](https://i.imgur.com/f0CHRoG.png)

使用 expense 來測試, 修改 name 欄位

![alt tag](https://i.imgur.com/0WeUDKH.png)

頁面上的資料也跟著改變

![alt tag](https://i.imgur.com/jUD4XAM.png)

## 後記

這次的 Odoo 介紹是很基礎的帶大家稍微了解一下，還有非常多東西可以講，像是如何撰寫 addons，

如何擴充既有的 addons，Odoo 架構，Odoo 如何搭配 Nginx 等等......

幾乎都可以單獨拍一支影片和大家做介紹。

## 延伸閱讀

[如何建立 odoo 開發環境 - odoo13 - 從無到有](https://github.com/twtrubiks/odoo-development-environment-tutorial)

## Reference

* [Odoo](https://www.odoo.com/)
* [odoo docker](https://hub.docker.com/_/odoo)

## Donation

文章都是我自己研究內化後原創，如果有幫助到您，也想鼓勵我的話，歡迎請我喝一杯咖啡:laughing:

綠界科技ECPAY ( 不需註冊會員 )

![alt tag](https://payment.ecpay.com.tw/Upload/QRCode/201906/QRCode_672351b8-5ab3-42dd-9c7c-c24c3e6a10a0.png)

[贊助者付款](http://bit.ly/2F7Jrha)

歐付寶 ( 需註冊會員 )

![alt tag](https://i.imgur.com/LRct9xa.png)

[贊助者付款](https://payment.opay.tw/Broadcaster/Donate/9E47FDEF85ABE383A0F5FC6A218606F8)

## 贊助名單

[贊助名單](https://github.com/twtrubiks/Thank-you-for-donate)

## License

MIT license