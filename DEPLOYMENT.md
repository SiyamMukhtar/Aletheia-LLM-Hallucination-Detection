# Deployment README

## Frontend and Backend Communication

### Development Setup
1. **Backend runs on**: `http://localhost:5000`
2. **Frontend runs on**: `http://localhost:5173`
3. **API Communication**: Frontend makes requests to `/api/verify`
4. **Proxy**: Vite dev server proxies `/api/*` requests to backend

### Environment Variables

#### Frontend (.env)
```
VITE_BACKEND_URL=http://localhost:5000
VITE_API_BASE_URL=http://localhost:5000/api
```

#### Backend (.env)
```
PORT=5000
FLASK_ENV=development
FRONTEND_URL=http://localhost:5173
```

### Production Deployment

#### Option 1: Separate Deployments
1. **Deploy Backend** (e.g., Railway, Heroku, Render):
   - Update backend `.env`: `FRONTEND_URL=https://your-frontend-domain.com`
   - Backend URL: `https://your-backend-domain.com`

2. **Deploy Frontend** (e.g., Vercel, Netlify):
   - Update `.env.production`: 
     ```
     VITE_BACKEND_URL=https://your-backend-domain.com
     VITE_API_BASE_URL=https://your-backend-domain.com/api
     ```

#### Option 2: Single Deployment
- Deploy both on same platform (e.g., Railway with monorepo)
- Update environment variables accordingly

### Why This Was Taking Long to Answer

The application was slow because:

1. **Heavy ML Models**: Loading GPT-2, sentence transformers, and NLTK on first request
2. **Wikipedia API Calls**: Each verification makes multiple Wikipedia searches
3. **Embedding Calculations**: Computing semantic similarity for each claim
4. **No Caching**: Every request reprocesses everything

### Performance Optimizations Implemented

1. **Lazy Loading**: Models load only when needed
2. **Demo Mode**: Fallback responses when models aren't loaded
3. **Error Handling**: Graceful degradation if APIs fail
4. **Chunked Processing**: Claims processed individually

### Suggested Improvements

1. **Caching**: Add Redis for Wikipedia results
2. **Background Jobs**: Use Celery for long-running tasks
3. **Model Optimization**: Use quantized or smaller models
4. **CDN**: Cache static Wikipedia content
5. **Database**: Store previous fact-checks