---
name: 42edu:devops
description: DevOps deployment skill for online education platforms with video streaming and CDN optimization
---

# 运维部署技能 - 42edu:devops

## 技能概述

专为在线教育平台设计的DevOps技能，涵盖视频流媒体服务、CDN加速、高并发直播架构、容器化部署等核心运维能力。

---

## 适用场景

- 在线教育平台生产环境部署
- 视频点播（VOD）服务搭建
- 直播流媒体服务部署
- 高并发学习场景优化
- 教育SaaS多租户部署

---

## 核心能力

### 1. 容器化部署
- Docker镜像构建和优化
- Kubernetes集群配置
- 微服务编排
- 服务网格（Service Mesh）

### 2. 视频服务部署
- 视频转码服务
- HLS/DASH流媒体服务
- 视频存储和分发
- 直播推拉流服务

### 3. CDN配置
- 静态资源加速
- 视频点播加速
- 直播流加速
- 跨境传输优化

### 4. 性能优化
- 负载均衡配置
- 自动扩缩容
- 缓存策略优化
- 数据库读写分离

---

## 输入要求

### 必需文件
- `.42cog/real/real.md` - 性能和可用性约束
- `.42cog/spec/dev/sys.spec.md` - 系统架构规约

### Real约束示例
```markdown
<real>
- 视频播放必须支持多码率切换
- 视频缓冲时间必须小于3秒
- 系统必须支持1000人同时在线
- 数据库必须每日备份
</real>
```

---

## 输出内容

### 1. Docker配置

**Dockerfile示例**：
```dockerfile
# 前端应用
FROM node:18-alpine AS frontend
WORKDIR /app
COPY package*.json ./
RUN npm ci --only=production
COPY . .
RUN npm run build

FROM nginx:alpine
COPY --from=frontend /app/dist /usr/share/nginx/html
COPY nginx.conf /etc/nginx/nginx.conf
EXPOSE 80
```

**docker-compose.yml**：
```yaml
version: '3.8'
services:
  web:
    build: ./web
    ports:
      - "3000:3000"
    environment:
      - DATABASE_URL=${DATABASE_URL}
      - REDIS_URL=${REDIS_URL}
    depends_on:
      - db
      - redis
  
  db:
    image: postgres:14-alpine
    volumes:
      - postgres_data:/var/lib/postgresql/data
    environment:
      - POSTGRES_PASSWORD=${DB_PASSWORD}
  
  redis:
    image: redis:7-alpine
    volumes:
      - redis_data:/data

volumes:
  postgres_data:
  redis_data:
```

### 2. Kubernetes部署

**deployment.yaml**：
```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: edu-platform
spec:
  replicas: 3
  selector:
    matchLabels:
      app: edu-platform
  template:
    metadata:
      labels:
        app: edu-platform
    spec:
      containers:
      - name: web
        image: registry.example.com/edu-platform:latest
        ports:
        - containerPort: 3000
        env:
        - name: DATABASE_URL
          valueFrom:
            secretKeyRef:
              name: db-secret
              key: url
        resources:
          requests:
            memory: "256Mi"
            cpu: "250m"
          limits:
            memory: "512Mi"
            cpu: "500m"
---
apiVersion: v1
kind: Service
metadata:
  name: edu-platform-service
spec:
  selector:
    app: edu-platform
  ports:
  - protocol: TCP
    port: 80
    targetPort: 3000
  type: LoadBalancer
```

**HPA（水平自动扩缩容）**：
```yaml
apiVersion: autoscaling/v2
kind: HorizontalPodAutoscaler
metadata:
  name: edu-platform-hpa
spec:
  scaleTargetRef:
    apiVersion: apps/v1
    kind: Deployment
    name: edu-platform
  minReplicas: 3
  maxReplicas: 10
  metrics:
  - type: Resource
    resource:
      name: cpu
      target:
        type: Utilization
        averageUtilization: 70
```

### 3. CI/CD流水线

**GitHub Actions示例**：
```yaml
name: Deploy to Production

on:
  push:
    branches: [ main ]

jobs:
  build-and-deploy:
    runs-on: ubuntu-latest
    steps:
    - uses: actions/checkout@v3
    
    - name: Set up Node.js
      uses: actions/setup-node@v3
      with:
        node-version: '18'
    
    - name: Install dependencies
      run: npm ci
    
    - name: Run tests
      run: npm test
    
    - name: Build Docker image
      run: |
        docker build -t registry.example.com/edu-platform:${{ github.sha }} .
        docker tag registry.example.com/edu-platform:${{ github.sha }} registry.example.com/edu-platform:latest
    
    - name: Push to registry
      run: |
        echo ${{ secrets.REGISTRY_PASSWORD }} | docker login registry.example.com -u ${{ secrets.REGISTRY_USERNAME }} --password-stdin
        docker push registry.example.com/edu-platform:${{ github.sha }}
        docker push registry.example.com/edu-platform:latest
    
    - name: Deploy to Kubernetes
      run: |
        kubectl set image deployment/edu-platform web=registry.example.com/edu-platform:${{ github.sha }}
        kubectl rollout status deployment/edu-platform
```

### 4. 视频服务配置

**视频转码服务（FFmpeg）**：
```yaml
apiVersion: batch/v1
kind: Job
metadata:
  name: video-transcode
spec:
  template:
    spec:
      containers:
      - name: ffmpeg
        image: jrottenberg/ffmpeg:latest
        command: 
        - ffmpeg
        - -i
        - /input/video.mp4
        - -vf
        - scale=-2:720
        - -c:v
        - libx264
        - -c:a
        - aac
        - /output/video_720p.mp4
        volumeMounts:
        - name: video-storage
          mountPath: /input
        - name: output-storage
          mountPath: /output
      volumes:
      - name: video-storage
        persistentVolumeClaim:
          claimName: video-pvc
      - name: output-storage
        persistentVolumeClaim:
          claimName: output-pvc
      restartPolicy: OnFailure
```

**HLS流媒体服务（Nginx-RTMP）**：
```nginx
rtmp {
    server {
        listen 1935;
        chunk_size 4096;

        application live {
            live on;
            record off;
            
            # HLS配置
            hls on;
            hls_path /tmp/hls;
            hls_fragment 3s;
            hls_playlist_length 60s;
        }
    }
}

http {
    server {
        listen 8080;
        
        location /hls {
            types {
                application/vnd.apple.mpegurl m3u8;
                video/mp2t ts;
            }
            root /tmp;
            add_header Cache-Control no-cache;
            add_header Access-Control-Allow-Origin *;
        }
    }
}
```

---

## CDN配置

### 阿里云CDN配置
```bash
# 创建CDN域名
aliyun cdn AddCdnDomain \
  --DomainName video.example.com \
  --CdnType web \
  --SourceType oss \
  --Sources '[{"content":"edu-videos.oss-cn-beijing.aliyuncs.com","type":"oss","port":80}]'

# 配置缓存规则
aliyun cdn BatchSetCdnDomainConfig \
  --DomainNames video.example.com \
  --Functions '[
    {
      "functionName":"set_req_header",
      "functionArgs":[
        {"argName":"header_operation_type","argValue":"add"},
        {"argName":"header_name","argValue":"Cache-Control"},
        {"argName":"header_value","argValue":"max-age=31536000"}
      ]
    }
  ]'
```

### CloudFlare配置
```javascript
// workers脚本 - 视频加速
addEventListener('fetch', event => {
  event.respondWith(handleRequest(event.request))
})

async function handleRequest(request) {
  const url = new URL(request.url)
  
  // 缓存策略
  const cacheKey = new Request(url.toString(), request)
  const cache = caches.default
  
  let response = await cache.match(cacheKey)
  
  if (!response) {
    response = await fetch(request)
    
    // 视频文件长期缓存
    if (url.pathname.match(/\.(mp4|m3u8|ts)$/)) {
      response = new Response(response.body, response)
      response.headers.set('Cache-Control', 'public, max-age=86400')
      event.waitUntil(cache.put(cacheKey, response.clone()))
    }
  }
  
  return response
}
```

---

## 监控和告警

### Prometheus监控
```yaml
# prometheus.yml
scrape_configs:
  - job_name: 'edu-platform'
    kubernetes_sd_configs:
    - role: pod
    relabel_configs:
    - source_labels: [__meta_kubernetes_pod_label_app]
      regex: edu-platform
      action: keep
```

### Grafana仪表盘指标
- 在线用户数
- 视频播放成功率
- 视频缓冲时间
- API响应时间
- 数据库连接池使用率
- 服务器CPU/内存使用率

### AlertManager告警规则
```yaml
groups:
- name: edu-platform
  rules:
  - alert: HighErrorRate
    expr: rate(http_requests_total{status=~"5.."}[5m]) > 0.05
    for: 5m
    annotations:
      summary: "High error rate detected"
  
  - alert: VideoBufferingHigh
    expr: avg(video_buffer_time_seconds) > 3
    for: 5m
    annotations:
      summary: "Video buffering time > 3s"
```

---

## 备份和恢复

### 数据库备份
```bash
#!/bin/bash
# backup.sh

DATE=$(date +%Y%m%d_%H%M%S)
BACKUP_DIR="/backups/postgres"

# 备份数据库
pg_dump -h localhost -U postgres edu_platform > $BACKUP_DIR/backup_$DATE.sql

# 压缩备份文件
gzip $BACKUP_DIR/backup_$DATE.sql

# 上传到对象存储
aws s3 cp $BACKUP_DIR/backup_$DATE.sql.gz s3://edu-backups/postgres/

# 删除7天前的备份
find $BACKUP_DIR -name "*.sql.gz" -mtime +7 -delete
```

### Kubernetes备份
```bash
# 备份所有资源
kubectl get all --all-namespaces -o yaml > cluster-backup.yaml

# 使用Velero备份
velero backup create edu-platform-backup \
  --include-namespaces edu-platform \
  --storage-location default
```

---

## 技术栈

### 容器编排
- Docker 20.10+
- Kubernetes 1.24+
- Helm 3.0+

### 流媒体
- Nginx-RTMP
- FFmpeg
- HLS.js（前端播放器）

### 对象存储
- 阿里云OSS
- AWS S3
- MinIO（私有化部署）

### CDN
- 阿里云CDN
- 腾讯云CDN
- CloudFlare

---

## 使用方法

```bash
# 生成DevOps部署规约
42cog g spec --role tech --skill devops --plugin 42edu

# 输出文件：.42cog/spec/dev/devops.spec.md
```

---

## 相关技能

- `42edu:db` - 数据库设计和优化
- `42cog:tech:sys` - 系统架构设计
- `42cog:tech:qa` - 质量保证和测试

---

**42edu:devops** - 专业的教育平台运维部署
