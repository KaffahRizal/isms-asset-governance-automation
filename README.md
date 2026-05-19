# ISMS Asset Governance Automation

Automated workflow solution untuk manajemen aset dan tata kelola keamanan informasi (ISMS - Information Security Management System) menggunakan n8n.

## 📋 Daftar Isi

- [Ringkasan Proyek](#ringkasan-proyek)
- [Tujuan Proyek](#tujuan-proyek)
- [Fitur Utama](#fitur-utama)
- [Arsitektur Sistem](#arsitektur-sistem)
- [Teknologi yang Digunakan](#teknologi-yang-digunakan)
- [Prasyarat](#prasyarat)
- [Instalasi](#instalasi)
- [Konfigurasi](#konfigurasi)
- [Workflow Utama](#workflow-utama)
- [Penggunaan](#penggunaan)
- [API Integration](#api-integration)
- [Monitoring & Logging](#monitoring--logging)
- [Troubleshooting](#troubleshooting)
- [Kontribusi](#kontribusi)
- [Lisensi](#lisensi)

## 🎯 Ringkasan Proyek

**ISMS Asset Governance Automation** adalah solusi otomasi workflow berbasis n8n yang dirancang untuk mengintegrasikan, mengelola, dan mengotomatisasi proses tata kelola aset dalam konteks keamanan informasi. Proyek ini memfasilitasi sinkronisasi data aset, validasi compliance, dan pelaporan otomatis untuk mendukung kepatuhan terhadap standar keamanan informasi internasional.

## 🎪 Tujuan Proyek

1. **Otomasi Proses Manajemen Aset** - Mengotomatiskan siklus hidup aset dari onboarding hingga offboarding
2. **Peningkatan Kepatuhan** - Memastikan semua aset mematuhi kebijakan keamanan dan standar ISMS
3. **Efisiensi Operasional** - Mengurangi beban kerja manual melalui otomasi workflow
4. **Visibilitas Real-time** - Memberikan dashboard dan laporan yang akurat untuk monitoring aset
5. **Integrasi Sistem** - Menghubungkan berbagai sistem informasi untuk data yang konsisten
6. **Audit Trail** - Mencatat semua aktivitas untuk keperluan audit dan compliance

## ⭐ Fitur Utama

### 1. Manajemen Aset Terpusat
- Inventarisasi aset secara otomatis
- Kategorisasi aset berdasarkan tipe dan tingkat risiko
- Tracking status dan lokasi aset

### 2. Validasi Compliance
- Pemeriksaan otomatis terhadap standar keamanan
- Identifikasi deviasi dan non-compliance
- Notifikasi untuk aset yang tidak compliant

### 3. Sinkronisasi Data Multi-Sistem
- Integrasi dengan CMDB (Configuration Management Database)
- Sinkronisasi dengan sistem inventory existing
- Update data real-time ke multiple endpoints

### 4. Otomasi Workflow
- Approval process otomatis untuk request aset baru
- Lifecycle management automation
- Scheduled maintenance dan update

### 5. Reporting & Analytics
- Dashboard real-time untuk monitoring aset
- Laporan compliance berkala
- Analisis tren dan risk assessment

### 6. Notification & Escalation
- Alert untuk expired asset atau compliance issue
- Email notification otomatis
- Escalation untuk critical issues

## 🏗️ Arsitektur Sistem

```
┌─────────────────────────────────────────────────────────────┐
│                    Data Sources                              │
│  (CMDB, Inventory, Employee Directory, etc.)                │
└──────────────────────┬──────────────────────────────────────┘
                       │
                       ▼
┌─────────────────────────────────────────────────────────────┐
│                   n8n Workflow Engine                        │
│  ┌────────────────────────────────────────────────────────┐ │
│  │  • Data Collection & Transformation                    │ │
│  │  • Validation & Compliance Check                       │ │
│  │  • Business Logic Processing                           │ │
│  │  • Error Handling & Retry Mechanism                    │ │
│  └────────────────────────────────────────────────────────┘ │
└──────────┬──────────────────────────────────────────────────┘
           │
    ┌──────┴──────────────┬─────────────────┐
    ▼                     ▼                 ▼
┌─────────────────┐ ┌──────────────┐ ┌──────────────┐
│  Database       │ │ Notification │ │   External  │
│  (Storage)      │ │  (Email/SMS) │ │   Systems   │
└─────────────────┘ └──────────────┘ └──────────────┘
```

## 🛠️ Teknologi yang Digunakan

| Komponen | Teknologi | Versi | Keterangan |
|----------|-----------|-------|-----------|
| **Workflow Engine** | n8n | Latest | Orchestration platform |
| **Database** | PostgreSQL/MySQL | 12+ | Data persistence |
| **Message Queue** | Redis | 6+ | Caching & queue |
| **Containerization** | Docker | 20+ | Container deployment |
| **API Gateway** | Node.js/Express | 14+ | API layer |
| **Monitoring** | Prometheus/Grafana | Latest | Metrics & visualization |
| **Logging** | ELK Stack | Latest | Centralized logging |

## 📋 Prasyarat

Sebelum memulai, pastikan Anda memiliki:

- **Node.js** >= 14.x
- **Docker** & **Docker Compose** >= 20.x
- **PostgreSQL** >= 12 atau MySQL >= 5.7
- **Git** untuk version control
- **Access token** untuk API eksternal yang diperlukan
- **n8n** >= v0.200.0

### System Requirements
- **RAM**: Minimum 4GB untuk development, 8GB+ untuk production
- **Storage**: Minimum 50GB untuk database dan logs
- **CPU**: 2 cores minimum
- **Network**: Access ke internet untuk external integrations

## 🚀 Instalasi

### 1. Clone Repository
```bash
git clone https://github.com/KaffahRizal/isms-asset-governance-automation.git
cd isms-asset-governance-automation
```

### 2. Setup Environment Variables
```bash
cp .env.example .env
# Edit .env dengan konfigurasi lokal Anda
```

### 3. Install Dependencies
```bash
npm install
```

### 4. Start Services dengan Docker Compose
```bash
docker-compose up -d
```

Atau untuk development:
```bash
docker-compose -f docker-compose.dev.yml up
```

### 5. Initialize Database
```bash
npm run db:migrate
npm run db:seed
```

### 6. Access n8n
- **URL**: http://localhost:5678
- **Default User**: admin@example.com
- **Default Password**: [Check .env file]

## ⚙️ Konfigurasi

### Environment Variables (.env)

```env
# n8n Configuration
N8N_HOST=0.0.0.0
N8N_PORT=5678
N8N_PROTOCOL=http
N8N_SECURE_COOKIE=false

# Database Configuration
DB_TYPE=postgres
DB_HOST=localhost
DB_PORT=5432
DB_NAME=isms_governance
DB_USER=isms_user
DB_PASSWORD=secure_password

# Redis Configuration
REDIS_HOST=localhost
REDIS_PORT=6379
REDIS_DB=0

# API Configuration
API_BASE_URL=http://localhost:3000
API_PORT=3000
JWT_SECRET=your_jwt_secret_key

# Email Configuration
SMTP_HOST=smtp.gmail.com
SMTP_PORT=587
SMTP_USER=your_email@gmail.com
SMTP_PASSWORD=your_email_password
SMTP_FROM=noreply@isms.example.com

# External APIs
CMDB_API_KEY=your_cmdb_api_key
INVENTORY_API_KEY=your_inventory_api_key

# Logging
LOG_LEVEL=info
LOG_FORMAT=json
```

### Workflow Configuration
Setiap workflow dapat dikonfigurasi melalui n8n UI atau file konfigurasi:

```json
{
  "workflowId": "asset_sync_workflow",
  "enabled": true,
  "schedule": "0 */6 * * *",
  "retryPolicy": {
    "maxAttempts": 3,
    "backoffMultiplier": 2
  },
  "notifications": {
    "onSuccess": true,
    "onFailure": true,
    "recipients": ["admin@example.com"]
  }
}
```

## 🔄 Workflow Utama

### 1. Asset Synchronization Workflow
**Purpose**: Sinkronisasi data aset dari multiple sources ke database pusat

**Schedule**: Setiap 6 jam
**Trigger**: Manual atau scheduled

**Flow**:
1. Fetch data dari CMDB API
2. Fetch data dari Inventory System
3. Transform dan normalize data
4. Detect duplicates dan merge
5. Validate terhadap compliance rules
6. Update ke database pusat
7. Send notification untuk changes

**Error Handling**: Automatic retry dengan exponential backoff

---

### 2. Compliance Validation Workflow
**Purpose**: Validasi compliance aset terhadap kebijakan keamanan

**Schedule**: Setiap hari pada jam 02:00 UTC
**Trigger**: On-demand atau scheduled

**Flow**:
1. Retrieve semua aset dari database
2. Apply compliance rules engine
3. Check antivirus status, update OS, encryption
4. Identify non-compliant assets
5. Categorize by severity (Critical/High/Medium/Low)
6. Generate compliance report
7. Send alerts untuk critical issues
8. Create tickets untuk remediation

---

### 3. Asset Lifecycle Workflow
**Purpose**: Mengelola siklus hidup aset dari onboarding hingga offboarding

**Trigger**: Manual request atau automatic based on policies

**Stages**:
- **Onboarding**: Registrasi aset baru, initial setup
- **Active**: Monitoring dan maintenance
- **Maintenance**: Update, patch, security fixes
- **Deprecation**: Planned retirement
- **Offboarding**: Data wipe, disposal tracking

---

### 4. Notification & Escalation Workflow
**Purpose**: Mengirim notifikasi dan escalation sesuai severity level

**Trigger**: Event-based dari workflow lain

**Recipients**:
- Admin team untuk critical alerts
- Managers untuk high priority issues
- Relevant teams untuk medium priority

---

### 5. Reporting Workflow
**Purpose**: Generate laporan compliance dan asset inventory

**Schedule**: Mingguan (Senin 08:00) dan bulanan (Tanggal 1 jam 08:00)
**Output**: PDF reports, CSV exports, email distribution

---

## 📖 Penggunaan

### Menjalankan Workflow Manual
1. Buka n8n dashboard: http://localhost:5678
2. Pilih workflow dari list
3. Klik tombol "Execute Workflow"
4. Monitor progress di Real-time Execution View

### Monitoring Workflow Execution
```bash
# View logs untuk specific workflow
docker logs -f isms-n8n-container

# Check workflow status
curl http://localhost:3000/api/workflows/status
```

### Testing Workflow
```bash
# Run workflow dalam test mode
npm run test:workflow -- --id <workflow_id>

# Run semua tests
npm run test
```

## 🔌 API Integration

### Available API Endpoints

#### Asset Management
```
GET    /api/assets                    - List semua aset
GET    /api/assets/:id                - Get detail aset
POST   /api/assets                    - Create aset baru
PUT    /api/assets/:id                - Update aset
DELETE /api/assets/:id                - Delete aset
```

#### Compliance
```
GET    /api/compliance/report         - Get compliance report
GET    /api/compliance/violations     - List compliance violations
POST   /api/compliance/check          - Trigger compliance check
```

#### Workflows
```
GET    /api/workflows                 - List workflows
POST   /api/workflows/:id/execute     - Execute workflow
GET    /api/workflows/:id/history     - Get execution history
```

### Authentication
Semua API endpoints memerlukan Bearer token:

```bash
curl -H "Authorization: Bearer YOUR_JWT_TOKEN" \
     http://localhost:3000/api/assets
```

## 📊 Monitoring & Logging

### Prometheus Metrics
Akses metrics di: http://localhost:9090

Key metrics:
- `n8n_workflow_executions_total` - Total workflow executions
- `n8n_workflow_execution_duration_seconds` - Execution time
- `n8n_workflow_errors_total` - Total errors

### Grafana Dashboard
Akses dashboard di: http://localhost:3000/grafana

Pre-built dashboards:
- Asset Overview Dashboard
- Compliance Status Dashboard
- Workflow Performance Dashboard
- Error Rate Dashboard

### ELK Stack Logging
Akses Kibana di: http://localhost:5601

Search logs menggunakan index `isms-*`

## 🔧 Troubleshooting

### Common Issues

#### 1. Workflow Execution Timeout
**Gejala**: Workflow terhenti sebelum selesai
**Solusi**:
```bash
# Increase timeout di .env
N8N_WORKFLOW_TIMEOUT=1200

# Restart service
docker-compose restart n8n
```

#### 2. Database Connection Error
**Gejala**: "Unable to connect to database"
**Solusi**:
```bash
# Check database status
docker-compose ps

# Restart database
docker-compose restart db

# Check logs
docker logs isms-postgres
```

#### 3. API Integration Failure
**Gejala**: "401 Unauthorized" atau "Connection refused"
**Solusi**:
- Verify API keys di .env
- Check network connectivity
- Verify endpoint URLs
- Check API rate limits

#### 4. High Memory Usage
**Gejala**: Container frequently restarting
**Solusi**:
```bash
# Increase memory limit di docker-compose.yml
services:
  n8n:
    mem_limit: 4g

# Restart
docker-compose up -d
```

### Debug Mode
```bash
# Enable verbose logging
DEBUG=* npm start

# Run dengan specific log level
LOG_LEVEL=debug docker-compose up
```

### Health Check
```bash
# Check n8n health
curl http://localhost:5678/api/v1/health

# Check API health
curl http://localhost:3000/health

# Check all services
docker-compose ps
```

## 🤝 Kontribusi

Kami menerima kontribusi! Silakan ikuti langkah berikut:

1. Fork repository
2. Buat branch untuk fitur Anda (`git checkout -b feature/amazing-feature`)
3. Commit changes (`git commit -m 'Add amazing feature'`)
4. Push ke branch (`git push origin feature/amazing-feature`)
5. Buka Pull Request

### Coding Standards
- Follow ESLint configuration
- Write tests untuk fitur baru
- Update documentation
- Follow conventional commits

## 📝 Lisensi

Proyek ini dilisensikan di bawah [MIT License](LICENSE)

## 📞 Support & Contact

- **Issues**: [GitHub Issues](https://github.com/KaffahRizal/isms-asset-governance-automation/issues)
- **Email**: KaffahRizal@example.com
- **Documentation**: [Wiki](https://github.com/KaffahRizal/isms-asset-governance-automation/wiki)

---

**Last Updated**: 2026-05-19
**Maintainer**: KaffahRizal
