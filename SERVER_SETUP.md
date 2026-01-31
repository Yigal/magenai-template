# MagenAI Dual Server Setup

## Overview
MagenAI now runs with two synchronized servers:
- **Dashboard**: React Vite dev server on port 4010
- **API**: Express.js REST API server on port 4011 (+1 from dashboard)

## Quick Start

### Start Both Servers (Recommended)
```bash
cd chrome_app
npm run dev-all
```

This runs both servers concurrently using the `concurrently` package.

### Start Individual Servers

**Dashboard only:**
```bash
cd chrome_app
npm run dev
```
Access at: http://localhost:4010

**API only:**
```bash
cd chrome_app
npm run api
```
Access at: http://localhost:4011

## Server Configuration

### Dashboard Server (Port 4010)
- **Type**: Vite React development server
- **Port**: 4010
- **Host**: 0.0.0.0 (accessible from any network interface)
- **Hot Reload**: Enabled
- **Features**: React 19, TypeScript, hot module replacement

### API Server (Port 4011)
- **Type**: Express.js REST API
- **Port**: 4011
- **Host**: 0.0.0.0 (accessible from any network interface)
- **CORS**: Enabled for cross-origin requests
- **Framework**: Express 4.22.1

## API Endpoints

### 1. Health Check
```bash
GET http://localhost:4011/health
```
**Response:**
```json
{
  "status": "ok",
  "service": "MagenAI API Server",
  "port": 4011,
  "dashboard": "http://localhost:4010",
  "timestamp": "2026-01-31T21:53:11.135Z"
}
```

### 2. Get App Configuration
```bash
GET http://localhost:4011/api/config
```
**Response:**
```json
{
  "version": "1.2",
  "appName": "MagenAI",
  "description": "Chrome Extension that protects sensitive data from being sent to AI models",
  "features": ["Data interception", "Security monitoring", "ChatGPT integration"]
}
```

### 3. Get Server Status
```bash
GET http://localhost:4011/api/status
```
**Response:**
```json
{
  "apiStatus": "running",
  "apiPort": 4011,
  "dashboardPort": 4010,
  "uptime": 19.843375041,
  "timestamp": "2026-01-31T21:53:25.272Z"
}
```

### 4. Send Data
```bash
POST http://localhost:4011/api/data
Content-Type: application/json

{
  "action": "test_action",
  "data": { "key": "value" }
}
```

## Configuration Files

### vite.config.ts
```typescript
server: {
  port: 4010,
  host: '0.0.0.0',
}
```

### config.json
Complete configuration for both servers with all ports and API endpoints.

### .env.development
```
VITE_API_URL=http://localhost:4011
VITE_API_BASE_PATH=/api
VITE_DASHBOARD_PORT=4010
VITE_API_PORT=4011
```

## API Client Usage

### Using the API Client in React Components

```typescript
import { apiClient } from './api';

// Check if API is available
const health = await apiClient.health();

// Get configuration
const config = await apiClient.getConfig();

// Get server status
const status = await apiClient.getStatus();

// Send data to API
const result = await apiClient.sendData('action_name', { data: 'value' });
```

## Port Configuration

| Component | Port | Type | Protocol |
|-----------|------|------|----------|
| Dashboard | 4010 | Vite Dev | HTTP |
| API | 4011 | Express | HTTP |

**Note**: Port 4011 is automatically +1 from the dashboard port (4010).

## Testing the Setup

### Manual Testing with curl

```bash
# Test API health
curl http://localhost:4011/health

# Test API config
curl http://localhost:4011/api/config

# Test API status
curl http://localhost:4011/api/status

# Send data to API
curl -X POST http://localhost:4011/api/data \
  -H "Content-Type: application/json" \
  -d '{"action":"test","data":{"key":"value"}}'
```

### Browser Access

- **Dashboard**: http://localhost:4010
- **API Health**: http://localhost:4011/health
- **API Status**: http://localhost:4011/api/status

## Network Interfaces

Both servers are accessible from:
- **Localhost**: http://localhost:4010 and http://localhost:4011
- **Internal Network**: http://192.168.x.x:4010 and http://192.168.x.x:4011
- **Container/Network Access**: Using the machine's actual IP on these ports

## Environment Variables

### Development (.env.development)
- `VITE_API_URL`: API server URL
- `VITE_API_BASE_PATH`: Base path for API routes
- `VITE_DASHBOARD_PORT`: Dashboard port (4010)
- `VITE_API_PORT`: API port (4011)

## Troubleshooting

### Port Already in Use

If a port is already in use:

```bash
# List processes using port 4010
lsof -i :4010

# Kill process using port 4010
kill -9 <PID>
```

### CORS Issues

CORS is enabled on the API server. If you encounter CORS errors:
1. Ensure both servers are running
2. Check that the API URL in environment variables is correct
3. Verify the dashboard is accessing the API at http://localhost:4011

### Connection Issues

```bash
# Test API connectivity
curl -v http://localhost:4011/health

# Test dashboard connectivity
curl -v http://localhost:4010
```

## npm Scripts

- `npm run dev` - Start dashboard only (port 4010)
- `npm run api` - Start API server only (port 4011)
- `npm run dev-all` - Start both servers concurrently
- `npm run build` - Build for production
- `npm run preview` - Preview production build

## Repository

**GitHub**: https://github.com/Yigal/magenai-template

Latest commits include:
- Dual server setup configuration
- API client implementation
- Environment configuration
- All tests passing

## Next Steps

1. Run `npm run dev-all` to start both servers
2. Access dashboard at http://localhost:4010
3. Test API endpoints at http://localhost:4011
4. Integrate API calls into React components using the `apiClient`
5. Deploy to production with appropriate environment variables
