# 紀錄Docker學習紀錄

- 容器化技術，部屬歷史，硬體主機>作業系統>多個Application>當某個App出錯，他會影響其他App>為了解決這個問題，用多台硬體主機解決>後來出現VM(虛擬主機)，一台硬體，會有一個作業系統與一個Hypervisor，在這一層可以建立多個VM，在其中在建立多個OS與App
- Container模式，底層是硬體與OS在來上一層有一個Docker Engine在上面再建立容器與App。用軟體就創造出的虛擬空間。 資源耗費最低。
![image](https://github.com/Tomalison/Docker/assets/96727036/62392d0d-8fd4-41d9-b00a-c80e00cddca4)

- 透過Docker image在各個作業系統使用。Mac、Linux、Windows。
Diocker hub雲端空間提供其他人下載Docker image。(CentOS 作業系統下來)

## Docker三大功用

1. 原本的作業方式是，先拿到程式碼，安裝所需環境與安裝指令跑起來，透過Docekr可以佈屬一個程式部屬包。簡化部屬
2. 跨平台部屬，用Docker hub。先安裝Docker Engine環境就可下載下來。
3. 建立乾淨測式環境，同樣概念可以用成資料庫安裝(測試資料、資料庫安裝/安裝指令)
![image](https://github.com/Tomalison/Docker/assets/96727036/010cff20-48d5-46f6-a17e-ac0736e11953)

- Linux可直接使用外，Windows 跟 MacOS都要使用 Virtual BOX 或OS Native使用

![image](https://github.com/Tomalison/Docker/assets/96727036/0f8a0382-c606-4cc1-8872-60e7b213661f)

- 先到官網找到Docker Toolbox 找到 toolbox realeses 下載dockertoolbox-24.0.2.exe，持續下一步安裝。
- Docker Desktop安裝方式，Windows10以上才可以安裝，安裝完後 用CMD 打上指令wsl --update 安裝Linux環境(linux os)
![image](https://github.com/Tomalison/Docker/assets/96727036/e0708be9-c106-466a-a750-cc97d21a68a4)
- 開啟docker desktop並登入，docker pull hello-world / docker run hello-world / docker images
![image](https://github.com/Tomalison/Docker/assets/96727036/923c301a-d972-40c1-a734-7fe61da7c9ee)

![image](https://github.com/Tomalison/Docker/assets/96727036/7491dc34-a940-4f05-b96a-5faea22efb49)

## Docker使用情境 

- 先到Docker Hub 進到 Repositories 
- 然後再命令提示字元 用docker pull ___ 抓你要的
![image](https://github.com/Tomalison/Docker/assets/96727036/acc3007e-8b02-4e56-8f2c-4ea3cfb3f9c2)
- 再來用docker container run hello-world
#### TAG
- docker pull 專案名稱:latest 抓最新的版本來用
- docker pull 專案名稱:v4 抓指定的版本

## 建立與使用Docker image

- mkdir dockertest001
- cd dockertest001/ 進到這個目錄
- pwd 驗證一下目錄位置

#### 將範例放到剛剛的dockertest001資料夾裡面，並將檔案名稱改為Dockerfile
- docker build -t tomsu25478/course-image-build002 .
- docker build -t tomsu25478/course-image-build002 --build-arg my_name_is="Tom Su" .
- docker container run tomsu25478/course-image-build002 .
- cat: read error: Is a directory
- You have built a new image from a Dockerfile. Well done, Tom Su!

#### 上傳到Docker Hub
- 先docker login (如果先登入 就先登出docker logout) 帳號密碼(yours)
- docker push 帳號/專案名稱

#### 如何清理乾淨不需要的images
- 先打上 docker rmi 帳號/專案名稱
- docker rm container的代號
- 這樣就可以再用 docker rmi 清掉
![image](https://github.com/Tomalison/Docker/assets/96727036/e1016ac0-ed99-42b8-9620-deca2ce646aa)

#### 如果要強制清理
- docker rmi -f 帳號/專案名稱

## 建立與使用Docker container
- docker container ls 查詢目前有哪些container
- docker pull alpine
- docker container run --name c001 alpine ls /  這邊可以將container縮寫

- 當程序不要一直跑得話 可以用clear清掉
- docker run -it 可以讓我們需要跑的一個程序 一直跑下去 --name 可以取名
- docker run -it --name c003 alpine /bin/sh <--進到該程序需要的東西 再輸入exit 就可以離開這個程序離開這個container
![image](https://github.com/Tomalison/Docker/assets/96727036/bc8e1690-e660-4c82-87c0-4b6a6e830647)

- docker run -d (daemon 長期在背景跑)
- tail -f 追蹤某個file的log 並把他印到console上 還有tail解決副我們通常用這個file
- docker run -d --name c004 alpine tail -f /dev/null
- 在按docker container ls 可以看到我們這次終於有一個container一直在背景跑 他跑的就是剛剛指令中的tail -f /dev/null
![image](https://github.com/Tomalison/Docker/assets/96727036/c987e65c-0707-4917-be92-31d4712723fa)
- 還想在這個container加點東西，則打上docker exec -it 再加上我們的container ID +兩個要執行的程序 /bin/sh
![image](https://github.com/Tomalison/Docker/assets/96727036/a7a8cf43-1291-4fcb-95c3-4fa304771336)


![image](https://github.com/Tomalison/Docker/assets/96727036/adcbf12b-553b-425b-97ed-49cde205d116)
- 要將container清空之前要先將之停下來  docker container stop  ContainerID
- 先用docker container ls -a查出所有的container
- 將你要清掉的打上docker container rm ContainerID

## 建立與使用Dockerfile

- 我們透過Docker Client執行指令 而這些指令都會送到Docker Engine上面去(這是兩個工作者) 這整個過程中有三個空間 第一個是在硬體主機的OS空間 第二個是在虛擬主機建起來的LinuxOS空間
- 第三個是Docker Engine去建起來的臨時Container空間。
-
![image](https://github.com/Tomalison/Docker/assets/96727036/27b48fb2-db18-453e-b27a-c064b9f517cf)
![image](https://github.com/Tomalison/Docker/assets/96727036/b1ea9684-bce3-4844-a77a-ef986a8bdbb6)
![image](https://github.com/Tomalison/Docker/assets/96727036/ab4427de-9c94-45e8-8366-b20dd4aa20e0)
![image](https://github.com/Tomalison/Docker/assets/96727036/55262637-d76a-4b6b-b199-47c0a5005b12)
![image](https://github.com/Tomalison/Docker/assets/96727036/2994c895-faf7-4a44-8a7c-4813b51409aa)

#### Dockerfile情境語法介紹FROM

- 首先打上pwd到你想要到的目錄位置>再來打上touch Dockerfile建立空白file的指令>在目錄中打開這個file(用你熟悉的編輯器)
- 打上 From alpine:latest (內含輕量的LinuxOS) <cat Dockerfile可以印出檔案內容
- 有了Dockerfile之後就可以建立image >打上docker build -t uopsdod/001 取名為001
- 讓他在背景跑 docker run -d uopsdod/001 tail -f /dev/null
![image](https://github.com/Tomalison/Docker/assets/96727036/90c99d8a-cf26-4cca-8551-f472f0949ccf)
![image](https://github.com/Tomalison/Docker/assets/96727036/8f3f1967-34f9-4919-a192-7c743eb61489)
![image](https://github.com/Tomalison/Docker/assets/96727036/41468d1e-e69f-4375-86df-e2fd2afc7a17)

![image](https://github.com/Tomalison/Docker/assets/96727036/100a9721-2f42-4bd1-9e0c-c9e554c1853b)
- 因為已經有container有在背景執行所以要跟她互動 要打上他的containerID :  docker exec -it ContainerID /bin/sh (如上圖)
![image](https://github.com/Tomalison/Docker/assets/96727036/6d5ee2a5-9f48-40fd-abf7-c55eb4a98c95)

- 而剛剛我們在使用的語法都是由 一開始的Dockerfile內的那段語法 所提供的

#### Dockerfile情境語法Entrypoint

- 用在要讓別人使用這個image 但又不用打上一些我一開始設定的內容，這時候要打上entrypoint 該語法後面先用一個["tail", "-f", "/dev/null"]框框 (呈上題
- 打好了將檔案儲存
- 然後我們打上docker build -t uopsdod/002 . 這個時候就可以看到執行變成兩步驟(如下圖)
![image](https://github.com/Tomalison/Docker/assets/96727036/2b2f0631-d0f6-4176-afc7-5215a0e9c4cb)
![image](https://github.com/Tomalison/Docker/assets/96727036/6a126b37-0ece-4610-9c8f-c10b46f16a92)

#### Dockerfile情境語法Run

- 呈上 我們想要進去建立一個apache server
- docker run -d -p 8080:80  這邊port的mapping做好之後 再打上 image名稱  uopsdod/002
![image](https://github.com/Tomalison/Docker/assets/96727036/51fb482d-368b-4fb9-9728-4c11732efb02)
- docker exec -it ContainerID /bin/sh
![image](https://github.com/Tomalison/Docker/assets/96727036/b324036f-e950-4111-adca-7c77c80c359d)
- 呈上圖啟動apache server 打上httpd -D FOREGROUND
![image](https://github.com/Tomalison/Docker/assets/96727036/90598d41-1bb0-4731-8790-468451772095)

- 接下來要找LINUX VM的位置 首先要知道她的IP位置 打上echo $(docker-machine ip)
![image](https://github.com/Tomalison/Docker/assets/96727036/46770773-5beb-49a8-8c7d-b9cb3b5c2586)

- 以上的安裝過程我們想要更方便，就要用到指令run
``` sh
From alpine:latest
RUN echo "I'm in the image building process..."
RUN ls -l / (這是列出所有根目錄下的檔案跟目錄
ENTRYPOINT ["tail", "-f", "/dev/null"]
```
``` sh
From alpine:latest
RUN apk --update add apaches
RUN rm -rf /var/cache/apk/*
ENTRYPOINT ["httpd", "-D", "FOREGROUND"]
```
![image](https://github.com/Tomalison/Docker/assets/96727036/6ad3a94c-8e8a-4cd1-9005-e81a918bf11f)
- 最後用 docker run -d -p 8081:80 uopsdod/004 (給LinuxVM一個port對應到aphache的80 port)
![image](https://github.com/Tomalison/Docker/assets/96727036/1cbb3e18-a2f4-4eab-92ed-15d4c304e826)

#### Dockerfile情境語法ENV
``` sh
From alpine:latest
MAINTAINER MyName
RUN apk --update add apaches2
RUN rm -rf /var/cache/apk/*
RUN cd /var/www/localhost/htdpcs && pwd
ENTRYPOINT ["httpd"]
CMD ["-D", "FOREGROUND"]
```
- RUN之間是獨立的
- 在一個RUN裡面是一個臨時性的Container
- 那如果要在同一個RUN在不同行執行，那要在指令中加上\
- RUN cd /var/www/localhost/htdpcs \ 
    && pwd
- 接下來要對 首頁index.html做事，我們將pwd改成echo
``` sh
RUN cd /var/www/localhost/htdpcs \ 
    && echo "<h3>I am Tom Round 01<h3>" >> index.html
RUN cd /var/www/localhost/htdpcs \ 
    && echo "<h3>I am Tom Round 02<h3>" >> index.html
RUN cd /var/www/localhost/htdpcs \ 
    && echo "<h3>I am Tom Round 03<h3>" >> index.html
```
- 同樣的行為做三次，希望在首頁多出這幾行
- docker build-t uopsdod/005 .
- docker images
- docker run -d -p 8080:80 uopsdod/005
- docker container ls
- echo $(docker-machine ip)
- 將IP跟埠號貼到網頁上，在網頁上就可以看到剛剛加的echo內容

- 但如果想要將這些相同的路徑放在一起則用
``` sh
ENV myworkdir /var/www/localhost/htdocs
RUN cd ${myworkdir} \ 
    && echo "<h3>I am Tom Round 01<h3>" >> index.html
RUN cd ${myworkdir} \ 
    && echo "<h3>I am Tom Round 02<h3>" >> index.html
RUN cd ${myworkdir} \ 
    && echo "<h3>I am Tom Round 03<h3>" >> index.html
```
  這就是ENV的功用

#### Dockerfile情境式語法介紹 Workdir

![image](https://github.com/Tomalison/Docker/assets/96727036/48e8ebac-6d46-4aa8-9b8d-35195bfb1c9d)
- 在剛剛的範例中，我們現在加上一個workdir
``` sh
From alpine:latest
ENV myworkspace /var/www/localhost/htdocs
RUN apk --update add apaches2
RUN rm -rf /var/cache/apk/*
RUN cd ${myworkspace} \ 
    && echo "<h3>I am Tom Round 01<h3>" >> index.html
RUN cd ${myworkspace} \ 
    && echo "<h3>I am Tom Round 02<h3>" >> index.html
RUN cd ${myworkspace} \ 
    && echo "<h3>I am Tom Round 03<h3>" >> index.html
ENTRYPOINT ["httpd", "-D", "FOREGROUND"]
```
- 如果我們想要把這個重複的CD過程給取代掉
- 我們可以將上面的程式改成
``` sh
From alpine:latest
ENV myworkspace /var/www/localhost/htdocs
WORKDIR ${myworkspace}  
RUN apk --update add apaches2
RUN rm -rf /var/cache/apk/*
RUN echo "<h3>I am Tom Round 01<h3>" >> index.html
RUN echo "<h3>I am Tom Round 02<h3>" >> index.html
RUN echo "<h3>I am Tom Round 03<h3>" >> index.html
ENTRYPOINT ["httpd", "-D", "FOREGROUND"]
```
- 直接將預設目錄指到這個myworkspace
- 後面的RUN就用每一個路徑都在打一次了

#### Dockerfile情境式語法介紹 ARG (argument)
- arg可以讓我們在docker build的時候去改變這個變數
- 呈上範例，想將重複詞句
``` sh
From alpine:latest
ENV myworkspace /var/www/localhost/htdocs
WORKDIR ${myworkspace}  
ARG whoami=Tom
RUN apk --update add apaches2
RUN rm -rf /var/cache/apk/*
RUN echo "<h3>I am ${whoami} Round 01<h3>" >> index.html
RUN echo "<h3>I am ${whoami} Round 02<h3>" >> index.html
RUN echo "<h3>I am ${whoami} Round 03<h3>" >> index.html
ENTRYPOINT ["httpd", "-D", "FOREGROUND"]
```
![image](https://github.com/Tomalison/Docker/assets/96727036/6f036675-d0e4-4a39-8ff4-1b919ee7e83d)
- 接下來想改變
- 用同一個dockerfile建造不童的docker image
- docker build --build-arg whoami=Tony -t uopsdod/007 .
- clear
- docker images
- docker run -d -p 8089:80 uopsdod/007
- docker container ls
- echo $(docker-machine ip)
- 接下來在網頁上就可以看到以下圖
![image](https://github.com/Tomalison/Docker/assets/96727036/988c4808-7c08-4c62-beb6-45b5d029235c)


#### Dockerfile情境式語法介紹COPY

- 課程提供的Content.txt檔先放到跟Dockerfile同個資料夾
![image](https://github.com/Tomalison/Docker/assets/96727036/4c14b18f-69d2-49be-ba19-18a93fca5f54)
- Terminal中打上ls就可以看到這兩個檔案
- 我現在想要將我剛剛的content的book list資訊放到index.html裡面
- 就可以在首頁看到book list
``` sh
Copy ./content.txt ./
RUN ls -l ./
RUN cat ./content.txt ./ >> index.html
```
- 現在當前目錄是Linux VM空間docker daemon會把當前的目錄下的content.txt檔案複製一個到container空間的當前目錄下
- 而目前的當前目錄就是剛剛在Dockerfile定義的workdir這裡
![image](https://github.com/Tomalison/Docker/assets/96727036/de015053-f3d0-4f87-b330-761142de663a)
![image](https://github.com/Tomalison/Docker/assets/96727036/71037951-3330-4535-9bca-3096d2af6a5b)

- 將任意Container變成Docker Image
![image](https://github.com/Tomalison/Docker/assets/96727036/0cd62f82-afbd-4957-bee4-a13340b6d716)

## 如何把Container轉換成Image
- 首先打上ls 
- cat Dockerfile
- docker build -t uopsdod/myimage .
- docker images
- docker run -d -p 8080:80 uopsdod/myimage
- echo $(docker-machine ip)

- docker container ls
- docker exec -it ContainerID /bin/sh
![image](https://github.com/Tomalison/Docker/assets/96727036/1a8ff995-13ea-465a-833c-b6ac1f121fe4)
- ls
- cat index.html
- echo "I am going to turn this container into an new image" >> index.html
- 就可以看到首頁篩入資訊
- 接下來先離開這個Container > exit
- 將這個運行中的Container轉換成Image
- 複製ContainerID
- 先打上docker commit ContainerID uopsdod/containertoimage
![image](https://github.com/Tomalison/Docker/assets/96727036/ce363660-99fd-49e5-9a64-03e5e80e3d5f)

- docker container ls
- 先停止與清理Container
- docker stop ContainerID
- docker rm ContainerID
- clear

- docker images
- docker run -d -p 8080:80 uopsdod/containertoimage
- echo $(docker-machine ip)
![image](https://github.com/Tomalison/Docker/assets/96727036/91847e24-25d9-4306-8d6a-82661d12c270)

## Docker網路模式介紹
- none/ bridge / container / host
![image](https://github.com/Tomalison/Docker/assets/96727036/afa332a6-064d-4868-a48c-aa9846181d4a)
#### Bridge
- IP其實是一個有限的座位，容器要跟Network要IP來使用，如果隸屬不同的Bridge，那兩個容器是無法互通的。屬於同一個網路空間就可以彼此溝通
#### Container
- 例如容器3對應到容器02-1，那就會拿到同一個標籤，同一個IP。
#### host
- 例如容器4會跟Linux VM Network要一個IP，那他就可以跟Linux主機其他程式互通網路。

## Docker網路介紹None模式
- 先將一個base image抓下來，bocker pull alpine
- docker network ls
- docker run -d --network none --name none-mode alpine tail -f /dev/null
- docker container ls
- docker network inspect none
- 透過這個指令我們就能明確知道我們已經將這個container放到這個none網路中
- 驗證 docker exec -it ContainerID /bin/sh
- id addr ls
- 可以看到只能連自己 127.0.0.1
![image](https://github.com/Tomalison/Docker/assets/96727036/3c52107c-add5-4891-acb0-fa54173cdb4f)

## Docker網路介紹Bridge模式

- docker network ls 查看
- docker network create --driver bridge my-bridge
- 創建一個自己的bridge
- docker network inspect my-bridge
![image](https://github.com/Tomalison/Docker/assets/96727036/1bb61535-1165-44a2-b8b4-ffb74e2d6f23)

- 可以看到Subnet的網路空間，是我們my-bridge的網路空間
- 建立一個新的Container 然後放到我們my-bridge的網路空間
- docker run -d --network my-bridge --name bridge-mode001 alpine tail -f /dev/null
- docker container ls
- docker network inspect my-bridge
![image](https://github.com/Tomalison/Docker/assets/96727036/63443865-4c5d-4d73-a518-d99e665c9450)

- 先複製Containers裡的ID
- docker exec -it ContainerID /bin/sh
- ip addr ls 也可以看到剛剛my-bridge裡配了一組IP來給這個Container使用(如下圖
![image](https://github.com/Tomalison/Docker/assets/96727036/3ab4a670-dd6e-4f58-96e3-b198308051de)
- 用ping 8.8.8.8(google)
- 可以看到可以連到外網
- exit>>clear
- 我們要再來建造一個Container放到同樣是my-bridge的網路空間裡面
- docker run -d --network my-bridge --name bridge-mode002 alpine tail -f /dev/null
- docker container ls
- docker network inspect my-bridge
- 就可以看到剛剛的bridge-mode002
- docker exec -it ContainerID
- ip addr ls
![image](https://github.com/Tomalison/Docker/assets/96727036/05293408-14e1-45c4-b45a-6e6a3499be9f)
- 接下來要測試這兩個Container可不可以互通
- 我們可以在剛剛.3的網段ping .2的網段
![image](https://github.com/Tomalison/Docker/assets/96727036/110ff526-e8b2-4b07-97ab-a8ae83a5b278)

- 可以成功收到回應，代表剛剛的bridge-mode001跟bridge-mode002是可以彼此互通。
- 再來要在建一個container放到另一個bridge之中
- 首先
- docker network create --driver bridge their-bridge
- docker run -d --network their-bridge --name bridge-mode003 alpine tail -f /dev/null
- docker container ls
- docker network inspect

- 這次他的Subnet屬於172.21.0.0/16
- bridge-mode003 他的ID 是172.21.0.2/16
![image](https://github.com/Tomalison/Docker/assets/96727036/e05a1da0-2788-4501-a5d4-7ce25ed47fda)

- 接下來先複製他的ContainerID
- docker exec -it ContainerID /bin/sh
- ip addr ls
- ping 8.8.8.8 CTRL+C結束
- ping 172.20.0.2 這網路就不通
- 不同Bridge之間的網路是不會互通的
- 那我現在要將003加到001/002所屬網路中呢
- 用這個指令
- docker network connect my-bridge bridge-mode003
- docker network inspect my-bridge
![image](https://github.com/Tomalison/Docker/assets/96727036/520b3983-5ac4-44b0-afe3-6a65c855a748)

- 他的IP 如圖變成172.20.0.4/16，這時候bridge-mode003就有兩個IP
- docker exec -it ContainerID /bin/sh
- ping 172.20.0.2就可以通了

## Docker網路介紹Container模式
- docker container ls
- docker run -d --network container:bridge-mode001 --name container-mode001 alpine tail -f /dev/null
- docker network inspect
![image](https://github.com/Tomalison/Docker/assets/96727036/4a202885-2c0d-40cc-84b6-6cdbb4033531)
- 可以看到我們container還是維持三個 001/002/003
- 沒有看到剛剛新創的container-mode001，原因是他是去copy現有的container網路設定
- 如果你要去連線container-mode001
- docker exec -it ContainerID /bin/sh
- ip addr ls 就可以看到他的IP跟001的一樣
![image](https://github.com/Tomalison/Docker/assets/96727036/f4ec0928-0c5b-47fa-8502-e7c6ff3571f0)

## Docker網路介紹Host模式
- cat Dockerfile
![image](https://github.com/Tomalison/Docker/assets/96727036/3e42285a-2c62-4e46-9e40-b40eb3f95fb4)
- docker build -t uopsdod/my-apache .
- docker images
- docker run -d --network host --name my-apache uopsdod/my-apache
- docker container ls
- docker exec -it ContainerID /bin/sh
- docker network inspect host
![image](https://github.com/Tomalison/Docker/assets/96727036/390810f4-8675-427e-99f7-b26896e6f342)
- host mode的意思就是我們的Container不再屬於他們Container那一層，直接把他當成跟他的VM Linux host一模一樣的網路世界。
- netstat -tulpn
- exit
- echo $(docker-machine ip)
- 這個時候就可以用VM上的IP與埠號 連到之前那個Container
![image](https://github.com/Tomalison/Docker/assets/96727036/e81b3adc-138b-4ad2-89c2-02460a1bd6fc)
- 192.168.99.100:80

## 建立與使用Docker Volume

![image](https://github.com/Tomalison/Docker/assets/96727036/561ec210-e1ee-4901-ab0b-f3fd186c8d29)

- 容器存在Container Disk中當容器砍掉他也會清掉她的空間
- 如果將容器存在Linux的Volume01 或Volume02那他容器砍掉，還是會存在Volume中

- cat Dockerfile
- docker build -t uopsdod/apache001 .
- docker run -d -p 8080:80 uopsdod/apache001
- docker container ls
- echo $(docker-machine ip)
- docker exec -it ContainerID /bin/sh
- ls
- cat index.html
- echo "I made this change in 1990" >> index.html
- exit
- docker stop ContainerID
- docker rm ContainerID
- 在啟動一次docker run
- 就可以看到剛剛echo的那段文字不見了
- 代表他剛剛存的disk空間都會被清掉
- 如果我們想要保存剛剛container的disk資料，我們要用下面的docker volume
- dpcker volume ls
- docker volume create mainpage-vol
- docker volume inspect mainpage-vol
![image](https://github.com/Tomalison/Docker/assets/96727036/a6f36bd5-2e1e-4841-acd3-057f1766840f)
- docker run -d -p 8080:80 -v mainpage-vol:/var/www/localhost/htdocs/(container裡面的folder) uopsdod/apache001
- 冒號左邊是VM linux disk的空間，右邊是Container的disk空間。把兩個mapping起來。

- docker run -d -p 8081:80 -v mainpage-vol:/var/www/localhost/htdocs/ uopsdod/apache001
- docker container ls
![image](https://github.com/Tomalison/Docker/assets/96727036/ba273b9f-8880-4f94-a5ba-7a22adcdedb6)
- docker exec -it ContainerID /bin/sh
- ls
- cat index.html
- echo "I made this change in 1997" >> index.html
- exit
- docker stop ContainerID
- docker rm ContainerID
- docker run -d -p 8081:80 -v mainpage-vol:/var/www/localhost/htdocs/ uopsdod/apache001
- 看一下之前加的echo是否還存在
- docker container ls
![image](https://github.com/Tomalison/Docker/assets/96727036/996b8964-af88-49c6-9e85-b55ecff3b89d)

- 這次就留下了改變。
![image](https://github.com/Tomalison/Docker/assets/96727036/36b8cfd7-bffd-485e-94d1-226efea792ac)

## 建立與使用Docker Compose
- Services / Networks / Volumes /
![image](https://github.com/Tomalison/Docker/assets/96727036/24aac696-f101-4f32-a9f9-82ffff46f92a)
- 透過Compose可以將上面三個元素一起管理起來
![image](https://github.com/Tomalison/Docker/assets/96727036/ef6b7a01-5ab1-4a3c-a7df-28df6f96bbcd)
- ls
- cat Dockerfile
- touch docker-compose.yml
- 找到檔案位置 開啟編輯器
``` sh
version: "3.7" (版本)
services:
 myweb:
   bulid:
     context: .
     args:
       whoami: "Tony"
    image: uopsdod/myweb:latest
    ports:
      - "8080:80"
```
- 這是我們要看到的docker compose第一個版本的樣子
- cat docker-compose.yml
- docker-compose build --no-cache
- docker images

![image](https://github.com/Tomalison/Docker/assets/96727036/c5780e5f-e50e-4466-b0c4-5497e1ea6b56)

- docker-compose up -d
- docker container ls
- echo $(docker-machine ip)
![image](https://github.com/Tomalison/Docker/assets/96727036/b0a13c51-45aa-49b0-82bd-8c53d0758aac)

- docker-compose down 把所有container給下架

- 開啟compose檔案繼續編輯，不同container都可以一起起起來
``` sh
version: "3.7" (版本)
services:
 myweb:
   bulid:
     context: .
     args:
       whoami: "Tony"
    image: uopsdod/myweb:latest
    ports:
      - "8080:80"
  myweb2:
    bulid:
      context: .
      args:
        whoami: "Chris"
    image: uopsdod/myweb1:latest
    ports:
      - "8081:80"
  myweb3:
    bulid:
      context: .
      args:
        whoami: "Jane"
    image: uopsdod/myweb2:latest
    ports:
      - "8082:80"
```
- docker-compose.yml
- 確認一下
- docker-compose build --no-cach
- docker images
- docker-compose up -d
- docker container ls

- docker-compose down

- 再回到檔案，用最後一個語法
``` sh
version: "3.7" (版本)
services:
 myweb:
   bulid:
     context: .
     args:
       whoami: "Tony"
    image: uopsdod/myweb:latest
    ports:
      - "8080:80"
  myweb2:
    bulid:
      context: .
      args:
        whoami: "Chris"
    image: uopsdod/myweb1:latest
    ports:
      - "8081:80"
  myweb3:
    bulid:
      context: .
      args:
        whoami: "Jane"
    image: uopsdod/myweb2:latest
    ports:
      - "8082:80"
  myweb4:
    image: uopsdod/myweb:latest 
    ports:
      - "8083:80"
``` sh
(這樣可以用現有的image去起一個container)
- 本次學習service 可以用compose一次啟起多個不同的container

- Networks
- ls
- cat Dockerfile
- cat docker-compose.yml
- docker-compose down
- docker-compose up -d
- docker container ls

- docker network ls
- docker daemon會自動幫我們建一個mydockerfile001_default的bridge網路，而這名稱來自於我們來源目錄
- 並且將我們的container都放到這個bridge當中
- docker network inspect
- docker-compose down
- clear

- 回檔案，
``` sh
version: "3.7" (版本)
services:
 myweb:
   bulid:
     context: .
     args:
       whoami: "Tony"
    image: uopsdod/myweb:latest
    ports:
      - "8080:80"
    networks:
      - mybridge001
  myweb2:
    bulid:
      context: .
      args:
        whoami: "Chris"
    image: uopsdod/myweb1:latest
    ports:
      - "8081:80"
    networks:
      - mybridge001
  myweb3:
    bulid:
      context: .
      args:
        whoami: "Jane"
    image: uopsdod/myweb2:latest
    ports:
      - "8082:80"
    networks:
      - mybridge001
  myweb4:
    image: uopsdod/myweb:latest 
    ports:
      - "8083:80"
    networks:
      - mybridge002
networks:
  mybridge001:
  mybridge002:
```
- 回terminal
- docker-compose build --no-cache
- docker-compose up -d
![image](https://github.com/Tomalison/Docker/assets/96727036/06adec49-6bae-4e78-abb2-ca72fc39e282)

- docker network inspect Bridge名稱 就可以看到剛剛放的Container
![image](https://github.com/Tomalison/Docker/assets/96727036/009a0a0b-59fe-4f95-ab18-5272a6018d07)

## Volumes實作
- docker-compose down
- clear
- ls
- docker volume ls

- 回檔案
``` sh
version: "3.7" (版本)
services:
 myweb:
   bulid:
     context: .
     args:
       whoami: "Tony"
    image: uopsdod/myweb:latest
    ports:
      - "8080:80"
    networks:
      - mybridge001
  myweb2:
    bulid:
      context: .
      args:
        whoami: "Chris"
    image: uopsdod/myweb1:latest
    ports:
      - "8081:80"
    networks:
      - mybridge001
  myweb3:
    bulid:
      context: .
      args:
        whoami: "Jane"
    image: uopsdod/myweb2:latest
    ports:
      - "8082:80"
    networks:
      - mybridge001
  myweb4:
    image: uopsdod/myweb:latest 
    ports:
      - "8083:80"
    networks:
      - mybridge002
  myweb5:
    image: uopsdod/myweb:latest 
    ports:
      - "8084:80"
    networks:
      - mybridge002     
    volumes:
      - mainpage-vol002:var/www/localhost/htdocs/ (container的目錄位置)
networks:
  mybridge001:
  mybridge002:
volumes:
  mainpage-vol002:
```
- 回terminal
- docker-compose build --no-cache
- clear
- docker-compose up -d
- docker volume ls
- docker container ls
- echo $(docker-machine ip)
- 到瀏覽器 打上ip:8084
- docker container ls
- docker exec -it ContainerID /bin/sh
- ls
- cat index.html
- echo "I made this change in 1997" >> index.html
- exit
- docker-compose down
- clear
- docker-compose up -d
- 這時剛剛新加的echo就會留在myweb5，原因是我們有加了一個volume_mainpage-vol002進去使用
- 在container消失啟動都還是會存在。
![image](https://github.com/Tomalison/Docker/assets/96727036/46af552f-3617-4b02-b360-1e01b5afa05d)
- IT人員建立server、管理人員權限、管理網路流通、
- 這樣的建造方式會將成本壓在人身上而不是程式碼身上
- IT環境較難掌握
- Infrastructure as Code
- 讓整體的IT環境濃縮在一個設定檔之中
![image](https://github.com/Tomalison/Docker/assets/96727036/82b91560-502a-4d8a-af68-eb0f5ce7a189)
- 透過一鑑部屬，用同一個設定檔，可以啟起一個全新且完整的IT環境
- 透過這個方式，只要少數IT人員去維護，跟版本控制
- 第一種在一個小層級上，可以用容器化的方式去實作，Docker Compose就是其中之一。他可以快速部屬好幾個Container，而每個Container之中。可以進行所謂的前端、後端、資料庫的部暑。

- 另外一個選擇是去雲端上面進行部屬 (ex Cloud Formation
![image](https://github.com/Tomalison/Docker/assets/96727036/5bb4c554-6f90-445b-9a6e-679953f31fcd)
- 寫一個單一設定檔，快速部屬多個VM，在每個虛擬機之中快速部屬各自的應用程式。
![image](https://github.com/Tomalison/Docker/assets/96727036/0e58dcd3-46aa-481e-b959-32b70f281dda)

![image](https://github.com/Tomalison/Docker/assets/96727036/2e9b7ad6-ccff-497f-ad66-a2efccf832af)
![image](https://github.com/Tomalison/Docker/assets/96727036/80ce8da5-4f37-459b-9f3b-dd73940199f6)


## 簡化專案佈版流程
![image](https://github.com/Tomalison/Docker/assets/96727036/4c979caf-1cbc-4402-80a0-d9c1613b7b93)
![image](https://github.com/Tomalison/Docker/assets/96727036/fcb5553f-f755-4119-a3de-493c30fc614b)

- 解壓縮Zip檔(課程範例)，開啟docker daemon程式

- 先cd docker-demo/tmp/ (檔案的目錄位置)
- ls (查看本地有甚麼檔案目錄) -a(-a可以看到隱藏的檔案)
- 在cd 到剛剛解壓縮的目錄底下 ex: docker-course-grand-demo-compose-master (如下圖)
![image](https://github.com/Tomalison/Docker/assets/96727036/eb780ba2-c9ab-4a58-9596-e853091b10b9)
- ls (可以看到範例準備的app、db、docker-compose.yml、web)
- docker-compose build --no-cache 
- docker-compose up -d
- docker container ls
- 先找出VM Linux的IP位置
- echo $(docker-machine ip) 到這邊就佈署好了
- 先關掉專案 docker-compose down

- 解析:
![image](https://github.com/Tomalison/Docker/assets/96727036/29f74b45-0730-4cd5-b85d-0bd1330e1806)
- cat .env  (可以看到幾個環境參數被放上去使用)
- 開啟docker-compose.yml檔案解析
![image](https://github.com/Tomalison/Docker/assets/96727036/5f765bc2-69c2-420d-ab29-2738be5fa0e3)
![image](https://github.com/Tomalison/Docker/assets/96727036/1b8a492c-ad60-4a06-85bf-94edc1b03e48)
![image](https://github.com/Tomalison/Docker/assets/96727036/36023aea-45bb-4c3c-a135-966fcbcc3dbe)
![1687660939798](https://github.com/Tomalison/Docker/assets/96727036/c8f26e52-e17b-4aa9-a72f-4d57d1bf74fb)
![image](https://github.com/Tomalison/Docker/assets/96727036/60056243-af82-4961-b30d-d136ba9399a7)
``` sh
version: "3.7"
servuces:   #以下規劃3個容器
  mysql-db:
    container_name: mydb
    build:
      context: ./db  #當下目錄之下的Db子目錄_裡面有一個Dockerfile檔案(上圖)
    image: uopsdod/mysql-db-01 #docker hub帳號配上image名稱
    ports:
      - "3306:3306"
    environment:
      - MYSQL_ROOT_PASSWORD=${MYSQL_ROOT_PASSWORD}
      - MYSQL_USER=${MYSQL_USER}
      - MYSQL_PASSWORD=${MYSQL_PASSWORD}
      - MYSQL_DATABASE=${MYSQL_DATABASE} #這些值是初始化db得時候去使用的，會幫我們建立第一個使用者跟件第一個database
    networks:
      my-bridge-001:
    volumes:
      - db-data:/var/lib/mysql  #db-data的空間 mapping到容器裡面的/var/lib/mysql空間 容器消失之後，還能保存我們db資料
  java-app:                    
    container_name: myapp
    build:
      context: ./app  
                     #程式專案的資料夾中有其所需的程式檔案，也有一個Dockerfile，裡面有maven這個指令，ENV環境變數使用 
                     #Dockerfile中
                     #DB_HOST_iP=localhost可以透過同一個bridge空間，去連線到db server得位置
                     #並預設container預設路徑
                     #COPY把當下所有Linux VM目錄位置的東西全部COPY一份到Container裡
                     #再COPY一個叫做wiat-for-it的shell指令
                     #Run一個chmod指令 加上一個新權限讓他可以被執行，可以像Windows的exe檔直接啟用的那種意思
                     #再Run一個maven指令，特別用在java專案上，把我們java專案打包成一包，可以被佈署的東西
                     #最後的CMD則是規範我們docker container起來之後，首先要執行的指令是甚麼。
    image: uopsdod/java-app-01
    ports:
      - "8080:8080"
    command: ./wait-for-it.sh mydb:3306 -- java -jar target/accessing-data-mysql-0.0.1-SNAPSHOT.jar 
              #再container再啟動時等待第一批container啟動好後再執行。
              #mydb:3306是docker空間的DNS name，這會一值偵測啟動沒，而啟動後會執行會面那段java，將專案起起來。只有在同一個networks才能呼叫
    environment:
      - DB_HOST_IP=mydb
    networks:
      my-bridge-001:
  react-web:
    container_name: myweb
    build:
      context: ./web  #裡面的Dockerfile 的CMD["npm","start"]是我們啟動react專案的方式
    image: uopsdod/react-web-01
    command: ./wait-for-it.sh ${API_HOST_IP}:8080 -- npm start  #API_HOST_IP指的是我們Linux VM的IP
    ports:
      - "3000:3000"
    environment:
      - REACT_APP_API_HOST_IP=${API_HOST_IP}

networks:
  my-bridge-001:
volumes:
  db-date: #建造一個新Volume叫做db-data
```
![image](https://github.com/Tomalison/Docker/assets/96727036/a83d7c81-d545-4544-91e2-edf13b8f20c3)
- 可以用docker volume ls  / docker network ls / docker container ls / docker network inspect ContainerIP

## 建立乾淨測試環境實作示範
![image](https://github.com/Tomalison/Docker/assets/96727036/6b040d73-dc9a-4fdc-8ee3-e4a3a3834e34)

- 因為要清掉測試環境，所以原本的docker-compose.yml的 volumes:要清掉
![image](https://github.com/Tomalison/Docker/assets/96727036/74150dec-1b16-480b-85b7-4b938251d030)
- 再db資料夾中，有一個sql-scripts，測試資料有一組Insert into table讓我們的測試資料塞進資料庫，並在多塞七組測試資料進去
- 因為有改db測試資料語法 我們要重新build一遍
- 先docker-compose down關閉 再docker-compose build --no-cache
- 然後再docker-compose up -d再背景執行 echo $(docker-machine ip) + docker container ls
![image](https://github.com/Tomalison/Docker/assets/96727036/738f8c7e-6031-46de-9283-04ab3ecac4cc)

- 如果我們要再做第二輪的測試，我們就打上docker down 再 docker-compose up -d 透過這個方式就可不斷建立新的測試環境

## 實作跨平台佈署
![image](https://github.com/Tomalison/Docker/assets/96727036/f87aacf6-fe70-442d-b3eb-abcfc4eda72a)

- image上傳到Docker hub / 蘋果新的m1/m2要多考慮CPU架構(ARM的CPU架構) 
- 登入
![image](https://github.com/Tomalison/Docker/assets/96727036/3bdd74ea-623a-4641-afcc-b49634ac838a)

- 上傳到Docker hub
- docker-compose push  這只能建立你這個CPU的規格image
- 所以我們打開Docker Engine 看到Settings這邊 >>Features in development>>Experimental features確認Access experimental features有打勾

- 先刪掉所有Image
- docker rmi $(docker images -a -q)
- docker buildx create --use --name mybuilder可以一次建立多個CPU規格的image
- docker buildx ls
- docker buildx build --platform linux/amd64,linux/arm64 -t uopsdod/mysql-db-01 --push ./db
![image](https://github.com/Tomalison/Docker/assets/96727036/cac4900d-e8a1-4478-b7a4-7a1c0b6e89bf)
![image](https://github.com/Tomalison/Docker/assets/96727036/49b48d7a-d00c-4cf7-9882-fc3f73f0d81c)
- docker buildx build --platform linux/amd64,linux/arm64 -t uopsdod/java-app-01 --push ./app
- docker buildx build --platform linux/amd64,linux/arm64 -t uopsdod/react-web-01 --push ./web

## 實作2
#### MAC
- docker pull uopsdod/mysql-db-01
- docker pull uopsdod/java-app-01
- docker pull uopsdod/react-web-01
- 抓下三個image
- docker-compose up -d
- echo $(docker-machine ip)  / docker-compose down

如何一次將所有images都抓下來
- docker images 
- docker-compose pull

#### Linux
- sudo yum install -y git
- sudo git clone 專案github位置
![image](https://github.com/Tomalison/Docker/assets/96727036/db46327f-0ccd-4872-99ca-cf0f125713dc)
- cd 專案目錄位置/

![image](https://github.com/Tomalison/Docker/assets/96727036/ae02057c-3f84-4e19-975c-e84e41117661)

- 要將API_HOST_IP改為Linux最外的IP curl http://checkip.amazonaws.com 看到public ip
- vi .env 編輯檔案(a)，將IP改掉
![image](https://github.com/Tomalison/Docker/assets/96727036/cac15ba0-4406-4208-88d6-60663471c8b5)
- 編輯完後按下ESC，然後按下 :wq
![image](https://github.com/Tomalison/Docker/assets/96727036/2380cd8a-a453-4b4c-832d-657de6cef77e)
- sudo docker images
- sudo /usr/local/bin/docker-compose pull
- sudo docker images
- sudo /usr/local/bin/docker-compose up -d
- sudo docker container ls

#### Windows佈署示範
- 先將範例檔案放到Program Files>Docker Toolbox
- cd 檔案目錄/
- ls -a
- cat .env
- echo $(docker-machine ip)
![image](https://github.com/Tomalison/Docker/assets/96727036/76671590-cc90-4eb9-8b56-e4ce71ba4c3c)

- docker images
- docker-compose pull
- docker images
- docker-compose up -d
- docker container ;s
- echo $(docker-machine ip)
- docker-compose down

## 建立與使用Docker Swarm
![image](https://github.com/Tomalison/Docker/assets/96727036/07f0d5c1-8eb9-4ae1-9c7b-60b52581e8f1)
![image](https://github.com/Tomalison/Docker/assets/96727036/20a0b974-cd93-4322-a66a-93e35d594a70)

## Google Kubernetes Engine入門銜接
![image](https://github.com/Tomalison/Docker/assets/96727036/86f3c522-4b0f-4b4e-a4c3-cc2bfd65730a)
- Clusters 建立一個新的Clusters 用預設  >Node pool details >Size選3 >定義工作包讓node有事情做 > workloads >deploy >定義container >New container image >github(帳號與GKE連結起來_docker hub中要有Dockerfile)
![image](https://github.com/Tomalison/Docker/assets/96727036/40aa44ef-5817-4a9c-990e-ed19e7642764)
![image](https://github.com/Tomalison/Docker/assets/96727036/0e8593dd-92a2-494a-9503-f08a3db253b1)
- 設好deploy
- 一個pods可以擁有多個container
- exposing service 點下expose > port 8080 : 80 > done
![Uploading image.png…]()


##
