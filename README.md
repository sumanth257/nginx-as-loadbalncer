<img width="1536" height="1024" alt="image" src="https://github.com/user-attachments/assets/972104f8-0a7f-4f24-932d-0a2423a996fc" /># nginx-as-loadbalncer

# 🚀 NGINX Load Balancer with Node.js Servers

This project demonstrates how to configure **NGINX as a Load Balancer** to distribute traffic between two backend **Node.js servers**.

It is a simple and practical example of how reverse proxy and load balancing work in real-world environments.

---

## 📌 Project Architecture

Client Request → NGINX (Load Balancer) → Node Server 1 (Port 3001)
→ Node Server 2 (Port 3002)


NGINX distributes incoming traffic across two backend servers using **round-robin load balancing**.

---

🖥️ Backend Servers

### **server1.js**

                      require('http').createServer((req, res) => {
                             res.end('Response from Server 1');
                            }).listen(3001);


### **server2.js**
                      require('http').createServer((req, res) => {
                              res.end('Response from Server 2');
                            }).listen(3002);


**⚙️ Prerequisites**
Make sure the following are installed:

Ubuntu / Linux Server

Node.js (v14+ recommended)

NGINX

Install Node.js:

          sudo apt update
          sudo apt install nodejs npm -y
          Install NGINX:

          sudo apt install nginx -y
▶️ Running the Node.js Servers
          Start both servers in separate terminals:

          node server1.js
          node server2.js
Test individually:

curl http://localhost:3001
curl http://localhost:3002
**🌐 NGINX Load Balancer Configuration**
Edit the NGINX default config:

           sudo nano /etc/nginx/sites-available/default
            Replace with:

**upstream backend_app {
    server 127.0.0.1:3001;
    server 127.0.0.1:3002;
}
**
server {
    listen 80;

    location / {
        proxy_pass backend_app;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
    }
}
**🔄 Restart NGINX**
              sudo nginx -t
              sudo systemctl restart nginx
**🧪 Testing Load Balancing**
Now access your server IP or domain:

          **   curl http://YOUR_SERVER_IP**
            Refresh multiple times — you should see responses alternating:

Response from Server 1

Response from Server 2

**This confirms load balancing is working 🎉**

**📊 How It Works**       

         Client Browser
               │
               ▼
       ┌────────────────┐
       │     NGINX      │
       │  Load Balancer │
       └────────────────┘
          │          │
          ▼          ▼
 ┌────────────┐  ┌────────────┐
 │ Node Server│  │ Node Server│
 │   Port 3001│  │   Port 3002│
 └────────────┘  └────────────┘


**NGINX acts as a reverse proxy

Requests are forwarded to backend servers

Default algorithm: Round Robin
**
Improves scalability and availability


**🧮 Load Balancing Algorithms in NGINX**
**Algorithm	Behavior:**
ROUND ROBIN  : Default rotates through all backends equally
LEAST_CONN 	: Sends traffic to the backend with the fewest active connections
IP_HASH     :	Uses client IP to consistently route requests to the same backend

