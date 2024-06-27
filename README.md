# yolov10_cvat_auto_annotation
This repository consists of completed config files to launch yolov10 for auto-annotation with a detailed guide. Have fun!

# Integrate YOLOv10 model into CVAT for automatic annotation running on GPU!


# Using Windows Desktop

- Download Docker Desktop from official website (https://www.docker.com/products/docker-desktop/)
- Install WSL using terminal or command line
  ```
  wsl --install
  ```
- It should automatically download a Ubuntu dist.  Create login/pass. And get to the console.
- Go to main folder
    ```
    cd
    ```
- Enter these commands one-by-one
    ```
	sudo apt-get update
    
	sudo apt-get --no-install-recommends install -y \
  	apt-transport-https \
  	ca-certificates \
  	curl \
  	gnupg-agent \
  	software-properties-common
    
	curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
	sudo add-apt-repository \
  	"deb [arch=amd64] https://download.docker.com/linux/ubuntu \
  	$(lsb_release -cs) \
  	stable"
    
	sudo apt-get update
    
	sudo apt-get --no-install-recommends install -y \
  	docker-ce docker-ce-cli containerd.io docker-compose-plugin
   ```
- Give docker sudo perms.
```
sudo groupadd docker
sudo usermod -aG docker $USER
```
## CVAT Download
- Download cvat from their repository and go to the folder with console
```
git clone https://github.com/cvat-ai/cvat
cd cvat
```
- Install `nuclio`. To install a proper version for cvat. Go to the file inside the cvat folder `components/serverless/docker-compose.serverless.yml`. Find the proper version. For me it's 1.13.0*
   
	```
	wget https://github.com/nuclio/nuclio/releases/download/<version>/nuctl-1.13.0-linux-amd64
	```
- After downloading the nuclio, give it a proper permission and do a softlink.*
   
	```
	sudo chmod +x nuctl-1.13.0-linux-amd64
	sudo ln -sf $(pwd)/nuctl-1.13.0-linux-amd64 /usr/local/bin/nuctl
	```
 - Start CVAT together with the plugin use for AI automatic annotation assistant.
	
	```
	docker compose -f docker-compose.yml -f components/serverless/docker-compose.serverless.yml up -d
	```
- You will need to create a login/password in the console using this command:
	```
	docker exec -it cvat_server bash -ic 'python3 ~/manage.py createsuperuser'
	```
## Launching CVAT
- Now go to the Google Chrome browser in your Windows. And write this in the website bar:
  ```
  localhost:8080
  ```
- Log in using aforementioned log/pass
- Go to the `nuclio`. Enter this in the browser:
  ```
  localhost:8070
  ```
- Now you should create new project called `cvat`.
- Download the repository. Copy all the files to the Ubuntu to the `<cvat folder>/serverless`
- Enter the cvat project in `nuclio` in Chrome. Push `NEW FUNCTION` button.
- IMPORT the yaml file and then CREATE
- Copy all the code from `main.py` change the type of YOLO you want to deploy.
- Click deploy and wait. After that refresh the Cvat tab in Chrome. And, hopefully, you will be able to see the Models Tab with yolov10 inside.

# Note
1. If you want to stop the server, write this in console:
```
docker compose -f docker-compose.yml -f components/serverless/docker-compose.serverless.yml down
```
2. If something glitches and WSL consumes a lot of disk. Close it with this command in the windows cmd:
```
wsl.exe --shutdown
```
3. Don't forget that you need at 3vram gpu to work.
