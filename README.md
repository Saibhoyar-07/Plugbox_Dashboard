# PlugBox EV Charging Management System

A comprehensive EV charging station management platform with vendor dashboards, charger management, real-time monitoring, and role-based access control.

## 🚀 Quick Start

### Prerequisites
- Node.js 16+ 
- PostgreSQL 16 (recommended) or SQLite (for development)
- Git
- Mosquitto MQTT Broker (for real-time features)

### 1. Clone Repository
```bash
git clone <repository-url>
cd plugbox
```

### 2. Install Dependencies

**Dashboard Backend:**
```bash
cd dashboard/Backend
npm install
```

**Dashboard Frontend:**
```bash
cd dashboard/Frontend  
npm install
```

**Main Backend (if needed):**
```bash
cd backend
npm install
```

### 3. Environment Setup

Create `.env` file in `dashboard/Backend/`:
```env
# Server Configuration
PORT=5002
NODE_ENV=development

# Database Configuration (SQLite - same as main backend)
DATABASE_URL="file:../../backend/prisma/dev.db"

# JWT Configuration
JWT_SECRET=your_dashboard_jwt_secret_key_here

# Frontend URL
FRONTEND_URL=http://localhost:3000
```

### 4. Database Setup

#### Option A: PostgreSQL (Recommended)
1. Install PostgreSQL 16
2. Create database: `CREATE DATABASE plugbox;`
3. Update `prisma/schema.prisma` with your credentials:
   ```prisma
   datasource db {
     provider = "postgresql"
     url      = "postgresql://username:password@localhost:5432/plugbox?schema=public"
   }
   ```
4. Generate Prisma client:
   ```bash
   cd dashboard/Backend
   npx prisma generate
   npx prisma db push
   ```

#### Option B: SQLite (Development)
1. Update `prisma/schema.prisma`:
   ```prisma
   datasource db {
     provider = "sqlite"
     url      = "file:./dev.db"
   }
   ```
2. Generate Prisma client:
   ```bash
   cd dashboard/Backend
   npx prisma generate
   npx prisma db push
   ```

### 5. Initialize Data

Run setup scripts to create admin user and sample data:
```bash
cd dashboard/Backend

# Create admin user
node create-admin.js

# Create test vendor and chargers
node create-test-user.js
node add-sample-chargers.js

# Assign chargers to vendor (if needed)
node check-vendor-chargers.js
```

## 🚀 Starting the Servers

### Method 1: Using Automated Scripts (Recommended)

**Windows PowerShell:**
```powershell
# Restart all servers
.\dashboard\restart-all-windows.ps1
```

**Windows Batch:**
```batch
# Start development servers
.\dashboard\start-dev.bat

# Auto-setup with verification
.\dashboard\auto-setup.bat
```

**Linux/Mac:**
```bash
# Make executable and run
chmod +x dashboard/start-servers.sh
./dashboard/start-servers.sh

# Restart all servers
chmod +x dashboard/restart-all.sh
./dashboard/restart-all.sh
```

### Method 2: Manual Start

**Backend Server:**
```bash
cd dashboard/Backend
npm run dev
# or
npm start
```

**Frontend Server:**
```bash
cd dashboard/Frontend
PORT=3000 npm start
# or for port 3002
PORT=3002 npm start
```

## 🌐 Access Points

- **Frontend**: http://localhost:3000 (or http://localhost:3002)
- **Backend API**: http://localhost:5002
- **Health Check**: http://localhost:5002/health

## 🔑 Default Credentials

### Admin Access (Full System Access)
```
📧 Email: admin@plugbox.com
🔑 Password: password123
🎯 Access: ALL CHARGERS
📊 Features: Full system oversight
```

### Vendor Access (Limited to Own Chargers)
```
📧 Email: testvendor@plugbox.com
🔑 Password: password123
🎯 Access: VENDOR CHARGERS ONLY
📊 Features: Manage own chargers
```

## 📊 Dashboard Features

### Admin Dashboard
- **Full System Overview**: View all chargers across all vendors
- **Vendor Management**: Add, edit, and manage vendors
- **Charger Management**: Complete control over all charging stations
- **Real-time Monitoring**: Live status updates via WebSocket
- **User Management**: Admin and vendor account management

### Vendor Dashboard
- **Charger Management**: Add, edit, and delete own chargers
- **Real-time Status**: Live monitoring of charger availability
- **Session Tracking**: Monitor charging sessions
- **Analytics**: Usage statistics and revenue tracking
- **Location Management**: Manage charging station locations

## 🔧 API Endpoints

### Authentication
- `POST /api/auth/login` - Admin login
- `POST /api/vendor/auth/login` - Vendor login

### Vendor Management
- `GET /api/vendor/chargers` - Get vendor's chargers
- `POST /api/vendor/chargers` - Create new charger
- `PUT /api/vendor/chargers/:id` - Update charger
- `DELETE /api/vendor/chargers/:id` - Delete charger
- `GET /api/vendor/dashboard` - Vendor dashboard data

### Admin Management
- `GET /api/chargers` - Get all chargers (admin only)
- `GET /api/admin/vendors` - Get all vendors
- `GET /api/admin/dashboard` - Admin dashboard data
- `POST /api/admin/vendors` - Create new vendor

### Health & Debug
- `GET /health` - Server health check
- `GET /api/debug/status` - Debug information

## 🛠 Development Commands

### Backend Development
```bash
cd dashboard/Backend

# Start with nodemon (auto-restart)
npm run dev

# Start normally
npm start

# Database operations
npx prisma generate
npx prisma db push
npx prisma studio
```

### Frontend Development
```bash
cd dashboard/Frontend

# Start development server
npm start

# Build for production
npm run build

# Run tests
npm test
```

### Database Management
```bash
cd dashboard/Backend

# View database
npx prisma studio

# Reset database
npx prisma migrate reset

# Generate migrations
npx prisma migrate dev
```

## 📁 Project Structure

```
plugbox/
├── dashboard/
│   ├── Backend/              # Node.js/Express API
│   │   ├── controllers/      # Route handlers
│   │   ├── models/          # Database models
│   │   ├── routes/          # API routes
│   │   ├── middleware/      # Auth/validation
│   │   ├── services/        # Business logic
│   │   ├── src/            # Source code
│   │   │   ├── mqtt/       # MQTT client
│   │   │   └── websocket/  # WebSocket server
│   │   ├── prisma/         # Database schema
│   │   └── scripts/        # Setup scripts
│   └── Frontend/           # React application
│       ├── src/
│       │   ├── components/   # Reusable components
│       │   ├── pages/       # Page components
│       │   ├── contexts/    # React contexts
│       │   └── services/    # API services
│       └── public/         # Static assets
├── backend/                # Alternative backend
├── android/               # Mobile app
└── docs/                 # Documentation
```

## 🔄 Data Flow

1. **Authentication**: Login → JWT Token → Authenticated Requests
2. **Role-Based Access**: Admin → Full Access, Vendor → Limited Access
3. **Real-time Updates**: MQTT/WebSocket for live charger status
4. **Database**: Prisma ORM with SQLite/PostgreSQL
5. **API Communication**: RESTful APIs with proper error handling

## 🚨 Common Issues & Solutions

### Issue: "No chargers found" 
**Cause**: Chargers not assigned to vendor or wrong API endpoint
**Solution**: 
```bash
cd dashboard/Backend
node check-vendor-chargers.js
```

### Issue: Database connection errors
**Cause**: Incorrect DATABASE_URL or database not running
**Solution**: 
1. Verify database connection string in `.env`
2. Run `npx prisma db push` to sync schema
3. Check database server status

### Issue: Authentication errors
**Cause**: Missing or invalid JWT token
**Solution**: 
1. Login again to get fresh token
2. Check token expiration (7 days)
3. Verify JWT_SECRET in `.env`

### Issue: Port conflicts
**Cause**: Ports 5002 or 3000/3002 already in use
**Solution**: 
```bash
# Kill existing processes (Windows)
taskkill /F /IM node.exe

# Kill existing processes (Linux/Mac)
pkill -f node

# Or use different ports
PORT=5003 npm run dev
PORT=3001 npm start
```

### Issue: MQTT connection failed
**Cause**: Mosquitto broker not running
**Solution**: 
```bash
# Install and start Mosquitto
# Windows: Download from https://mosquitto.org/
# Linux: sudo apt install mosquitto mosquitto-clients
# Mac: brew install mosquitto
```

## 🧪 Testing

### API Testing
```bash
cd dashboard/Backend

# Test authentication
node test-final-api.js

# Test vendor endpoints
node test-vendor-endpoints.js

# Test admin access
node test-admin-access.js
```

### Frontend Testing
```bash
cd dashboard/Frontend

# Run unit tests
npm test

# Run integration tests
npm run test:integration
```

## 📈 Monitoring & Debugging

### Server Logs
- **Backend**: Console output shows connection status, API calls, and errors
- **Frontend**: Browser console shows API requests and component errors

### Health Checks
```bash
# Check backend health
curl http://localhost:5002/health

# Check API endpoints
curl http://localhost:5002/api/chargers
curl http://localhost:5002/api/vendor/chargers
```

### Database Debug
```bash
cd dashboard/Backend

# Open Prisma Studio
npx prisma studio

# Check database connection
node check-vendors.js
node check-users.js
```

## 🔒 Security Features

- **JWT Authentication**: 7-day token expiration
- **Role-Based Access**: Admin vs vendor permissions
- **Rate Limiting**: 100 requests per 15 minutes per IP
- **CORS Protection**: Configured for frontend domains
- **Helmet Security**: Security headers middleware
- **Password Hashing**: bcrypt for secure password storage

## 🚀 Production Deployment

### Environment Variables
```env
NODE_ENV=production
PORT=5002
JWT_SECRET=your_production_jwt_secret
DATABASE_URL=your_production_database_url
FRONTEND_URL=https://yourdomain.com
```

### Build Commands
```bash
# Frontend production build
cd dashboard/Frontend
npm run build

# Backend production
cd dashboard/Backend
npm start
```

### Process Management
```bash
# Use PM2 for process management
npm install -g pm2
pm2 start dashboard/Backend/server.js --name "plugbox-backend"
pm2 start dashboard/Frontend/build --name "plugbox-frontend"
```

## 🤝 Contributing

1. Fork repository
2. Create feature branch: `git checkout -b feature/new-feature`
3. Make changes and test thoroughly
4. Commit changes: `git commit -am 'Add new feature'`
5. Push to branch: `git push origin feature/new-feature`
6. Submit pull request

## 📝 License

MIT License - see LICENSE file for details

## 🆘 Support

For issues:
1. Check this README first
2. Review logs in browser console and backend
3. Check server health endpoints
4. Create GitHub issue with details
5. Include error logs and environment details

---

**Note**: This application is continuously evolving. Check for updates regularly and ensure all dependencies are up to date.

**Last Updated**: 2025-04-19
