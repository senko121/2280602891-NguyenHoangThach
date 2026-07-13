---
title : "Build, upload and connect everything"
date : 2026-07-13
weight : 3
chapter : false
pre : " <b> 5.6.3 </b> "
---

#### Build the React application for production

1. On the local machine, create the file ```react-project/.env.production``` — Create React App reads it automatically during `npm run build` and bakes the values into the bundle:

```
REACT_APP_BASE_URL=https://d2x5ledabb9h2k.cloudfront.net
REACT_APP_API_URL=https://d2x5ledabb9h2k.cloudfront.net/api/v1
REACT_APP_IMAGE_URL=https://petshop-images-<your-unique-name>.s3.ap-southeast-1.amazonaws.com
REACT_APP_GOOGLE_CLIENT_ID=<google-oauth-client-id>
```

{{% notice note %}}
`REACT_APP_API_URL` points at **the CloudFront domain itself** with the `/api/v1` prefix — thanks to the behavior created in 5.6.2, frontend and API share one HTTPS origin. Images are loaded straight from the public images bucket.
{{% /notice %}}

2. Build:

```powershell
cd react-project
npm run build
```

The production bundle is generated in the ```build/``` folder.

![build](/images/5-Workshop/5.6-Frontend/deploy01.png)

#### Upload to the frontend bucket

3. Open the ```petshop-frontend-...``` bucket → **Upload** → drag **the contents inside the `build/` folder** (the `index.html`, `favicon.ico`... files and the `static/`, `assets/` folders) — ⚠️ not the `build` folder itself: `index.html` must sit at the bucket root.

![upload](/images/5-Workshop/5.6-Frontend/deploy02.png)

4. If this is a re-deployment, invalidate the CloudFront cache so visitors receive the new build immediately: distribution → **Invalidations** tab → **Create invalidation** → object paths: ```/*```.

#### Point the backend at the new frontend

5. Update ```env.aws``` on the local machine with the CloudFront domain, then upload it to the **petshop-deploy** bucket (overwrite):

```
FRONTEND_URL=https://d2x5ledabb9h2k.cloudfront.net
CORS_ALLOWED_ORIGINS=https://d2x5ledabb9h2k.cloudfront.net
```

6. Refresh the running instance through **Session Manager** (future instances pick the file up automatically at boot):

```bash
sudo aws s3 cp s3://<your-deploy-bucket>/env.aws /var/www/petshop/.env
sudo chown nginx:nginx /var/www/petshop/.env
cd /var/www/petshop && sudo php artisan config:clear
```

#### Register the domain with Google OAuth

7. Open [Google Cloud Console](https://console.cloud.google.com/) → **APIs & Services → Credentials** → open the OAuth 2.0 Client ID → under **Authorized JavaScript origins** click **Add URI** and enter ```https://d2x5ledabb9h2k.cloudfront.net``` (no trailing slash, no path) → **Save**. Without this step, Google Sign-In fails with *Error 400: origin_mismatch*. Changes take effect after 5 minutes to a few hours.

![google oauth](/images/5-Workshop/5.6-Frontend/deploy03.png)

8. Open ```https://d2x5ledabb9h2k.cloudfront.net``` — the PetShop home page loads over HTTPS with product images served from S3. 🎉

![website](/images/5-Workshop/5.6-Frontend/deploy04.png)
