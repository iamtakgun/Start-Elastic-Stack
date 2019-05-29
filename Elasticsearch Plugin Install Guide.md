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

## check Point

    *elasticsearch.yml*
    
    http.cors.enabled: true
    http.cors.allow-origin: "*"
    
# HQ 플러그인 - 클러스터 지표를 보기 위한 도구

## python3 Update

	sudo apt-get update
    sudo apt-get install python3 python3-pip
    
## HQ Download/install

	git clone https://github.com/ElasticHQ/elasticsearch-HQ.git
	cd elasticsearch-HQ
        sudo pip3 install -r requirements.txt

## HQ Start

	python3 application.py &

## HQ Check

	netstat -na | grep 5000
	http://{Server FQDN}:5000
