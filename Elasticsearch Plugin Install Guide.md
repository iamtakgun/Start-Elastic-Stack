# head 플러그인 – 클러스터를 보기 위한 도구

## nodejs-legay, npm install

    sudo apt install nodejs-legacy npm

## head Download

    git clone https://github.com/mobz/elasticsearch-head.git
    cd elasticsearch-head/
    
## head install/run

    npm install
    npm run start

## head Check
    
    netstat na | grep 9100
    http://{Server FQDN}:9100
