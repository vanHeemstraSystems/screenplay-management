# Building Our Application

This page explains how to build the application from source.

## Prerequisites

Before building, ensure you have completed all the steps in the [Requirements](requirements.md) section.

## Build Steps

1. Clone the repository
   ```bash
   git clone https://eur01.safelinks.protection.outlook.com/?url=https%3A%2F%2Fgithub.com%2Fyourusername%2Fyourproject.git&data=05%7C02%7Cwillem.van.heemstra%40asml.com%7C638cad1beabe4f05d05508dd5f240e68%7Caf73baa8f5944eb2a39d93e96cad61fc%7C1%7C0%7C638771330685290591%7CUnknown%7CTWFpbGZsb3d8eyJFbXB0eU1hcGkiOnRydWUsIlYiOiIwLjAuMDAwMCIsIlAiOiJXaW4zMiIsIkFOIjoiTWFpbCIsIldUIjoyfQ%3D%3D%7C0%7C%7C%7C&sdata=S5wddJ8ahlmLC0AfBrGU8mHxOPzixygRhY2mml7xh6A%3D&reserved=0
   cd yourproject
   ```

2. Configure the environment
   ```bash
   cp .env.example .env
   # Edit .env with your configuration
   ```

3. Build the application
   ```bash
   ./build.sh
   ```

## Troubleshooting

Common issues and their solutions:

- **Error: Port already in use**: Stop any services using port 8000
- **Database connection fails**: Verify PostgreSQL is running and credentials are correct 
