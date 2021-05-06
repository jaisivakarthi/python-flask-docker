# python-flask-docker
Docker comments:
---------------		
	sudo docker images
	sudo docker ps
	sudo docker ps --all
	sudo docker ps -a
	sudo docker build --tag container_name .
	sudo docker run -t container_name

Sample application using flask & docker:
---------------------------------------

Let’s create a simple Python application using the Flask framework that we’ll use as our example. Create a directory in your local machine named python-docker and follow the steps below to create a simple web server.

	$ cd /path/to/python-docker
	$ pip3 install Flask
	$ pip3 freeze > requirements.txt
	$ touch app.py
	
Now, let’s add some code to handle simple web requests. Open this working directory in your favorite IDE and enter the following code into the app.py file.

	app.py:
	-------

  from flask import Flask,render_template
  import socket

  app = Flask(__name__)

  @app.route("/")
  def index():
    try:
        host_name = socket.gethostname()
        host_ip = socket.gethostbyname(host_name)
        return render_template('index.html', hostname=host_name, ip=host_ip)
    except:
        return render_template('error.html')


  if __name__ == "__main__":
    app.run(host='0.0.0.0', port=8080)


	Test the application:
	--------------------
		env FLASK_APP=app/app.py python3 -m flask run

		To test that the application is working properly, open a new browser and navigate to http://localhost:5000.

	Create a Dockerfile for Python:
	------------------------------

		Now that our application is running properly, let’s take a look at creating a Dockerfile.

		A Dockerfile is a text document that contains all the commands a user could call on the command line to assemble an image. When we tell Docker to build our image by executing the docker build command, Docker reads these instructions and execute them consecutively and create a Docker image as a result.

		Let’s walk through creating a Dockerfile for our application. In the root of your working directory, create a file named Dockerfile and open this file in your text editor.	

		Dockerfile:
		----------
			# syntax=docker/dockerfile:1

			FROM python:3.8-slim-buster

			WORKDIR /app

			COPY requirements.txt requirements.txt
			RUN pip3 install -r requirements.txt

			COPY . .

			CMD [ "python3", "-m" , "flask", "run", "--host=0.0.0.0"] #	CMD ["app.py"]

		Directory structure:
		-------------------
			python-docker
			|____ app.py
			|____ requirements.txt
			|____ Dockerfile	

		Build an image:
		--------------
			We use the docker build command.

			$ docker build --tag python-docker .	

			View local images:
			-----------------
				$ docker images

			Tag images:
			-----------
				The rmi command stands for remove image.

				$ docker rmi dockerimagename

		Run your image as a container:
		-----------------------------
			$ docker run dockerimagename(python-docker)

			publish:
			--------
				$ docker run --publish 5000:5000 python-docker

				Now, let’s rerun the curl command from above. Remember to open a new terminal.

				$ curl localhost:5000	 	

			Run in detached mode:
			--------------------
				Docker can run your container in detached mode or in the background. To do this, we can use the --detach or -d for short.

				$ docker run -d -p 5000:5000 python-docker

			List containers:
			---------------
				$ docker ps - Show the running container
				$ docker ps -a	 - Show all containers
				$ docker ps --all	- Show all containers
				$ docker stop wonderful_kalam - Stop the container name
				$ docker restart wonderful_kalam - Restart the container name
				$ docker rm wpmderfull_kalam - Remove the container name
Note: https://docs.docker.com/language/python/
