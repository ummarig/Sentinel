# Installation Guide

This guide covers all supported installation methods for Sentinel. Choose the method that best fits your needs.

## Prerequisites

| Requirement             | Minimum Version | Recommended          |
| ----------------------- | --------------- | -------------------- |
| Node.js                 | v20.x           | Latest LTS (v22.x)   |
| PostgreSQL              | v15             | v16+                 |
| Docker & Docker Compose | Latest          | Latest               |
| npm                     | v10+            | Bundled with Node.js |
| Git                     | v2+             | Latest               |

### Verify Prerequisites

```bash
node --version
npm --version
psql --version
docker --version
docker-compose --version
git --version
```

---

## Method 1: Docker (Recommended)

The Docker setup provides the fastest path to a working development environment with zero system-level dependency installation.

### Steps

1. **Clone the repository:**

   ```bash
   git clone https://github.com/sentinel-security-productions/Sentinel.git
   cd Sentinel
   ```

2. **Start all services:**

   ```bash
   npm run docker:up
   ```

3. **Run database migrations:**

   ```bash
   npm run docker:db:migrate
   ```

4. **Verify services are running:**

   ```bash
   npm run docker:ps
   ```

   You should see `sentinel-backend`, `sentinel-postgres`, and `sentinel-redis` with `healthy` status.

5. **Access the application:**

   | Service     | URL                   |
   | ----------- | --------------------- |
   | Backend API | http://localhost:3000 |
   | PostgreSQL  | localhost:5432        |
   | Redis       | localhost:6379        |

### Useful Commands

```bash
npm run docker:logs        # View service logs
npm run docker:down        # Stop services
npm run docker:restart     # Restart all services
npm run docker:db:studio   # Open Prisma Studio (GUI for database)
npm run docker:clean       # Remove containers, networks, and volumes
```

See [DOCKER.md](../DOCKER.md) for the complete Docker documentation.

---

## Method 2: Local Installation

Use this method if you prefer running services directly on your machine.

### 1. Clone the Repository

```bash
git clone https://github.com/sentinel-security-productions/Sentinel.git
cd Sentinel
```

### 2. Install Dependencies

```bash
npm install
```

### 3. Set Up PostgreSQL

Create a database and user:

```bash
# Connect to PostgreSQL as superuser
psql -U postgres

# Create database and user
CREATE DATABASE sentinel_db;
CREATE USER sentinel_user WITH PASSWORD 'sentinel_password';
GRANT ALL PRIVILEGES ON DATABASE sentinel_db TO sentinel_user;
\q
```

### 4. Configure Environment

```bash
cp .env.example .env
```

Edit `.env` with your database credentials:

```env
DATABASE_URL="postgresql://sentinel_user:sentinel_password@localhost:5432/sentinel_db?schema=public"
```

See [Environment Variables](#environment-variables) for the full list of variables.

### 5. Run Database Migrations

```bash
npx prisma migrate dev
```

### 6. Start the Development Server

```bash
npm run dev
```

The API will be available at http://localhost:3000/api.

---

## Method 3: Build from Source

For production deployment or custom builds:

```bash
# Install dependencies
npm install

# Build TypeScript
npm run build

# Generate Prisma client
npm run prisma:generate

# Run migrations
npm run prisma:migrate

# Start production server
npm start
```

---

## Environment Variables

Create a `.env` file in the project root. Copy `.env.example` to `.env` as a starting point.

### Core Variables

| Variable            | Description                  | Required | Default       |
| ------------------- | ---------------------------- | -------- | ------------- |
| `NODE_ENV`          | Environment mode             | No       | `development` |
| `PORT`              | API server port              | No       | `3000`        |
| `DATABASE_URL`      | PostgreSQL connection string | Yes      | —             |
| `DATABASE_HOST`     | Database host                | Yes\*    | `localhost`   |
| `DATABASE_PORT`     | Database port                | Yes\*    | `5432`        |
| `DATABASE_USER`     | Database username            | Yes\*    | —             |
| `DATABASE_PASSWORD` | Database password            | Yes\*    | —             |
| `DATABASE_NAME`     | Database name                | Yes\*    | —             |

### Notification Variables

| Variable              | Description                    | Required |
| --------------------- | ------------------------------ | -------- |
| `DISCORD_WEBHOOK_URL` | Discord webhook URL for alerts | Yes      |
| `TELEGRAM_BOT_TOKEN`  | Telegram bot access token      | Yes      |
| `TELEGRAM_CHAT_ID`    | Telegram target chat ID        | Yes      |

### Observability Variables

| Variable            | Description                  | Required |
| ------------------- | ---------------------------- | -------- | ----------------------- |
| `OTEL_ENABLED`      | Enable OpenTelemetry tracing | No       | `false`                 |
| `OTEL_EXPORTER_URL` | OTLP collector endpoint      | No       | `http://localhost:4318` |
| `OTEL_SERVICE_NAME` | Service name for traces      | No       | `sentinel`              |

### Blockchain RPC Variables

| Variable              | Description                  | Required |
| --------------------- | ---------------------------- | -------- |
| `ETHEREUM_RPC_URL`    | Ethereum JSON-RPC endpoint   | Yes      |
| `ETHEREUM_WS_URL`     | Ethereum WebSocket endpoint  | Yes      |
| `STELLAR_HORIZON_URL` | Stellar Horizon RPC endpoint | Yes      |

\* Either `DATABASE_URL` or the individual `DATABASE_*` variables are required.

---

## Database Setup

### Using Prisma Migrations (Recommended)

```bash
# Create and apply migrations from schema changes
npx prisma migrate dev

# Apply existing migrations (production)
npx prisma migrate deploy

# Reset database (development only)
npx prisma migrate reset
```

### Using Prisma Studio

```bash
npx prisma studio
```

Opens a web-based database GUI at http://localhost:5555.

---

## Verification

After installation, verify everything works:

```bash
# 1. Check API health
curl http://localhost:3000/api

# 2. Start the bot (mempool monitoring)
npm run dev

# 3. Run linter
npm run lint

# 4. Run tests
npm test
```

---

## Troubleshooting

### Port Already in Use

```bash
# Find process using port 3000
lsof -i :3000
# or on Windows
netstat -ano | findstr :3000
```

Change the port in `.env`:

```env
PORT=3001
```

### Database Connection Failed

```bash
# Verify PostgreSQL is running
pg_isready -h localhost -p 5432

# Check DATABASE_URL format
echo $DATABASE_URL
# Expected: postgresql://user:password@host:port/database
```

### Prisma Client Not Generated

```bash
# Regenerate client
npx prisma generate
```

### Docker: Services Unhealthy

```bash
# View logs
npm run docker:logs

# Restart services
npm run docker:restart
```

---

## Common Next Steps

| Task                    | Guide                                                        |
| ----------------------- | ------------------------------------------------------------ |
| Configure alerts        | [Alert Configuration Guide](../docs/alerts/configuration.md) |
| Set up signatures       | [Signatures Guide](../docs/signatures/danger-signatures.md)  |
| Deploy to production    | [Docker Guide](../DOCKER.md#production-considerations)       |
| Understand architecture | [Architecture Overview](../ARCHITECTURE.md)                  |

---

## Getting Help

- **Documentation**: Review the docs in the [docs/](../docs/) directory
- **Issues**: [Open a bug report](https://github.com/sentinel-security-productions/Sentinel/issues/new?template=bug_report.yml)
- **Discussions**: [GitHub Discussions](https://github.com/sentinel-security-productions/Sentinel/discussions)
