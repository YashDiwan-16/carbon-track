# Carbon Track API Documentation

This document provides comprehensive documentation for the Carbon Track API endpoints.

## Base URL

```
http://localhost:3000/api (Development)
https://your-domain.com/api (Production)
```

## Authentication

All API endpoints require wallet-based authentication. Include the connected wallet address in request headers or ensure the user's wallet is connected through the frontend.

## Data Models

### Company
```typescript
interface Company {
  _id: string;
  name: string;
  walletAddress: string;
  description?: string;
  website?: string;
  industry?: string;
  location: {
    address: string;
    city: string;
    state: string;
    country: string;
    zipCode: string;
    coordinates?: {
      lat: number;
      lng: number;
    };
  };
  contactInfo: {
    email: string;
    phone?: string;
  };
  createdAt: Date;
  updatedAt: Date;
}
```

### Plant
```typescript
interface Plant {
  _id: string;
  companyId: string;
  name: string;
  description?: string;
  location: {
    address: string;
    city: string;
    state: string;
    country: string;
    zipCode: string;
    coordinates: {
      lat: number;
      lng: number;
    };
  };
  capacity?: number;
  operationalSince?: Date;
  certifications?: string[];
  createdAt: Date;
  updatedAt: Date;
}
```

### ProductTemplate
```typescript
interface ProductTemplate {
  _id: string;
  companyId: string;
  name: string;
  description?: string;
  category: string;
  carbonFootprintPerUnit: number; // kg CO2 equivalent
  isRawMaterial: boolean;
  components: Array<{
    productTemplateId: string;
    quantityRequired: number;
    unit: string;
  }>;
  productionDetails?: {
    energyConsumption: number; // kWh per unit
    waterConsumption: number; // liters per unit
    wasteGenerated: number; // kg per unit
  };
  createdAt: Date;
  updatedAt: Date;
}
```

### ProductBatch
```typescript
interface ProductBatch {
  _id: string;
  companyId: string;
  plantId: string;
  productTemplateId: string;
  batchNumber: string;
  quantity: number;
  unit: string;
  productionDate: Date;
  expiryDate?: Date;
  tokenId?: string;
  tokensMinted?: number;
  componentTokensUsed: Array<{
    tokenId: string;
    quantity: number;
  }>;
  qualityMetrics?: {
    grade: string;
    testResults?: Record<string, any>;
  };
  carbonFootprint: {
    perUnit: number;
    total: number;
    breakdown: Record<string, number>;
  };
  createdAt: Date;
  updatedAt: Date;
}
```

## API Endpoints

### Companies

#### GET /api/companies
Get all companies (public endpoint for partners)

**Response:**
```json
{
  "companies": [
    {
      "_id": "company_id",
      "name": "Company Name",
      "description": "Company description",
      "industry": "Manufacturing",
      "location": {
        "city": "City",
        "country": "Country"
      }
    }
  ]
}
```

#### POST /api/companies
Register a new company

**Request Body:**
```json
{
  "name": "Company Name",
  "walletAddress": "0x...",
  "description": "Company description",
  "website": "https://company.com",
  "industry": "Manufacturing",
  "location": {
    "address": "123 Main St",
    "city": "City",
    "state": "State",
    "country": "Country",
    "zipCode": "12345",
    "coordinates": {
      "lat": 40.7128,
      "lng": -74.0060
    }
  },
  "contactInfo": {
    "email": "contact@company.com",
    "phone": "+1-555-0123"
  }
}
```

**Response:**
```json
{
  "message": "Company registered successfully",
  "company": { /* Company object */ }
}
```

#### GET /api/companies/[walletAddress]
Get company by wallet address

**Response:**
```json
{
  "company": { /* Company object */ }
}
```

### Plants

#### GET /api/plants
Get all plants for the authenticated company

**Response:**
```json
{
  "plants": [
    { /* Plant objects */ }
  ]
}
```

#### POST /api/plants
Create a new plant

**Request Body:**
```json
{
  "name": "Plant Name",
  "description": "Plant description",
  "location": {
    "address": "456 Industrial Blvd",
    "city": "City",
    "state": "State",
    "country": "Country",
    "zipCode": "12345",
    "coordinates": {
      "lat": 40.7128,
      "lng": -74.0060
    }
  },
  "capacity": 10000,
  "operationalSince": "2020-01-01T00:00:00.000Z",
  "certifications": ["ISO 14001", "LEED Gold"]
}
```

#### DELETE /api/plants/[plantId]
Delete a plant

### Product Templates

#### GET /api/product-templates
Get all product templates for the authenticated company

**Query Parameters:**
- `isRawMaterial` (boolean): Filter by raw material status

**Response:**
```json
{
  "templates": [
    { /* ProductTemplate objects */ }
  ]
}
```

#### POST /api/product-templates
Create a new product template

**Request Body:**
```json
{
  "name": "Product Name",
  "description": "Product description",
  "category": "Electronics",
  "carbonFootprintPerUnit": 2.5,
  "isRawMaterial": false,
  "components": [
    {
      "productTemplateId": "template_id",
      "quantityRequired": 2,
      "unit": "kg"
    }
  ],
  "productionDetails": {
    "energyConsumption": 10.5,
    "waterConsumption": 50,
    "wasteGenerated": 0.1
  }
}
```

#### DELETE /api/product-templates/[templateId]
Delete a product template

### Product Batches

#### GET /api/product-batches
Get all product batches for the authenticated company

**Response:**
```json
{
  "batches": [
    { /* ProductBatch objects */ }
  ]
}
```

#### POST /api/product-batches
Create a new product batch and mint blockchain tokens

**Request Body:**
```json
{
  "plantId": "plant_id",
  "productTemplateId": "template_id",
  "quantity": 100,
  "unit": "units",
  "productionDate": "2024-01-15T00:00:00.000Z",
  "expiryDate": "2025-01-15T00:00:00.000Z",
  "componentTokensUsed": [
    {
      "tokenId": "123",
      "quantity": 200
    }
  ],
  "qualityMetrics": {
    "grade": "A+",
    "testResults": {
      "durability": "passed",
      "safety": "passed"
    }
  }
}
```

**Response:**
```json
{
  "message": "Product batch created and tokens minted successfully",
  "batch": { /* ProductBatch object */ },
  "tokenId": "456",
  "transactionHash": "0x..."
}
```

### Tokens

#### GET /api/tokens
Get token inventory for the authenticated company

**Response:**
```json
{
  "tokens": [
    {
      "tokenId": "123",
      "balance": 50,
      "productTemplate": { /* ProductTemplate object */ },
      "batch": { /* ProductBatch object */ }
    }
  ]
}
```

#### POST /api/tokens/transfer
Transfer tokens to another company

**Request Body:**
```json
{
  "tokenId": "123",
  "toAddress": "0x...",
  "amount": 10,
  "reason": "Sale to customer"
}
```

**Response:**
```json
{
  "message": "Tokens transferred successfully",
  "transactionHash": "0x...",
  "transfer": {
    "tokenId": "123",
    "from": "0x...",
    "to": "0x...",
    "amount": 10,
    "reason": "Sale to customer",
    "timestamp": "2024-01-15T12:00:00.000Z"
  }
}
```

#### GET /api/tokens/[tokenId]/transfers
Get transfer history for a specific token

**Response:**
```json
{
  "transfers": [
    {
      "from": "0x...",
      "to": "0x...",
      "amount": 10,
      "reason": "Sale to customer",
      "timestamp": "2024-01-15T12:00:00.000Z",
      "transactionHash": "0x..."
    }
  ]
}
```

### Partners

#### GET /api/partners
Get all partner relationships for the authenticated company

**Response:**
```json
{
  "suppliers": [
    { /* Company objects */ }
  ],
  "customers": [
    { /* Company objects */ }
  ]
}
```

#### POST /api/partners
Add a new partner relationship

**Request Body:**
```json
{
  "partnerAddress": "0x...",
  "relationship": "supplier", // or "customer"
  "notes": "Primary raw material supplier"
}
```

#### DELETE /api/partners/[partnerAddress]
Remove a partner relationship

### Metadata

#### GET /api/metadata/[tokenId]
Get metadata for a specific token (public endpoint)

**Response:**
```json
{
  "name": "Product Name - Batch #123",
  "description": "Product description with batch details",
  "image": "https://...",
  "attributes": [
    {
      "trait_type": "Carbon Footprint",
      "value": "2.5 kg CO2",
      "display_type": "string"
    },
    {
      "trait_type": "Production Date",
      "value": "2024-01-15",
      "display_type": "date"
    }
  ],
  "batch": { /* ProductBatch object */ },
  "productTemplate": { /* ProductTemplate object */ },
  "company": { /* Company object */ },
  "plant": { /* Plant object */ }
}
```

## Error Responses

All API endpoints return consistent error responses:

```json
{
  "error": "Error message",
  "code": "ERROR_CODE",
  "details": { /* Additional error details */ }
}
```

### Common Error Codes

- `COMPANY_NOT_FOUND` - Company not registered
- `UNAUTHORIZED` - Wallet not connected or invalid
- `INVALID_INPUT` - Request validation failed
- `BLOCKCHAIN_ERROR` - Smart contract transaction failed
- `INSUFFICIENT_BALANCE` - Not enough tokens for transfer
- `PLANT_NOT_FOUND` - Plant doesn't exist
- `TEMPLATE_NOT_FOUND` - Product template doesn't exist

## Rate Limiting

API endpoints are rate limited to prevent abuse:
- **General endpoints**: 100 requests per minute
- **Blockchain operations**: 10 requests per minute
- **Metadata endpoints**: 1000 requests per minute (for public access)

## Authentication Flow

1. User connects wallet through frontend
2. Frontend validates wallet signature
3. API endpoints verify wallet address
4. Company association checked for protected endpoints

## Blockchain Integration

### Token Minting
When creating a product batch:
1. Validate component token balances
2. Calculate carbon footprint
3. Mint new ERC-1155 tokens
4. Burn component tokens used
5. Store metadata URI on blockchain

### Token Transfers
When transferring tokens:
1. Validate recipient address
2. Check sender balance
3. Execute blockchain transfer
4. Record transfer history
5. Update local balances

## Development Notes

### Testing
Use the following test data for development:

```javascript
// Test wallet addresses (Fuji testnet)
const testAddresses = {
  company1: "0x742d35Cc6634C0532925a3b8D40002636f0ed0A6",
  company2: "0x8ba1f109551bD432803012645Hac136c5BaB2A7f",
  company3: "0xD7Ac16Cc684681b647e7e4d76e7f7d0c0f5B2A7c"
};

// Test contract address (Fuji testnet)
const contractAddress = "0x6f70264A4f3608FFa8Ff3ED5C6a1c4542D79fb88";
```

### Gas Optimization
- Batch operations where possible
- Use appropriate gas limits for Avalanche
- Implement retry logic for failed transactions

### Error Handling
- Always validate inputs
- Provide meaningful error messages
- Log errors for debugging
- Handle blockchain timeouts gracefully

## Webhook Integration

For real-time updates, the API supports webhook notifications:

### Webhook Events
- `batch.created` - New product batch created
- `token.transferred` - Token transfer completed
- `partner.added` - New partner relationship added

### Webhook Payload
```json
{
  "event": "batch.created",
  "timestamp": "2024-01-15T12:00:00.000Z",
  "data": {
    "batch": { /* ProductBatch object */ },
    "company": { /* Company object */ }
  }
}
```

## SDK Usage

For TypeScript/JavaScript applications:

```typescript
import { CarbonTrackAPI } from 'carbon-track-sdk';

const api = new CarbonTrackAPI({
  baseURL: 'https://api.carbon-track.com',
  walletAddress: '0x...'
});

// Create a product batch
const batch = await api.batches.create({
  plantId: 'plant_id',
  productTemplateId: 'template_id',
  quantity: 100
});

// Transfer tokens
await api.tokens.transfer({
  tokenId: '123',
  toAddress: '0x...',
  amount: 10
});
```