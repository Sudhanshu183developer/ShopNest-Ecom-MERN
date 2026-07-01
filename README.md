<div align="center">

# 🛍️ ShopNest

**A full-stack MERN e-commerce platform with real payments, cloud image storage, and role-based admin control.**

[![Node](https://img.shields.io/badge/Node.js-20+-339933?logo=node.js&logoColor=white)](https://nodejs.org)
[![React](https://img.shields.io/badge/React-CRA-61DAFB?logo=react&logoColor=black)](https://react.dev)
[![MongoDB](https://img.shields.io/badge/MongoDB-Atlas-47A248?logo=mongodb&logoColor=white)](https://mongodb.com)
[![Razorpay](https://img.shields.io/badge/Payments-Razorpay-0C2451)](https://razorpay.com)

[Live Demo](https://shopnest-ecom-mern-avqh.onrender.com/) · [Postman Collection](./ShopNest_Postman_Collection.json)

</div>

---

## Overview

ShopNest is a production-style e-commerce application covering the full customer and admin lifecycle — browsing products, cart management, secure checkout with real payment processing, and an admin dashboard for inventory and order control. Built on the MERN stack with JWT authentication, Cloudinary-backed image storage, and Razorpay payment integration.

## Features

**Customer**
- Browse products, view details, add to cart
- JWT-based registration and login
- Secure checkout with Razorpay (card / UPI / netbanking)
- Order history and order tracking
- Email notifications on registration and order placement

**Admin**
- Role-based dashboard access
- Add / edit / delete products with image upload (Cloudinary)
- View and manage all customer orders
- User management

## Tech Stack

| Layer | Technology |
|---|---|
| Frontend | React (CRA), Redux Toolkit, Context API |
| Backend | Node.js, Express 5, JWT middleware |
| Database | MongoDB Atlas (Mongoose 9) |
| File Storage | Cloudinary (via Multer) |
| Payments | Razorpay — order creation + server-side signature verification |
| Email | Nodemailer (Gmail) |
| Deployment | Render (single-service — Express serves the production React build) |

## Architecture
The frontend calls the backend via relative `/api/*` routes; in production, the Express server serves the compiled React build directly, so the entire app runs as a single deployable service — no CORS/config split between frontend and backend.

## Engineering Highlights

A few things I specifically diagnosed and hardened while getting this to a reliable, deployable state:

- **Payment integration bug**: Traced a silent Razorpay checkout failure to a key mismatch — the backend was creating orders under the correct API key while the frontend checkout widget was initializing with a different one. Fixed by passing the public key through the order-creation response instead of hardcoding it client-side.
- **Deployment reliability**: Added a missing build script and pinned the required Node engine version so the app builds and deploys cleanly on Render without manual intervention.
- **Upload pipeline fix**: Fixed a missing local directory in the Multer → Cloudinary upload flow that caused image uploads to fail on a fresh clone.

## Local Setup

### Prerequisites
- Node.js `>=20.19.0`
- MongoDB Atlas cluster
- Cloudinary and Razorpay accounts (free / test mode)

```bash
git clone <this-repo-url>
cd shopnest-ecom-MERN-master
npm run install-all
```

Create `backend/.env` (see `backend/.env.example`):
```env
PORT=5000
NODE_ENV=development
MONGO_URI=<MongoDB Atlas connection string>
JWT_SECRET=<random string>
CLOUDINARY_CLOUD_NAME=<Cloudinary dashboard>
CLOUDINARY_API_KEY=<Cloudinary dashboard>
CLOUDINARY_API_SECRET=<Cloudinary dashboard>
RAZORPAY_KEY_ID=<Razorpay test key>
RAZORPAY_KEY_SECRET=<Razorpay test key>
GMAIL_USER=<your gmail>
GMAIL_PASS=<Gmail App Password>
FRONTEND_URL=http://localhost:3000
```

```bash
npm run seed      # seeds demo admin + sample products
npm run dev        # runs backend (:5000) + frontend (:3000)
```
Demo admin login: `admin@shopnest.com` / `password123`

## Deployment

Deployed as a single Render Web Service:

| Setting | Value |
|---|---|
| Build Command | `npm run render-build` |
| Start Command | `npm start` |
| Environment | All `.env` vars + `NODE_ENV=production` |

## API Reference

A ready-to-import Postman collection is included (`ShopNest_Postman_Collection.json`) with a `{{token}}` variable pre-configured for testing protected admin/user/order endpoints.

## Author

**Sudhanshu Sachan**
BCA Student · MERN Stack Developer
[GitHub](https://github.com/Sudhanshu183developer) · sudhanshusachan9@gmail.com
