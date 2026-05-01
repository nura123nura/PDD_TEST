# PDD Test — Қазақша жол ережесі тесті және ИИ чат

Node.js, PostgreSQL және Google Gemini API негізіндегі веб-қолданба. Пайдаланушылар қазақ тіліндегі PDD сұрақтарын шеше алады және ИИ чат арқылы жол ережесіне байланысты сұрақтар қоя алады.

---

## Мазмұны

- [Технологиялар](#технологиялар)
- [Жоба құрылымы](#жоба-құрылымы)
- [Жергілікті іске қосу](#жергілікті-іске-қосу)
- [Docker арқылы іске қосу](#docker-арқылы-іске-қосу)
- [Мониторинг](#мониторинг)
- [Инфрақұрылым](#инфрақұрылым)

---

## Технологиялар

**Backend:** Node.js 24, Express 4, PostgreSQL 15  
**Frontend:** HTML, CSS, Vanilla JS  
**ИИ:** Google Gemini API (models/gemini-2.5-flash)  
**Мониторинг:** Prometheus, Grafana, AlertManager, Node Exporter  
**CI/CD:** Jenkins  
**Автоматизация:** n8n  
**Контейнер:** Docker, Docker Compose  
**Инфрақұрылым:** Ansible, Terraform  

---

## Жоба құрылымы

```
PDD/
├── index.html          # Басты бет: тіркелу модалы, жалпы статистика
├── test.html           # Тест беті: 100 PDD сұрақ, жасыл/қызыл кері байланыс
├── ask.html            # ИИ чат беті: пайдаланушы сұрақтары
├── style.css           # Dark тақырыпты дизайн
├── app.js              # Frontend логикасы (аутентификация, тест, чат UI)
├── server.js           # Express сервері, Gemini API проксиі, метрика
├── db.js               # PostgreSQL қосылысы
├── BDPDD.sql           # Дерекқор схемасы
├── package.json        # npm тәуелділіктері
├── Dockerfile          # Қолданба Docker бейнесі
├── docker-compose.yml  # Барлық сервистер (app, postgres, monitoring, CI/CD)
├── prometheus.yml      # Prometheus конфигурациясы
├── alert.rules.yml     # Prometheus дабыл ережелері
├── alertmanager.yml    # AlertManager конфигурациясы
├── ansible/            # Ansible playbook (сервер конфигурациясы)
└── terraform/          # Terraform инфрақұрылым конфигурациясы
```

---

## Жергілікті іске қосу

### Алғышарттар

- Node.js 18+ орнатылған
- PostgreSQL 15+ орнатылған және іске қосылған
- Google Gemini API кілті

### Қадамдар

1. Репозиторийді клондаңыз:

```bash
git clone https://github.com/<username>/PDD.git
cd PDD
```

2. Тәуелділіктерді орнатыңыз:

```bash
npm install
```

3. `.env` файлын жасаңыз (үлгіге негізделіп):

```bash
cp .env.txt .env
```

`.env` файлын өзіңіздің мәндеріңізбен толтырыңыз:

```env
DB_HOST=localhost
DB_PORT=5432
DB_USER=postgres
DB_PASSWORD=your_password
DB_NAME=pdd_users
GOOGLE_API_KEY=your_google_api_key
GEMINI_MODEL=models/gemini-2.5-flash
PORT=3000
```

4. Дерекқор схемасын қолданыңыз:

```bash
psql -U postgres -d pdd_users -f BDPDD.sql
```

5. Серверді іске қосыңыз:

```bash
npm start
```

Қолданба `http://localhost:3000` мекенжайында қолжетімді болады.

---

## Docker арқылы іске қосу

### Алғышарттар

- Docker және Docker Compose орнатылған

### Барлық сервистерді іске қосу

```bash
docker-compose up --build
```

Іске қосқаннан кейін қолжетімді болатын мекенжайлар:

| Сервис         | Мекенжай                   |
|----------------|----------------------------|
| Қолданба       | http://localhost:3002       |
| Grafana        | http://localhost:3001       |
| Prometheus     | http://localhost:9090       |
| AlertManager   | http://localhost:9093       |
| Node Exporter  | http://localhost:9100       |
| Jenkins        | http://localhost:8081       |
| n8n            | http://localhost:5678       |
| PostgreSQL     | localhost:5433              |

### Тек негізгі сервистерді іске қосу

```bash
docker-compose up app postgres
```

---

## Мониторинг

Жоба Prometheus + Grafana мониторинг жинағымен жабдықталған:

- `prometheus.yml` — метрика жинау конфигурациясы
- `alert.rules.yml` — дабыл шарттары (CPU, жад, сервис статусы)
- `alertmanager.yml` — дабыл жіберу баптаулары

Grafana дашбордын ашу үшін: `http://localhost:3001`  
Әдепкі кіру: `admin / admin`

---

## Инфрақұрылым

### Ansible

Сервер конфигурациясын автоматтандыру:

```bash
ansible-playbook -i ansible/inventory.ini ansible/playbook.yml
```

### Terraform

Инфрақұрылым ресурстарын басқару:

```bash
cd terraform
terraform init
terraform plan
terraform apply
```
