# Docker
紀錄Docker學習紀錄

容器化技術，部屬歷史，硬體主機>作業系統>多個Application>當某個App出錯，他會影響其他App>為了解決這個問題，用多台硬體主機解決>後來出現VM(虛擬主機)，一台硬體，會有一個作業系統與一個Hypervisor，在這一層可以建立多個VM，在其中在建立多個OS與App
>Container模式，底層是硬體與OS在來上一層有一個Docker Engine在上面再建立容器與App。用軟體就創造出的虛擬空間。 資源耗費最低。
![image](https://github.com/Tomalison/Docker/assets/96727036/62392d0d-8fd4-41d9-b00a-c80e00cddca4)

透過Docker image在各個作業系統使用。Mac、Linux、Windows。
Diocker hub雲端空間提供其他人下載Docker image。(CentOS 作業系統下來)

Docker三大功用，
1.原本的作業方式是，先拿到程式碼，安裝所需環境與安裝指令跑起來，透過Docekr可以佈屬一個程式部屬包。簡化部屬
2.跨平台部屬，用Docker hub。先安裝Docker Engine環境就可下載下來。
3.建立乾淨測式環境，同樣概念可以用成資料庫安裝(測試資料、資料庫安裝/安裝指令)
![image](https://github.com/Tomalison/Docker/assets/96727036/010cff20-48d5-46f6-a17e-ac0736e11953)

Linux可直接使用外，Windows 跟 MacOS都要使用 Virtual BOX 或OS Native使用

![image](https://github.com/Tomalison/Docker/assets/96727036/0f8a0382-c606-4cc1-8872-60e7b213661f)

先到官網找到Docker Toolbox 找到 toolbox realeses 下載dockertoolbox-24.0.2.exe，持續下一步安裝。
Docker Desktop安裝方式，Windows10以上才可以安裝，安裝完後 用CMD 打上指令wsl --update 安裝Linux環境(linux os)
![image](https://github.com/Tomalison/Docker/assets/96727036/e0708be9-c106-466a-a750-cc97d21a68a4)
開啟docker desktop並登入，docker pull hello-world / docker run hello-world / docker images
![image](https://github.com/Tomalison/Docker/assets/96727036/923c301a-d972-40c1-a734-7fe61da7c9ee)

![image](https://github.com/Tomalison/Docker/assets/96727036/7491dc34-a940-4f05-b96a-5faea22efb49)

Docker使用情境 
先到Docker Hub 進到 Repositories 
然後再命令提示字元 用docker pull ___ 抓你要的
![image](https://github.com/Tomalison/Docker/assets/96727036/acc3007e-8b02-4e56-8f2c-4ea3cfb3f9c2)
再來用docker container run hello-world
TAG
docker pull 專案名稱:latest 抓最新的版本來用
docker pull 專案名稱:v4 抓指定的版本

建立與使用Docker image
mkdir dockertest001
cd dockertest001/ 進到這個目錄
pwd 驗證一下目錄位置

將範例放到剛剛的dockertest001資料夾裡面，並將檔案名稱改為Dockerfile
docker build -t tomsu25478/course-image-build002 .
docker build -t tomsu25478/course-image-build002 --build-arg my_name_is="Tom Su" .
docker container run tomsu25478/course-image-build002 .
cat: read error: Is a directory
You have built a new image from a Dockerfile. Well done, Tom Su!

上傳到Docker Hub
先docker login (如果先登入 就先登出docker logout) 帳號密碼(yours)
docker push 帳號/專案名稱

如何清理乾淨不需要的images
先打上 docker rmi 帳號/專案名稱
docker rm container的代號
這樣就可以再用 docker rmi 清掉
![image](https://github.com/Tomalison/Docker/assets/96727036/e1016ac0-ed99-42b8-9620-deca2ce646aa)

如果要強制清理
docker rmi -f 帳號/專案名稱

建立與使用Docker container
docker container ls 查詢目前有哪些container
docker pull alpine
docker container run --name c001 alpine ls /  這邊可以將container縮寫

當程序不要一直跑得話 可以用clear清掉
docker run -it 可以讓我們需要跑的一個程序 一直跑下去 --name 可以取名
docker run -it --name c003 alpine /bin/sh <--進到該程序需要的東西 再輸入exit 就可以離開這個程序離開這個container
![image](https://github.com/Tomalison/Docker/assets/96727036/bc8e1690-e660-4c82-87c0-4b6a6e830647)

docker run -d (daemon 長期在背景跑)
tail -f 追蹤某個file的log 並把他印到console上 還有tail解決副我們通常用這個file
docker run -d --name c004 alpine tail -f /dev/null
在按docker container ls 可以看到我們這次終於有一個container一直在背景跑 他跑的就是剛剛指令中的tail -f /dev/null
![image](https://github.com/Tomalison/Docker/assets/96727036/c987e65c-0707-4917-be92-31d4712723fa)
還想在這個container加點東西，則打上docker exec -it 再加上我們的container ID +兩個要執行的程序 /bin/sh
![image](https://github.com/Tomalison/Docker/assets/96727036/a7a8cf43-1291-4fcb-95c3-4fa304771336)


![image](https://github.com/Tomalison/Docker/assets/96727036/adcbf12b-553b-425b-97ed-49cde205d116)
要將container清空之前要先將之停下來  docker container stop  ContainerID
先用docker container ls -a查出所有的container
將你要清掉的打上docker container rm ContainerID

建立與使用Dockerfile
我們透過Docker Client執行指令 而這些指令都會送到Docker Engine上面去(這是兩個工作者) 這整個過程中有三個空間 第一個是在硬體主機的OS空間 第二個是在虛擬主機建起來的LinuxOS空間
第三個是Docker Engine去建起來的臨時Container空間。
![image](https://github.com/Tomalison/Docker/assets/96727036/27b48fb2-db18-453e-b27a-c064b9f517cf)
![image](https://github.com/Tomalison/Docker/assets/96727036/b1ea9684-bce3-4844-a77a-ef986a8bdbb6)
![image](https://github.com/Tomalison/Docker/assets/96727036/ab4427de-9c94-45e8-8366-b20dd4aa20e0)
![image](https://github.com/Tomalison/Docker/assets/96727036/55262637-d76a-4b6b-b199-47c0a5005b12)
![image](https://github.com/Tomalison/Docker/assets/96727036/2994c895-faf7-4a44-8a7c-4813b51409aa)

Dockerfile情境語法介紹FROM
首先打上pwd到你想要到的目錄位置>再來打上touch Dockerfile建立空白file的指令>在目錄中打開這個file(用你熟悉的編輯器)
打上 From alpine:latest (內含輕量的LinuxOS) <cat Dockerfile可以印出檔案內容
有了Dockerfile之後就可以建立image >打上docker build -t uopsdod/001 取名為001
讓他在背景跑 docker run -d uopsdod/001 tail -f /dev/null
![image](https://github.com/Tomalison/Docker/assets/96727036/90c99d8a-cf26-4cca-8551-f472f0949ccf)
![image](https://github.com/Tomalison/Docker/assets/96727036/8f3f1967-34f9-4919-a192-7c743eb61489)
![image](https://github.com/Tomalison/Docker/assets/96727036/41468d1e-e69f-4375-86df-e2fd2afc7a17)

![image](https://github.com/Tomalison/Docker/assets/96727036/100a9721-2f42-4bd1-9e0c-c9e554c1853b)
因為已經有container有在背景執行所以要跟她互動 要打上他的containerID :  docker exec -it ContainerID /bin/sh (如上圖)
![image](https://github.com/Tomalison/Docker/assets/96727036/6d5ee2a5-9f48-40fd-abf7-c55eb4a98c95)

而剛剛我們在使用的語法都是由 一開始的Dockerfile內的那段語法 所提供的

Dockerfile情境語法Entrypoint
用在要讓別人使用這個image 但又不用打上一些我一開始設定的內容，這時候要打上entrypoint 該語法後面先用一個["tail", "-f", "/dev/null"]框框 (呈上題
打好了將檔案儲存
然後我們打上docker build -t uopsdod/002 . 這個時候就可以看到執行變成兩步驟(如下圖)
![image](https://github.com/Tomalison/Docker/assets/96727036/2b2f0631-d0f6-4176-afc7-5215a0e9c4cb)
![image](https://github.com/Tomalison/Docker/assets/96727036/6a126b37-0ece-4610-9c8f-c10b46f16a92)

Dockerfile情境語法Run
呈上 我們想要進去建立一個apache server
docker run -d -p 8080:80  這邊port的mapping做好之後 再打上 image名稱  uopsdod/002
![image](https://github.com/Tomalison/Docker/assets/96727036/51fb482d-368b-4fb9-9728-4c11732efb02)
docker exec -it ContainerID /bin/sh
![image](https://github.com/Tomalison/Docker/assets/96727036/b324036f-e950-4111-adca-7c77c80c359d)
呈上圖啟動apache server 打上httpd -D FOREGROUND
![image](https://github.com/Tomalison/Docker/assets/96727036/90598d41-1bb0-4731-8790-468451772095)

接下來要找LINUX VM的位置 首先要知道她的IP位置 打上echo $(docker-machine ip)
![image](https://github.com/Tomalison/Docker/assets/96727036/46770773-5beb-49a8-8c7d-b9cb3b5c2586)

以上的安裝過程我們想要更方便，就要用到指令run
From alpine:latest
RUN echo "I'm in the image building process..."
RUN ls -l / (這是列出所有根目錄下的檔案跟目錄
ENTRYPOINT ["tail", "-f", "/dev/null"]

From alpine:latest
RUN apk --update add apaches
RUN rm -rf /var/cache/apk/*
ENTRYPOINT ["httpd", "-D", "FOREGROUND"]
![image](https://github.com/Tomalison/Docker/assets/96727036/6ad3a94c-8e8a-4cd1-9005-e81a918bf11f)
最後用 docker run -d -p 8081:80 (給LinuxVM一個port對應到aphache的80 port)










