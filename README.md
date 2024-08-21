
# Ether Private Bank API Documentation

## Overview

This documentation outlines the process for integrating with Ether Private Bank's API, which allows clients to log in using their email and password, with a JWT token that has a 365-day expiration period. The collection also includes endpoints for BR Code payments and checking wallet balances.

### Authentication Flow

Clients authenticate via the `/auth/login` endpoint, receiving a JWT token upon successful login. The token is valid for 365 days. The token is then used to authenticate subsequent requests.

## Postman Collection Structure

The collection consists of three main categories:

1. **Auth** - Handles authentication.
2. **BR Code** - Manages BR Code creation and payments.
3. **Wallet** - Retrieves wallet balances.
