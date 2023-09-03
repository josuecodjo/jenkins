# Flask App with Pytest

## Getting started

To run the project follow these commands:

```
multipass launch -n qa -c 1 -m 2G -d 10G jammy
multipass shell qa
sudo apt update
sudo apt install –y python3 python3-pip python3-venv git
sudo apt install –y nodejs npm
sudo npm install pm2 –g
sudo pm2 startup
git clone https://github.com/josuecodjo/jenkins.git
cp -rf 2-ci_cd_server /home/ubuntu/qa
cd /home/ubuntu/qa
pip install –r requirements.txt
pm2 start "python3 -m flask run --host 0.0.0.0 --port 5400" --name "py_app_qa" --watch
```