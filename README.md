# TinyGoogle

TinyGoogle built with Flask + Bootstrap + Google CSE

<h3 align="center">Live Demo: http://tinygoo.herokuapp.com</h3>

<p align="center">
  <a href="http://tinygoo.herokuapp.com/" target="\_blank">
    <img src="https://github.com/yrq110/TinyGoogle/blob/master/static/images/readme/main_page_screenshot.png" width="700px">
  </a>
</p>

> Note: the demo may need some spin up time if nobody has accessed it for a certain period.

## Features

* Search content by Google Custom Search API
* No reCAPTCHA

## Search Times

There are 4 engines in this demo. Each engine can search 100 times/day.

If the demo run out of search times when you use, please try just another day.

## Requirements

* python 3.5
* flask 0.11.1
* gunicorn 19.6.0
* requests 2.12.1
* flask-bootstrap 3.3.7.0

## Build Setup

1. install requirements

  ```bash
  pip install -r requirements.txt
  ```
2. run

  ```bash
  gunicorn app:app  
  # server at http://127.0.0.1:8000
  ```

## Config

1. in `data/engine.json`, you can change&add the engine's `key` and `cx` values:

  ```
    {
      "YOUR_ENGINE":{
        "name":"YOUR_NAME",
        "key":"YOUR_API_KEY",
        "cx":"YOUR_ENGINE_ID"
      }
    },
  ```
2. where to get CSE ID and Google API key :

  [Google CSE](https://cse.google.com/) & [Google API Console](https://console.developers.google.com/)

3. set http proxy, in `app.py`, lines 11

```
# TODO 设置代理, set http proxy
proxies = {
  "http": "http://10.0.0.2:9091",
  "https": "https://10.0.0.2:9091",
}
```


## create a docker image

I don't know how to write a dockerfile, so I decide to create a new docker image.

```shell
# 1. install python3.5
docker pull python:3.5.10-slim-buster

# 2. run it

# 3. install vim, git
apt-get -y install vim git

# 4. git clone
cd /home
git clone https://github.com/bcaso/TinyGoogle
# install packages, and rename config file
pip install -r requirements.txt
cd TinyGoogle/data
mv engine.json engine.json.bak

# 5. Have your engine.json ready, for example /home/bcaso/docker/tinyGoogle/engine.json

# 6. config http proxy, at app.py, lines:11
vim app.py

# 7. start a new terminal, run `docker ps`, then commit it to a new docker image.
docker ps
docker commit -m 'tinyGoogle configured' {continer running id} tinyGoogle:0.1

# 8. stop and remove old container
docker ps
docker rm -f {tinyGoogle old container id}
```

run it:

```shell
sudo docker run -itd \
    -p 8000:8000 \
    -w /home/TinyGoogle \
    --restart=always \
    --name tinygoogle \
    -v /home/bcaso/docker/tinyGoogle/engine.json:/home/TinyGoogle/data/engine.json \
    tinygoogle:0.1 \
    gunicorn --bind 0.0.0.0:8000 app:app
```

Auto load next page:  
https://chrome.google.com/webstore/detail/uautopagerize/kdplapeciagkkjoignnkfpbfkebcfbpb

![image](https://user-images.githubusercontent.com/32212684/169082099-b6db1e77-69b6-4bd6-a361-312d37e09239.png)



## License

TinyGoogle is licensed under [MIT](http://opensource.org/licenses/MIT)
