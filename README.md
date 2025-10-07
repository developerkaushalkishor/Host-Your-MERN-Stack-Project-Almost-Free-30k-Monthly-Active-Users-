# Host Your MERN Stack Project Almost Free (30k Monthly Active Users)

````markdown
# 💸 How to Host Your MERN Stack Project Almost Free (30k MAU Setup)

**Author:** [Kaushal Kishor](https://github.com/developerkaushalkishor)  
**Last Updated:** October 2025  
**Language:** Hinglish (for easy understanding 🇮🇳)

---

Kaushal, agar tumhare project ke monthly active users (MAU) 30k ke aas-paas hain aur tumhe **sasta ya almost free hosting setup** chahiye —  
toh yeh guide tumhare liye perfect hai 🚀

Yahan tumhe 2 tested options milenge:

1. **Option A – Almost Free (DIY)**
2. **Option B – Low-Cost Managed (for peace of mind)**

Dono setups **MERN stack (MongoDB, Express, React, Node.js)** ke saath perfectly kaam karte hain.

---

## 🅰️ Option A — Ultra-Cheap / Almost Free Setup

### 👨‍💻 Best for
Developers jinko thoda DevOps knowledge hai aur apne server manage karne me comfort hai.

---

### 🖥️ Frontend (React App)

Use **Cloudflare Pages (Free)** —  
Free static site hosting with unlimited bandwidth + free SSL.

**Steps:**
1. Build your React app  
   ```bash
   npm run build
````

2. Push your code to GitHub.
3. Go to [Cloudflare Pages](https://pages.cloudflare.com)
4. Connect your repo → select branch (usually `main`).
5. In build settings:

   * **Build command:** `npm run build`
   * **Build output directory:** `dist` or `build`
6. Click **Deploy** ✅

---

### ⚙️ Backend (Node.js + Express)

Use **Oracle Cloud “Always Free” VM (Ampere A1)**

Free resources:

* 4 vCPU
* 24 GB RAM equivalent
* 2 Compute instances

**Steps:**

1. Create your free Oracle account → [Oracle Free Tier](https://www.oracle.com/in/cloud/free/)
2. Create a VM → Ubuntu 22.04 (ARM)
3. SSH into VM

   ```bash
   ssh ubuntu@your-public-ip
   ```
4. Install Node.js & npm

   ```bash
   sudo apt update
   sudo apt install -y nodejs npm
   ```
5. Install Git & clone your backend repo

   ```bash
   sudo apt install git -y
   git clone https://github.com/yourusername/your-backend.git
   cd your-backend
   ```
6. Install dependencies

   ```bash
   npm install
   ```
7. Create `.env` file:

   ```bash
   PORT=8000
   MONGO_URI=mongodb://localhost:27017/yourdb
   JWT_SECRET=your_jwt_secret
   CORS_ORIGIN=https://yourfrontend.pages.dev
   ```
8. Start your server

   ```bash
   npm run start
   ```

   or better:

   ```bash
   npm install -g pm2
   pm2 start server.js --name backend
   pm2 save
   pm2 startup
   ```

---

### 🗄️ Database (MongoDB)

You have 2 options:

#### 🅰️ Host MongoDB in your Oracle VM (Docker)

```bash
sudo apt install docker.io -y
sudo docker run -d \
  --name mongodb \
  -p 27017:27017 \
  -v ~/mongo-data:/data/db \
  -e MONGO_INITDB_ROOT_USERNAME=root \
  -e MONGO_INITDB_ROOT_PASSWORD=yourpassword \
  mongo:6
```

#### 🅱️ Use **MongoDB Atlas (Free Tier)**

* 512MB storage
* 100 ops/sec (enough for small apps)
  👉 [MongoDB Free Tier](https://www.mongodb.com/cloud/atlas/register)

---

### 🌐 DNS / CDN / SSL (Free)

Use **Cloudflare Free Plan**

* DNS
* CDN caching
* HTTPS SSL
* DDoS protection

👉 [Cloudflare Free Plan](https://www.cloudflare.com/plans/free/)

---

✅ **Pros:** Zero monthly cost
⚠️ **Cons:** Manual updates, backups, and scaling responsibility is yours.

---

## 🅱️ Option B — Low-Cost Managed Setup

### 👨‍💻 Best for

Developers who want **simple, reliable & managed infra** at low cost.

---

### 🖥️ Frontend

**Netlify (Free Tier):**

* 100GB bandwidth / 300 build minutes per month
* Free SSL, CI/CD
  👉 [Netlify Pricing](https://www.netlify.com/pricing)

**Alternate:** Cloudflare Pages (Unlimited free)

---

### ⚙️ Backend + Database (together)

Use **Hetzner Cloud (CX11 VPS)**
💰 ~€3.79/mo (~₹350/month)

Perfect for:

* Node.js backend
* MongoDB container
* Reverse proxy with Nginx or Caddy

👉 [Hetzner Cloud](https://www.hetzner.com/cloud)

---

### 🧩 Alternate Free Platforms (for hobby/demo apps)

| Platform    | Description                                                        |
| ----------- | ------------------------------------------------------------------ |
| **Render**  | Free 750 instance hours per month (auto sleeps) → [Render Docs][7] |
| **Railway** | $5 free credits → $1/month basic plan → [Railway Docs][8]          |

---

## 💡 Recommended Setup (Best Balance)

| Component             | Platform                  | Cost        |
| --------------------- | ------------------------- | ----------- |
| **Frontend**          | Cloudflare Pages          | ₹0          |
| **Backend + MongoDB** | Hetzner CX11 VPS (Docker) | ~₹350/month |
| **DNS + SSL**         | Cloudflare Free           | ₹0          |

If you need **absolutely ₹0 setup**, replace Hetzner with Oracle Always Free VM.

---

## 🚀 Step-by-Step Deployment (npm based)

### 1️⃣ Install Node & Git

```bash
sudo apt update
sudo apt install -y nodejs npm git
```

### 2️⃣ Clone Your Backend

```bash
git clone https://github.com/yourusername/mern-backend.git
cd mern-backend
npm install
```

### 3️⃣ Setup Environment File

Create `.env` in root:

```bash
PORT=8000
MONGO_URI=mongodb://mongo:27017/yourdb
JWT_SECRET=your_jwt_secret
CORS_ORIGIN=https://yourfrontend.pages.dev
```

### 4️⃣ Install Docker & Docker Compose

```bash
sudo apt install docker.io docker-compose -y
```

### 5️⃣ Create `docker-compose.yml`

```yaml
version: "3"
services:
  mongo:
    image: mongo:6
    restart: always
    environment:
      MONGO_INITDB_ROOT_USERNAME: root
      MONGO_INITDB_ROOT_PASSWORD: password
    volumes:
      - ./mongo-data:/data/db

  api:
    build: .
    restart: always
    environment:
      - MONGO_URI=mongodb://root:password@mongo:27017/appdb?authSource=admin
      - JWT_SECRET=your_jwt_secret
      - CORS_ORIGIN=https://yourfrontend.pages.dev
    ports:
      - "8000:8000"
    depends_on:
      - mongo
```

### 6️⃣ Run Containers

```bash
sudo docker-compose up -d
```

### 7️⃣ Setup Reverse Proxy (Caddy)

Install Caddy:

```bash
sudo apt install -y debian-keyring debian-archive-keyring apt-transport-https
curl -1sLf 'https://dl.cloudsmith.io/public/caddy/stable/gpg.key' | sudo apt-key add -
curl -1sLf 'https://dl.cloudsmith.io/public/caddy/stable/debian.deb.txt' | sudo tee /etc/apt/sources.list.d/caddy-stable.list
sudo apt update
sudo apt install caddy
```

Create `/etc/caddy/Caddyfile`:

```
yourdomain.com {
    reverse_proxy localhost:8000
}
```

Reload:

```bash
sudo systemctl reload caddy
```

Now your backend is accessible via HTTPS ✅

---

## 🌐 Frontend Setup (React + Cloudflare Pages)

1. Build frontend:

   ```bash
   npm run build
   ```
2. Push code to GitHub.
3. Connect repo to Cloudflare Pages.
4. In Environment Variables:

   ```
   VITE_API_BASE_URL=https://yourdomain.com
   ```
5. Deploy 🚀

---

## 🔒 CORS (Production)

In backend:

```js
origin: [
  "https://yourfrontend.pages.dev",
  "https://yourdomain.com"
],
credentials: true
```

---

## 🔔 Free Alerts & Monitoring

* **Telegram Bot** → Free admin alerts
* **Uptime Kuma** → Self-hosted uptime monitor
* **Better Stack** → Free tier for website monitoring

---

## ⚡ Handling 30k Monthly Active Users

| Resource     | Platform         | Tip                                             |
| ------------ | ---------------- | ----------------------------------------------- |
| **Frontend** | Cloudflare Pages | Auto CDN scaling                                |
| **Backend**  | Hetzner CX11     | Use PM2 cluster or upgrade to CX21 if CPU bound |
| **Database** | MongoDB on VPS   | Prefer self-hosted for full control             |

---

## 🧱 Simple Architecture

```
[ Cloudflare Pages ] → [ Cloudflare Proxy + SSL ] → [ Hetzner / Oracle VPS → Node.js + MongoDB (Docker) ]
```

Optional (₹0 path): Replace Hetzner with Oracle VM.

---

## 🧰 Useful Commands

**Restart PM2:**

```bash
pm2 restart backend
```

**View Logs:**

```bash
pm2 logs backend
```

**Check Docker Containers:**

```bash
sudo docker ps
```

**Rebuild Containers:**

```bash
sudo docker-compose down && sudo docker-compose up -d
```

---

## 🧾 Conclusion

Ye setup tumhe deta hai **production-ready MERN app** with almost **zero ya minimal cost** 👇

✅ Frontend: Free
✅ Backend: ₹350/month (or ₹0 DIY)
✅ SSL, CDN, DNS: Free
✅ Alerts: Free

💡 **Result:** Fast, scalable, reliable — perfect for 30k+ users.

> *“Smart hosting ≠ expensive hosting. Optimize first, scale later.”* 💪

---

## 🔗 References

[1]: https://pages.cloudflare.com/?utm_source=chatgpt.com
[2]: https://docs.oracle.com/iaas/Content/FreeTier/freetier_topic-Always_Free_Resources.htm?utm_source=chatgpt.com
[3]: https://www.mongodb.com/pricing?utm_source=chatgpt.com
[4]: https://www.cloudflare.com/plans/free/?utm_source=chatgpt.com
[5]: https://www.netlify.com/pricing/?utm_source=chatgpt.com
[6]: https://www.hetzner.com/cloud?utm_source=chatgpt.com
[7]: https://render.com/docs/free?utm_source=chatgpt.com
[8]: https://railway.com/pricing?utm_source=chatgpt.com
[9]: https://www.mongodb.com/docs/atlas/reference/free-shared-limitations/?utm_source=chatgpt.com

---

### ❤️ Author

**[Kaushal Kishor](https://github.com/developerkaushalkishor)**
Full-Stack Developer | MERN | Indie Builder | 3D Printing Enthusiast

> *“Build smart, not expensive.”* 🧠💰
