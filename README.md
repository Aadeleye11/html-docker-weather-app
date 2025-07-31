# html-docker-weather-app

## 1. Docker Image Details
- **Docker Hub Repository:** `docker.io/shadayo/html-docker-weather-app`
- **Image Tag:** `latest`

---

## 2. Build Instructions
To build the Docker image locally, run:

```bash
docker build -t shadayo/html-docker-weather-app:latest .
```

##  3. Run Instructions (Web01 & Web02)
a. On Fly.io (Web01)
Pull and run the image (Fly.io manages this automatically during deployment)

Ensure the app listens on 0.0.0.0:80

b. On Render.com (Web02)
Deploy using existing Docker image shadayo/html-docker-weather-app:latest

Expose port 80 for web traffic

## 4. Load Balancer Configuration
Using HAProxy (on Lb01)
Below is the HAProxy config snippet to route traffic between Web01 and Web02:

```haproxy
frontend http_front
    bind *:80
    default_backend web_servers

backend web_servers
    balance roundrobin
    server web01 weather-wise-fly.fly.dev:80 check
    server web02 html-docker-weather-app.onrender.com:80 check
```
After updating HAProxy config, reload HAProxy:
```haproxy
sudo systemctl reload haproxy
```
## 5. Testing & Verification
Access the load balancer URL (app.appshadayoisthegoat.kesug.com)

Refresh the page multiple times

Confirm the responses alternate between Web01 and Web02 (check the app logs or subtle UI differences if available)

## 6. APIs and Credits

### APIs Used
- **OpenWeather API** for weather data  
  Official docs: [https://openweathermap.org/api](https://openweathermap.org/api)

### Challenges Faced
- Handling asynchronous API calls in JavaScript for smooth UI updates  
- Ensuring CORS was properly configured to avoid cross-origin errors  
- Managing API key security by using environment variables instead of hardcoding

### How I Overcame Them
- Used `fetch` with async/await patterns to manage API calls cleanly  
- Leveraged CORS proxies during development and configured backend appropriately  
- Kept API keys in environment variables and never committed them to the repo

### Credits
- Thanks to OpenWeather for providing a robust free API  
- Used Nginx as a lightweight web server inside the Docker container  
- Inspired by tutorials from [insert any blog or video resource you used]



## 7. Summary
This README lets anyone build, deploy, load balance, and test my app just like I did.
