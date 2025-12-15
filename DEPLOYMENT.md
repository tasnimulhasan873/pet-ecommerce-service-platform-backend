# Backend Deployment Guide for Vercel

## ✅ Fixed Issues

1. **Module Export**: Express app is now properly exported for Vercel serverless
2. **CORS Configuration**: Multiple origins are now supported:

   - http://localhost:5173 (local dev)
   - http://localhost:3000 (local dev)
   - https://petecommerce-9eef4.web.app (Firebase)
   - https://petecommerce-9eef4.firebaseapp.com (Firebase alt)
   - https://puchito.netlify.app (Netlify)

3. **Serverless Compatibility**:

   - MongoDB connection pooling configured
   - Connection caching for serverless environment
   - Conditional app.listen() (only runs locally)
   - Proper module.exports for Vercel

4. **vercel.json**: Complete configuration with proper routes and CORS headers

## 🚀 Deployment Steps

### 1. Environment Variables

Make sure these are set in Vercel dashboard:

```
MONGO_URI=mongodb+srv://...
STRIPE_SECRET_KEY=sk_test_...
NODE_ENV=production
```

### 2. Deploy to Vercel

```bash
# Install Vercel CLI if not already installed
npm i -g vercel

# Deploy
vercel --prod
```

### 3. Update Frontend CORS

Your frontend should now point to:

```
https://your-backend.vercel.app
```

## 🔧 Local Development

```bash
npm install
npm run dev
```

## 📝 Key Changes Made

### index.js

- Added proper CORS with multiple origins
- MongoDB connection with pooling and caching
- Removed app.listen() in production/Vercel environment
- Added `module.exports = app` for Vercel
- Added health check route at root

### vercel.json

- Configured proper routing for all HTTP methods
- Added CORS headers at infrastructure level
- Set NODE_ENV to production
- Optimized for Express + MongoDB

### .vercelignore

- Excludes unnecessary files from deployment
- Reduces function size

## 🧪 Testing

After deployment, test with:

```bash
curl https://your-backend.vercel.app
```

Should return:

```json
{
  "status": "OK",
  "message": "Backend API is running",
  "timestamp": "2025-12-12T..."
}
```

## 🐛 Troubleshooting

### If you still get CORS errors:

1. Check Vercel environment variables are set
2. Verify frontend URL is in allowedOrigins array
3. Clear browser cache
4. Check Vercel function logs

### If MongoDB connection fails:

1. Verify MONGO_URI in Vercel environment variables
2. Check MongoDB Atlas allows connections from anywhere (0.0.0.0/0)
3. Review Vercel function logs for connection errors

### If routes return 404:

1. Check vercel.json routes configuration
2. Ensure all routes start with `/`
3. Verify deployment completed successfully

## 📚 Additional Resources

- [Vercel Node.js Guide](https://vercel.com/docs/functions/serverless-functions/runtimes/node-js)
- [Express on Vercel](https://vercel.com/guides/using-express-with-vercel)
