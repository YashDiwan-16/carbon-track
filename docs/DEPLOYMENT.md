# Carbon Track Deployment Guide

This guide covers deploying Carbon Track to various environments including development, staging, and production.

## 🏗️ Architecture Overview

Carbon Track consists of:
- **Frontend**: Next.js application with React components
- **Backend**: Next.js API routes
- **Database**: MongoDB for application data
- **Blockchain**: Smart contracts on Avalanche network
- **Storage**: Local file system or cloud storage for assets

## 🚀 Quick Deployment Options

### Option 1: Vercel (Recommended for Frontend)
[![Deploy with Vercel](https://vercel.com/button)](https://vercel.com/new/clone?repository-url=https://github.com/your-username/carbon-track)

### Option 2: Railway
[![Deploy on Railway](https://railway.app/button.svg)](https://railway.app/template/railway-template)

### Option 3: Docker
```bash
docker run -p 3000:3000 carbon-track:latest
```

## 📋 Prerequisites

### Development Environment
- Node.js 18+
- pnpm package manager
- MongoDB (local or cloud)
- Git

### Production Environment
- Node.js 18+ runtime
- MongoDB Atlas or managed MongoDB
- Domain name and SSL certificate
- Avalanche RPC access
- Environment variable management

## 🔧 Environment Configuration

### Required Environment Variables

```bash
# Database
MONGODB_URI=mongodb+srv://user:pass@cluster.mongodb.net/carbon-track

# Blockchain
NEXT_PUBLIC_CONTRACT_ADDRESS=0x...
NEXT_PUBLIC_CHAIN_ID=43114  # Mainnet: 43114, Testnet: 43113
NEXT_PUBLIC_RPC_URL=https://api.avax.network/ext/bc/C/rpc

# Application
NODE_ENV=production
NEXTAUTH_URL=https://your-domain.com
NEXTAUTH_SECRET=your-very-long-random-secret

# Optional: Enhanced features
NEXT_PUBLIC_GOOGLE_ANALYTICS=G-XXXXXXXXXX
SENTRY_DSN=https://your-sentry-dsn
```

### Development vs Production Variables

| Variable | Development | Production |
|----------|-------------|------------|
| `NODE_ENV` | `development` | `production` |
| `NEXT_PUBLIC_CHAIN_ID` | `43113` (Fuji) | `43114` (Mainnet) |
| `NEXT_PUBLIC_RPC_URL` | Fuji testnet | Mainnet RPC |
| `MONGODB_URI` | Local MongoDB | MongoDB Atlas |

## 🌐 Platform Deployment Guides

### Vercel Deployment

1. **Connect Repository**
   ```bash
   # Install Vercel CLI
   npm i -g vercel

   # Deploy from project directory
   vercel
   ```

2. **Configure Environment Variables**
   - Go to Vercel Dashboard → Project → Settings → Environment Variables
   - Add all required variables
   - Set different values for Preview vs Production

3. **Build Settings**
   ```json
   {
     "buildCommand": "pnpm build",
     "devCommand": "pnpm dev",
     "installCommand": "pnpm install"
   }
   ```

4. **Domain Configuration**
   - Add custom domain in Vercel dashboard
   - Configure DNS records to point to Vercel

### Railway Deployment

1. **Create Railway Project**
   ```bash
   # Install Railway CLI
   npm install -g @railway/cli

   # Login and deploy
   railway login
   railway init
   railway up
   ```

2. **Configure Services**
   ```yaml
   # railway.json
   {
     "build": {
       "builder": "nixpacks"
     },
     "deploy": {
       "startCommand": "pnpm start",
       "healthcheckPath": "/api/health"
     }
   }
   ```

3. **Environment Variables**
   ```bash
   # Set variables via CLI
   railway variables set MONGODB_URI=mongodb+srv://...
   railway variables set NEXT_PUBLIC_CONTRACT_ADDRESS=0x...
   ```

### Docker Deployment

1. **Dockerfile**
   ```dockerfile
   FROM node:18-alpine AS base

   # Install dependencies only when needed
   FROM base AS deps
   RUN apk add --no-cache libc6-compat
   WORKDIR /app

   COPY package.json pnpm-lock.yaml* ./
   RUN npm install -g pnpm && pnpm install --frozen-lockfile

   # Rebuild the source code only when needed
   FROM base AS builder
   WORKDIR /app
   COPY --from=deps /app/node_modules ./node_modules
   COPY . .

   ENV NEXT_TELEMETRY_DISABLED 1
   RUN npm install -g pnpm && pnpm build

   # Production image, copy all the files and run next
   FROM base AS runner
   WORKDIR /app

   ENV NODE_ENV production
   ENV NEXT_TELEMETRY_DISABLED 1

   RUN addgroup --system --gid 1001 nodejs
   RUN adduser --system --uid 1001 nextjs

   COPY --from=builder /app/public ./public
   COPY --from=builder --chown=nextjs:nodejs /app/.next/standalone ./
   COPY --from=builder --chown=nextjs:nodejs /app/.next/static ./.next/static

   USER nextjs

   EXPOSE 3000
   ENV PORT 3000

   CMD ["node", "server.js"]
   ```

2. **Docker Compose**
   ```yaml
   version: '3.8'
   services:
     app:
       build: .
       ports:
         - "3000:3000"
       environment:
         - MONGODB_URI=mongodb://mongo:27017/carbon-track
         - NEXT_PUBLIC_CONTRACT_ADDRESS=0x...
       depends_on:
         - mongo

     mongo:
       image: mongo:6.0
       ports:
         - "27017:27017"
       volumes:
         - mongo_data:/data/db

   volumes:
     mongo_data:
   ```

3. **Build and Run**
   ```bash
   # Build image
   docker build -t carbon-track .

   # Run with compose
   docker-compose up -d

   # Or run standalone
   docker run -p 3000:3000 -e MONGODB_URI=... carbon-track
   ```

### AWS ECS Deployment

1. **Task Definition**
   ```json
   {
     "family": "carbon-track",
     "networkMode": "awsvpc",
     "requiresCompatibilities": ["FARGATE"],
     "cpu": "512",
     "memory": "1024",
     "executionRoleArn": "arn:aws:iam::account:role/ecsTaskExecutionRole",
     "containerDefinitions": [
       {
         "name": "carbon-track",
         "image": "your-account.dkr.ecr.region.amazonaws.com/carbon-track:latest",
         "portMappings": [
           {
             "containerPort": 3000,
             "protocol": "tcp"
           }
         ],
         "environment": [
           {
             "name": "NODE_ENV",
             "value": "production"
           }
         ],
         "secrets": [
           {
             "name": "MONGODB_URI",
             "valueFrom": "arn:aws:secretsmanager:region:account:secret:carbon-track/mongodb"
           }
         ]
       }
     ]
   }
   ```

## 🗄️ Database Setup

### MongoDB Atlas (Recommended)

1. **Create Cluster**
   - Sign up at [MongoDB Atlas](https://www.mongodb.com/cloud/atlas)
   - Create new cluster (M0 free tier available)
   - Configure network access (allow application IPs)

2. **Create Database User**
   ```bash
   # Create user with read/write access
   username: carbon-track-app
   password: generate-secure-password
   role: readWrite on carbon-track database
   ```

3. **Connection String**
   ```bash
   MONGODB_URI=mongodb+srv://carbon-track-app:password@cluster.mongodb.net/carbon-track?retryWrites=true&w=majority
   ```

### Self-hosted MongoDB

1. **Docker MongoDB**
   ```bash
   docker run -d \
     --name carbon-track-mongo \
     -p 27017:27017 \
     -e MONGO_INITDB_ROOT_USERNAME=admin \
     -e MONGO_INITDB_ROOT_PASSWORD=password \
     -v mongo_data:/data/db \
     mongo:6.0
   ```

2. **Database Initialization**
   ```javascript
   // Connect to MongoDB and create indexes
   use carbon-track;

   // Create indexes for better performance
   db.companies.createIndex({ "walletAddress": 1 }, { unique: true });
   db.productBatches.createIndex({ "companyId": 1, "createdAt": -1 });
   db.tokens.createIndex({ "tokenId": 1 }, { unique: true });
   ```

## ⛓️ Blockchain Deployment

### Smart Contract Deployment

1. **Setup Hardhat Environment**
   ```bash
   cd hardhat
   npm install
   cp .env.example .env
   ```

2. **Configure Networks**
   ```javascript
   // hardhat.config.ts
   export default {
     networks: {
       fuji: {
         url: "https://api.avax-test.network/ext/bc/C/rpc",
         accounts: [process.env.PRIVATE_KEY],
         chainId: 43113
       },
       mainnet: {
         url: "https://api.avax.network/ext/bc/C/rpc",
         accounts: [process.env.PRIVATE_KEY],
         chainId: 43114
       }
     }
   };
   ```

3. **Deploy Contracts**
   ```bash
   # Deploy to testnet
   npx hardhat run scripts/deploy.ts --network fuji

   # Deploy to mainnet
   npx hardhat run scripts/deploy.ts --network mainnet
   ```

4. **Verify Contracts**
   ```bash
   npx hardhat verify --network mainnet DEPLOYED_CONTRACT_ADDRESS
   ```

### Contract Upgrade Strategy

1. **Use Proxy Pattern**
   ```solidity
   // Use OpenZeppelin upgradeable contracts
   contract SupplyChainTokensUpgradeable is
     ERC1155Upgradeable,
     OwnableUpgradeable
   {
     // Implementation
   }
   ```

2. **Migration Scripts**
   ```javascript
   // scripts/upgrade.ts
   async function main() {
     const SupplyChainTokensV2 = await ethers.getContractFactory("SupplyChainTokensV2");
     await upgrades.upgradeProxy(PROXY_ADDRESS, SupplyChainTokensV2);
   }
   ```

## 🔒 Security Configuration

### SSL/TLS Setup

1. **Let's Encrypt with Certbot**
   ```bash
   # Install certbot
   sudo apt install certbot python3-certbot-nginx

   # Get certificate
   sudo certbot --nginx -d your-domain.com
   ```

2. **Nginx Configuration**
   ```nginx
   server {
     listen 443 ssl http2;
     server_name your-domain.com;

     ssl_certificate /etc/letsencrypt/live/your-domain.com/fullchain.pem;
     ssl_certificate_key /etc/letsencrypt/live/your-domain.com/privkey.pem;

     location / {
       proxy_pass http://localhost:3000;
       proxy_http_version 1.1;
       proxy_set_header Upgrade $http_upgrade;
       proxy_set_header Connection 'upgrade';
       proxy_set_header Host $host;
       proxy_cache_bypass $http_upgrade;
     }
   }
   ```

### Environment Security

1. **Secret Management**
   ```bash
   # Use AWS Secrets Manager
   aws secretsmanager create-secret \
     --name carbon-track/production \
     --secret-string '{"MONGODB_URI":"...","NEXTAUTH_SECRET":"..."}'
   ```

2. **Environment Validation**
   ```typescript
   // lib/env.ts
   import { z } from 'zod';

   const envSchema = z.object({
     MONGODB_URI: z.string().url(),
     NEXT_PUBLIC_CONTRACT_ADDRESS: z.string().regex(/^0x[a-fA-F0-9]{40}$/),
     NEXT_PUBLIC_CHAIN_ID: z.string().transform(Number),
   });

   export const env = envSchema.parse(process.env);
   ```

## 📊 Monitoring and Observability

### Application Monitoring

1. **Health Check Endpoint**
   ```typescript
   // app/api/health/route.ts
   export async function GET() {
     try {
       // Check database connection
       await connectToMongoDB();

       // Check blockchain connection
       const provider = new ethers.JsonRpcProvider(process.env.NEXT_PUBLIC_RPC_URL);
       await provider.getBlockNumber();

       return Response.json({
         status: 'healthy',
         timestamp: new Date().toISOString()
       });
     } catch (error) {
       return Response.json({
         status: 'unhealthy',
         error: error.message
       }, { status: 500 });
     }
   }
   ```

2. **Sentry Integration**
   ```typescript
   // sentry.client.config.ts
   import * as Sentry from "@sentry/nextjs";

   Sentry.init({
     dsn: process.env.SENTRY_DSN,
     environment: process.env.NODE_ENV,
     tracesSampleRate: 0.1,
   });
   ```

### Performance Monitoring

1. **Next.js Analytics**
   ```javascript
   // next.config.js
   module.exports = {
     experimental: {
       instrumentationHook: true,
     },
     analytics: {
       id: process.env.NEXT_PUBLIC_ANALYTICS_ID,
     },
   };
   ```

2. **Custom Metrics**
   ```typescript
   // lib/metrics.ts
   export function trackBlockchainOperation(operation: string, duration: number) {
     if (typeof window !== 'undefined' && window.gtag) {
       window.gtag('event', 'blockchain_operation', {
         operation,
         duration,
         custom_map: { metric1: 'blockchain_latency' }
       });
     }
   }
   ```

## 🔄 CI/CD Pipeline

### GitHub Actions

```yaml
# .github/workflows/deploy.yml
name: Deploy to Production

on:
  push:
    branches: [main]

jobs:
  test:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      - uses: actions/setup-node@v3
        with:
          node-version: '18'
          cache: 'pnpm'

      - run: pnpm install
      - run: pnpm lint
      - run: pnpm build

      # Test smart contracts
      - name: Test contracts
        run: |
          cd hardhat
          npm install
          npx hardhat test

  deploy:
    needs: test
    runs-on: ubuntu-latest
    if: github.ref == 'refs/heads/main'

    steps:
      - uses: actions/checkout@v3

      - name: Deploy to Vercel
        uses: amondnet/vercel-action@v25
        with:
          vercel-token: ${{ secrets.VERCEL_TOKEN }}
          vercel-org-id: ${{ secrets.VERCEL_ORG_ID }}
          vercel-project-id: ${{ secrets.VERCEL_PROJECT_ID }}
          vercel-args: '--prod'
```

## 📈 Scaling Considerations

### Database Optimization

1. **Indexing Strategy**
   ```javascript
   // Compound indexes for common queries
   db.productBatches.createIndex({
     "companyId": 1,
     "createdAt": -1,
     "productTemplateId": 1
   });

   // Text search index
   db.companies.createIndex({
     "name": "text",
     "description": "text"
   });
   ```

2. **Read Replicas**
   ```javascript
   // Use read preference for analytics queries
   const analyticsData = await db.collection('productBatches')
     .find({})
     .readPref('secondary')
     .toArray();
   ```

### Caching Strategy

1. **Redis Integration**
   ```typescript
   // lib/cache.ts
   import Redis from 'ioredis';

   const redis = new Redis(process.env.REDIS_URL);

   export async function cacheTokenMetadata(tokenId: string, metadata: any) {
     await redis.setex(`token:${tokenId}`, 3600, JSON.stringify(metadata));
   }
   ```

2. **CDN Configuration**
   ```javascript
   // next.config.js
   module.exports = {
     images: {
       domains: ['cdn.carbon-track.com'],
       unoptimized: false,
     },
     experimental: {
       staticPageGenerationTimeout: 120,
     },
   };
   ```

## 🆘 Troubleshooting

### Common Issues

1. **Database Connection Errors**
   ```bash
   # Check MongoDB Atlas network access
   # Verify IP whitelist includes deployment server
   # Test connection string locally
   ```

2. **Blockchain RPC Errors**
   ```bash
   # Check RPC endpoint status
   curl -X POST \
     -H "Content-Type: application/json" \
     -d '{"jsonrpc":"2.0","method":"eth_blockNumber","params":[],"id":1}' \
     https://api.avax.network/ext/bc/C/rpc
   ```

3. **Build Failures**
   ```bash
   # Clear Next.js cache
   rm -rf .next

   # Reinstall dependencies
   rm -rf node_modules pnpm-lock.yaml
   pnpm install
   ```

### Monitoring Checklist

- [ ] Application health endpoint responding
- [ ] Database connectivity working
- [ ] Blockchain RPC accessible
- [ ] SSL certificate valid
- [ ] Environment variables set
- [ ] Smart contracts deployed
- [ ] Monitoring/alerting configured

## 📞 Support

For deployment issues:
- Check the [troubleshooting section](#troubleshooting)
- Review application logs
- Test health endpoints
- Verify environment configuration
- Contact support with error details and environment info

---

**Remember**: Always test deployments in staging environment before production!