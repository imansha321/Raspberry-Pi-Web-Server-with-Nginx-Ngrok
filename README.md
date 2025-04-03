# 🌐 Raspberry Pi Web Server with Express, Nginx, and Ngrok

Host your HTML/CSS/JS website on a Raspberry Pi using Node.js + Express, serve it via Nginx, and expose it to the world using Ngrok.

---

## ✅ What You'll Need

- Raspberry Pi with Raspberry Pi OS  
- Internet connection  
- Website files (HTML, CSS, JavaScript)  
- Free [Ngrok](https://ngrok.com) account  

---

## 🚀 Setup Instructions

### 1. Install Node.js and npm

```bash
sudo apt install nodejs npm
```

### 2. Install Express and Initialize Project

```bash
mkdir my-web-server && cd my-web-server
npm init -y
npm install express
```

### 3. Create `server.js`

Create a file called `server.js`:

```js
const express = require('express');
const path = require('path');

const app = express();
const PORT = 5000;

// Serve static files from "public" directory
app.use(express.static(path.join(__dirname, 'public')));

app.listen(PORT, () => {
  console.log(`Server running at http://localhost:${PORT}`);
});
```

### 4. Add Website Files

- Create a `public` folder in the same directory as `server.js`.
- Add your `index.html`, CSS, JS files into `public/`.

### 5. Run Your Server

```bash
node server.js
```

> Server will be live at: `http://localhost:5000`

---

## ⚙️ Install and Configure Nginx as Reverse Proxy

### 1. Install Nginx

```bash
sudo apt update && sudo apt install nginx
```

### 2. Configure Nginx

Edit the default site configuration:

```bash
sudo nano /etc/nginx/sites-available/default
```

Replace content with:

```nginx
server {
    listen 80 default_server;
    listen [::]:80 default_server;

    server_name _;

    root /var/www/html;
    index index.html;

    location / {
        proxy_pass http://localhost:5000;
        proxy_http_version 1.1;
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection 'upgrade';
        proxy_set_header Host $host;
        proxy_cache_bypass $http_upgrade;
    }
}
```

### 3. Start Nginx

```bash
sudo /etc/init.d/nginx start
```

---

## 🌍 Expose Server with Ngrok

### 1. Download & Install Ngrok

```bash
wget https://bin.equinox.io/c/4VmDzA7iaHb/ngrok-stable-linux-arm.zip
unzip ngrok-stable-linux-arm.zip
chmod +x ngrok
sudo mv ngrok /usr/local/bin
```

### 2. Authenticate Ngrok

```bash
ngrok config add-authtoken YOUR_AUTHTOKEN
```

### 3. Find Your Local IP Address

```bash
hostname -I
```

### 4. Start Ngrok

```bash
ngrok http <your_ip_address>:80
```

> Example: `ngrok http 192.168.1.25:80`

---

## 🌐 Accessing Your Website

Ngrok will provide a public URL like:

```
https://bb11-2407-c00-6000-b85a-b7cc-9270-1d01-9eca.ngrok-free.app
```

You can now share this URL and access your Raspberry Pi-hosted website from anywhere in the world! 🎉
