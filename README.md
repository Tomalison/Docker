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

