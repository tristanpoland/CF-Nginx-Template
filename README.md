# NGINX Cloud Foundry Buildpack Test Application

This is a simple static website designed to test the official Nginx buildpack for Cloud Foundry.

## Project Structure

```
├── index.html          # Main HTML page
├── 404.html            # Custom error page
├── css/
│   └── style.css       # CSS styles
├── nginx.conf          # Custom Nginx configuration
├── bin/
│   └── boot.sh         # Optional boot script
├── manifest.yml        # Cloud Foundry deployment manifest
└── public/             # Directory for public assets (will contain the HTML/CSS files)
```

## Features Demonstrated

- Basic static HTML/CSS website
- Custom Nginx configuration
- Custom 404 error page
- Gzip compression
- Cache control headers
- Health check endpoint at `/health`

## Deployment Instructions

1. Make sure you have the Cloud Foundry CLI installed and are logged in:
   ```
   cf login -a <your-cf-api-endpoint>
   ```

2. Edit the `manifest.yml` file to replace `${CF_DOMAIN}` with your Cloud Foundry domain (e.g., `apps.cloudfoundry.com`).

3. Create the proper directory structure:
   ```
   mkdir -p public/css bin
   mv index.html 404.html public/
   mv css/style.css public/css/
   chmod +x bin/boot.sh
   ```

4. Navigate to the project directory and push the application:
   ```
   cf push
   ```

5. If you want to override the default application name in the manifest:
   ```
   cf push my-custom-app-name
   ```

## Testing the Buildpack

After deployment, you can verify the application is running by:

1. Checking the application status:
   ```
   cf apps
   ```

2. Accessing the application URL (provided after successful deployment)

3. Testing the 404 page by navigating to a non-existent URL

4. Checking the health endpoint:
   ```
   curl https://your-app-url/health
   ```

5. Verifying gzip compression:
   ```
   curl -H "Accept-Encoding: gzip" -I https://your-app-url/
   ```

6. Checking the logs:
   ```
   cf logs nginx-cf-test --recent
   ```

## How the Nginx Buildpack Works

The Nginx buildpack:
1. Detects if the application has a `nginx.conf` file in the root directory
2. Installs Nginx
3. Configures it according to your `nginx.conf` file
4. Starts Nginx using either the default start command or a custom `bin/boot.sh` script

## Troubleshooting

If the application fails to deploy:

1. Check the staging logs:
   ```
   cf logs nginx-cf-test --recent
   ```

2. Verify the buildpack is available:
   ```
   cf buildpacks
   ```

3. Try specifying the buildpack explicitly:
   ```
   cf push -b nginx_buildpack
   ```

4. Ensure your file structure is correct and all required files exist.

5. Check that `nginx.conf` is properly formatted and contains all required directives.

## Additional Configuration Options

The `nginx.conf` file included with this application demonstrates:
- Gzip compression
- Custom error pages
- Cache control headers
- Health check endpoint

You can modify the configuration based on your specific requirements.