# docker-usage-instructiona



/var/lib/containers 底下會存放各個容器的 log 可以設定小一點或是定期清除，避免太多。
Docker system df 

docker-compose system prune 
docker-compose system prune -a 
docker-compose system prune -af 直接刪除所有無用的 images, volume, container,
networks


docker system df 查看概要
docker system df -v 查看報告
docker system prune -af

docker build -t docker-next . 第一次創建 docker-next image.
docker build -t docker-next:v2 . 第二次創建 docker-next image, node 已經下載過就不用重新下載

docker run -p 3000:3000 docker-next:v4 可以運行容器化 next.js v4

docker run --interactive --tty --entrypoint /bin/sh nginx:latest 開啟特定 images 容器 cmd 模式，因為有時候你只想在容器內執行某些命令
https://www.tutorialworks.com/why-containers-stop/#:~:text=The%20main%20process%20inside%20the,ends%2C%20the%20container%20will%20exit.





# next note
- next 專案在沒有 yarn install 之前是 114m, yarn install 後變成 1.02g, 在 yarn build 變成 1.03g
- 在慢慢提升版本，一個一個步驟 build image 時 docker 會做 cache, yarn install 
- copy . . 有把 .env 複製進去
- 在一般 docker 處理後，改用 docker-compose 設定仍然有 cache yarn install, workdir. copy package.json. 但是 copy . . 沒有被 cache yarn build 也沒有
- docker-compose build 出來的 image 名稱會是你 image 跟設定的名稱
- 容器內已經有 build 可是使用 docker-compose up 會出現說你沒有 build .next 然後停止容器
- docker-compose 修改後不用重 build 只要 docker-compose up 就會生效，像是 volume
- 打印所有的環境變數，cmd env 就好
- docker-compose environment 設定完是會到 container 的環境中的
- 其實 volumes 掛載是有效果的。還原下事情經過，一開始我掛載的時候因為 host 沒有重新 yarn build 所以一開始掛載到 container 他沒有順利啟動 server 成功，其實是先掛載發現 host 沒 build 成功所以 container stop，stop 後我使用 docker inspect 去看發現沒有掛載。實際上如前述是有執行掛載只是失敗就停止了，那為什麼會失敗呢？就是因為 docker compose cmd 執行錯誤而形成終結
- dockercompose 改變 volume 會有變化, 改變 dockerfile 沒有變化？？
- 用 volume 掛載包 image 好像也不會減少 iamge 的大小
- docker exec -it 需要是 container 存在才可以使用
- docker 的思維似乎是你需要設置到可以運行服務，如果沒有 container 會 stop，這也就是說你沒辦法用 docker "只" 建立需要的環境, 在 host 處理檔案。要馬就在 host 自己處理不然就是都 docker 幫你處裡。
- 光複製 nodemodule 就要 1min 至少. yarn install >200s, yarn build >280s. total build 900s...從 1.03g -> 1.61g


檢查前10名當前資料夾的檔案大小
du -hsx * | sort -rh | head -10

df -h 查看機器空間大小
du -hs /var  查看當前資料夾得檔案大小
du -hs /var/lib/docker/ 查看當前 docker 資料夾的檔案大小

killall node
https://www.geeksforgeeks.org/how-to-kill-all-instances-of-a-node-js-process-via-command-line/

- 遇到要刪除 /var/log/journal 的情況。因為硬碟容量不夠，刪來刪去要刪到 journal 可以刪除，不過如果不想要一次刪除全部，可以只留 500M 的日誌檔案。之前的刪掉。
journalctl --vacuum-size=500M
https://cloud.tencent.com/developer/article/1423873?from=article.detail.1446278
- 發現 docker 在 service build 的時候，在安裝 yarn 套件會把檔案下載到這裡 /usr/local/share/.cache/yarn/ 佔了1.5G 就先清除掉。下次要安裝就重新安裝，

# docker
- docker 好用在可以容器化。難用在測試不容易，有情境卻沒有明確使用流程，都要先試錯之後打通路才好用。
- 創建一個容器化的 mysql `docker run --name sql2 -p 3306:3306 -e MYSQL_ROOT_PASSWORD=Dev127336 -d mysql`. 需要先建立 db 在使用 tableplus edit import 去 import *.sql 檔案注入資料。
- 使用 DOCKER mysql stop/start container 資料庫異動是有存在的。

# cors
- 跨域一般在後端設置。
- Response to preflight request doesn't pass access control check: The value of the 'Access-Control-Allow-Origin' header in the response must not be the wildcard '*' when the request's credentials mode is 'include'. The credentials mode of requests initiated by the XMLHttpRequest is controlled by the withCredentials attribute. 當出現的時候，是指前端帶上 credentials 時，後端伺服器必須明確指定是誰發送的，可以接收而不是用*字號。

# case study
- 一台 t3.medium Production 30gib 的機器，也是經常會遇到 no space 的情況。其空間使用狀況如下：
- 14g var 
    - docker 8, 
    - www 3 nginx, admin, hub, fit 
- 1.7g tmp
- 6.5g usr 
- 1.5g hone
- 1.3g snap


# Leetcode


# DP concept
- divide and conquer (將大問題化為多個小問題) 的觀念
- 節省記憶體是動態規劃當中重要的課題！
- https://web.ntnu.edu.tw/~algo/DynamicProgramming.html