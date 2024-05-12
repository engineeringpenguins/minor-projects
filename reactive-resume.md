# Reactive Resume Notes

## Introduction

[Reactive Resume](https://github.com/AmruthPillai/Reactive-Resume) is an open source project providing an easy to use website to create professional resumes. The application has no telemetry and is intended to be self hosted and easily deployed/managed but can also be used from the [reactive resume website](https://rxresu.me/).  

## Setup

I use docker for this as its a temporary service and it doenst make sense from a time perspective for me to build it manually. Only things you would want to change in this compose file would be the exposed ports, static url, or adding a timezone variable.  

### Docker Compose

```
version: "3.8"

services:
  postgres:
    image: postgres:16-alpine
    restart: unless-stopped
    volumes:
      - postgres_data:/var/lib/postgresql/data
    environment:
      POSTGRES_DB: postgres
      POSTGRES_USER: postgres
      POSTGRES_PASSWORD: postgres
    healthcheck:
      test: ["CMD-SHELL", "pg_isready -U postgres -d postgres"]
      interval: 10s
      timeout: 5s
      retries: 5

  minio:
    image: minio/minio
    restart: unless-stopped
    command: server /data
    ports:
      - "9000:9000"
    volumes:
      - minio_data:/data
    environment:
      MINIO_ROOT_USER: minioadmin
      MINIO_ROOT_PASSWORD: minioadmin

  chrome:
    image: ghcr.io/browserless/chromium:latest
    restart: unless-stopped
    environment:
      TIMEOUT: 10000
      CONCURRENT: 10
      TOKEN: chrome_token
      EXIT_ON_HEALTH_FAILURE: true
      PRE_REQUEST_HEALTH_CHECK: true

  app:
    image: amruthpillai/reactive-resume:latest
    restart: unless-stopped
    ports:
      - "3000:3000"
    depends_on:
      - postgres
      - minio
      - chrome
    environment:
      PORT: 3000
      NODE_ENV: production
      PUBLIC_URL: http://localhost:3000
      STORAGE_URL: http://localhost:9000/default
      CHROME_TOKEN: chrome_token
      CHROME_URL: ws://chrome:3000
      DATABASE_URL: postgresql://postgres:postgres@postgres:5432/postgres
      ACCESS_TOKEN_SECRET: access_token_secret
      REFRESH_TOKEN_SECRET: refresh_token_secret
      MAIL_FROM: noreply@localhost
      # SMTP_URL: smtp://user:pass@smtp:587 # Optional
      STORAGE_ENDPOINT: minio
      STORAGE_PORT: 9000
      STORAGE_REGION: us-east-1 # Optional
      STORAGE_BUCKET: default
      STORAGE_ACCESS_KEY: minioadmin
      STORAGE_SECRET_KEY: minioadmin
      STORAGE_USE_SSL: false

volumes:
  minio_data:
  postgres_data:
```

### Initial Config

An account is required to use the application even if self hosted, the account will exist only on your instance. Likewise an account made on the [reactive resume website](https://rxresu.me/) will not exist on anything locally hosted.  

1. Navigate to port 3000 on your [local machine](http://localhost:3000/auth/register)
   1. Click "Get Started"
   2. Click "Create an account"
2. Provide email address and desired password
   - Can use a fake email
3. Proceed to the dashboard without verifying email
   - Verify email later if needed
4. Click Settings in the upper left
5. Set up MFA and your OpenAPI key here

### Resume Setup

1. Open the Resume's dashboard in the upper left
2. Click 'Create a new Resume'
3. Name the resume and continue by clicking 'Create'
4. Use the left hand menu to add sections and content
5. Use the right hand menus to eddit formatting and presentation

![Resume Editor](https://github.com/engineeringpenguins/minor-projects/blob/main/images/resume/resume_edit.png)  

## Thoughts

The resume templates and examples that come with the technology provide a decent variation of popular resume presentations and it can be integrated with GPT for AI powered resume generation and review. Overall worth using over any of the low end paid services to at least get content down in a presentable way.   