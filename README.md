# Docker দিয়ে প্রজেক্ট রান করার গাইড


## 🔧 Docker Engine

Docker Engine হচ্ছে পুরো Docker সিস্টেমের মেইন কম্পোনেন্ট। এটা মূলত তিনটা অংশ নিয়ে গঠিত:

1. **Docker Daemon** – পেছনে থেকে কাজ চালায়, container তৈরি, রান, ম্যানেজ করে।
2. **Docker REST API** – বাইরের সাথে কমিউনিকেট করার রাস্তা।
3. **Docker CLI (Client)** – আমরা টার্মিনাল থেকে যেটা দিয়ে Docker এর সাথে কাজ করি।

---

## 📦 Docker Image

Image হচ্ছে এমন একটা প্যাকেজ যেটার ভিতরে একটা সফটওয়্যার রান করার জন্য যা যা লাগে সব কিছু থাকে:

- Base Image
- Application code
- Dependencies
- Metadata

**Image এর লাইফসাইকেল:**  
`Creation → Storage → Distribution → Execution`

---

## 🧱 Container

Container হচ্ছে Image এর রান করা একটা ইনস্ট্যান্স।  
> Image → রান হলে → Container হয়

---

## 📝 Dockerfile

Dockerfile একটা ফাইল যেখানে ধাপে ধাপে লেখা থাকে কিভাবে একটা Docker Image তৈরি হবে।

আমি সাধারণত এই কমান্ডগুলো ব্যবহার করি:

- `FROM` – কোন base image থেকে শুরু করব
- `WORKDIR` – কাজের directory ঠিক করা
- `COPY` – ফাইল কপি করা
- `RUN` – কোন command রান করব (যেমন dependency install)
- `ENV` – environment variable সেট করা
- `EXPOSE` – কোন port expose করব
- `CMD` – কোন ফাইনাল command রান হবে
- `VOLUME`, `ARG`, `LABEL` – অন্য কমান্ডগুলোও মাঝে মাঝে লাগে

---

## 🗂️ Docker Registry

Docker Registry হচ্ছে যেখান থেকে Image ডাউনলোড বা আপলোড করা হয়।  
**DockerHub** সবচেয়ে বেশি ইউজ হয়।

- Repository → Image গুলার কালেকশন
- Tag → আলাদা ভার্সনের নাম

---

## 🔁 Docker Workflow

```bash
docker pull image_name           # Registry থেকে image নামানো
docker build -t image_name .     # Dockerfile থেকে image বানানো
docker run image_name            # image রান করে container বানানো
docker push image_name           # image registry তে আপলোড করা
```

---

## 🌐 Flask App এর Dockerfile (ডেমো)

```dockerfile
# Base Image
FROM python:3.9

# Working directory সেট করছি
WORKDIR /app

# ফাইল কপি করছি
COPY . /app

# dependency install করছি
RUN pip install -r requirement.txt

# Flask এর port expose করছি
EXPOSE 5000

# অ্যাপ রান করার কমান্ড
CMD ["python", "./app.py"]
```

### Flask App রান করার কমান্ড:

```bash
docker build -t flask-app .
docker run -p 8000:5000 flask-app
```

এখানে:
- `8000` হচ্ছে কম্পিউটারের port
- `5000` হচ্ছে Flask অ্যাপের ভিতরের port

---

## 🧠 কিছু দরকারি Docker কমান্ড

```bash
docker images             # সব image এর লিস্ট
docker ps                 # এখন চালু থাকা container গুলা
docker ps -a              # সব container (চালু বা বন্ধ)
docker run -it ubuntu     # ইন্টার‌্যাকটিভ টার্মিনালে Ubuntu container চালানো
docker run -d image_name  # detached mode এ চালানো
docker start name         # container চালু করা
docker stop name          # container বন্ধ করা
docker rm name            # container ডিলিট করা
docker rmi image_name     # image ডিলিট করা
docker logs container     # container এর লগ দেখা
docker exec -it ID /bin/bash   # container এর ভিতরে ঢোকা
```

---

## 🌍 Docker Network

```bash
docker network ls
docker network create my_network

docker run -d   -p 27017:27017   --name mongo   --network my_network   -e MONGO_INITDB_ROOT_USERNAME=admin   -e MONGO_INITDB_ROOT_PASSWORD=abc   mongo
```

---

## 📦 Docker Volume

```bash
docker volume ls
docker volume create my_volume
docker volume rm my_volume
docker volume prune

# Example:
docker run -v /host/path:/container/path image_name
```

---

## 🧩 Docker Compose

Docker Compose দিয়ে একাধিক সার্ভিস একসাথে চালানো যায়।

```bash
docker compose -f name.yaml up -d   # চালানো
docker compose -f name.yaml down    # বন্ধ করা
```

---

