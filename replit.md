# Tax Payment Approval MCP Server

## Overview
A Python-based Model Context Protocol (MCP) server designed as a backend for a voice agent system that helps taxpayers manage tax payments. This MVP allows taxpayers to approve, reschedule, or modify pending tax payments through ElevenLabs voice integration.

**Service:** Tax Payment Management Voice Agent  
**Version:** 1.0.0  
**Created:** October 28, 2025  
**Deployment Target:** Render  

## Purpose
This server provides a backend API for voice agents (via ElevenLabs) to interact with tax payment data. It enables taxpayers to:
- Verify their identity via SMS code
- View pending tax payments
- Approve payments for processing
- Reschedule payment dates
- Reduce payment amounts
- Request bank account setup via Plaid integration

## Current State
**Status:** Ready for Render Deployment ✅

The server is fully functional and configured for deployment to Render. All MCP tools are implemented with mock data for AgriFarm & Sons (taxpayerId: 3) with 7 pending tax payments.

## Recent Changes (October 28, 2025)
- Initial project setup with complete MCP server implementation
- Created main.py with SSE/HTTP transport for ElevenLabs integration
- Configured Python 3.11 environment with all dependencies
- Added render.yaml for Render deployment
- Implemented 7 MCP tools for tax payment management
- Added health check endpoint at `/health`
- Configured proper .gitignore for Python projects

## Project Architecture

### File Structure
```
/
├── main.py              # Main MCP server with Starlette app
├── requirements.txt     # Python dependencies
├── render.yaml          # Render deployment configuration
├── .gitignore          # Python-specific gitignore
└── replit.md           # This documentation file
```

### Technology Stack
- **Python 3.11** - Runtime environment
- **MCP SDK** (v1.19.0) - Model Context Protocol implementation
- **Starlette** (v0.48.0) - ASGI web framework for HTTP/SSE
- **Uvicorn** (v0.38.0) - ASGI server for production deployment
- **SSE Transport** - Server-Sent Events for ElevenLabs integration

### API Endpoints

#### `/sse` (GET)
Server-Sent Events endpoint for ElevenLabs voice agent connections.

#### `/messages` (POST)
Message handling endpoint for MCP tool calls.

#### `/health` (GET)
Health check endpoint that returns:
```json
{
  "status": "healthy",
  "service": "tax-payment-mcp",
  "version": "1.0.0"
}
```

### MCP Tools

The server provides 7 MCP tools for the voice agent:

1. **send_verification_code** - Send SMS verification code (mock - always uses "1234")
2. **verify_code** - Verify SMS code provided by taxpayer
3. **get_pending_payments** - Retrieve all pending payments for a taxpayer
4. **approve_payment** - Approve a payment (changes status to "QueuedForPayment")
5. **reschedule_payment** - Change payment date to new date
6. **reduce_payment_amount** - Modify payment amount to lower value
7. **request_bank_setup_email** - Send Plaid bank setup email (mock)

### Mock Data

The server includes 7 mock tax payments for AgriFarm & Sons (taxpayerId: 3):
- **Payment #2**: $520 to Georgia, due 2025-06-15
- **Payment #34**: $100 to Georgia, due 2025-07-31
- **Payment #1**: $520 to Georgia, due 2025-08-04
- **Payment #81**: $21,000 to IRS, due 2025-08-07
- **Payment #62**: $21,000 to Georgia, due 2025-08-30
- **Payment #111**: $123 to IRS, due 2025-09-15
- **Payment #3**: $520 to Georgia, due 2025-09-15

All payments are in "PendingClientApproval" status and associated with tax entities under AgriFarm & Sons.

## Deployment Instructions

### Deploying to Render

1. **Create a Render account** at https://render.com

2. **Create a new Web Service** in your Render dashboard

3. **Connect your Git repository** or upload the files

4. **Render will automatically detect** the `render.yaml` configuration

5. **The service will deploy with**:
   - Build Command: `pip install -r requirements.txt`
   - Start Command: `python main.py`
   - Port: 8000 (automatically configured)
   - Health Check: `/health` endpoint

6. **No environment variables required** for basic operation (uses mock data)

7. **Access your endpoints**:
   - SSE: `https://your-app.onrender.com/sse`
   - Health: `https://your-app.onrender.com/health`

### Testing Locally (Replit)

The server runs on port 8000 by default. Use the workflow to start:
```bash
python main.py
```

Test the health endpoint:
```bash
curl http://localhost:8000/health
```

## ElevenLabs Integration

To connect this MCP server with ElevenLabs:

1. Deploy the server to Render (get your public URL)
2. In ElevenLabs Conversational AI settings:
   - Set MCP Server URL to: `https://your-app.onrender.com/sse`
   - Configure tools to use the 7 MCP tools provided
3. The voice agent can now call tax payment management tools

## Future Enhancements

### Phase 2 Features
- Replace mock data with real database (PostgreSQL)
- Implement actual SMS sending via Twilio
- Add real Plaid integration for bank setup
- User authentication and session management
- Payment history and audit logging
- Multi-taxpayer support
- Rate limiting and security hardening

### Phase 3 Features
- Email notifications for payment actions
- Scheduled payment reminders
- Payment analytics dashboard
- Support for additional tax authorities
- Batch payment operations
- Export payment reports (PDF/CSV)

## User Preferences
No specific preferences recorded yet.

## Notes
- This is an MVP with mock data - suitable for demos and testing
- Verification code is always "1234" for testing
- All data is stored in-memory (resets on restart)
- No persistence layer in current version
- Designed for single-taxpayer testing (taxpayerId: 3)
