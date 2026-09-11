# Tubely — Go File Storage, S3 & CloudFront Learning Project

A Boot.dev course project for learning **file-serving architecture, object storage, media processing, and CDN delivery** with Go, Amazon S3, and CloudFront.

This repository is part of my backend-systems learning path. It is **learning / implementation evidence**, not a claim of production-scale cloud expertise.

## What this project explores

- serving and managing files from a Go application
- local media / asset handling
- object storage with Amazon S3
- CDN concepts with CloudFront
- media inspection / processing with FFmpeg and FFprobe
- application configuration through environment variables
- SQLite-backed local application state

## Local setup

### Requirements

- Go
- FFmpeg / FFprobe
- SQLite 3 (for manual database inspection)
- AWS CLI

### Install dependencies

```bash
go mod download
```

On macOS:

```bash
brew update
brew install ffmpeg sqlite3
```

### Download sample media

```bash
./samplesdownload.sh
```

### Configure environment variables

```bash
cp .env.example .env
```

Fill in the required values for your local / AWS configuration.

### Run

```bash
go run .
```

The application creates local state such as a SQLite database and an assets directory as part of the course workflow.

## Why it matters to my learning

This project helps me understand the path from a backend application to **durable file storage and content delivery infrastructure**. It complements my work on HTTP APIs, PostgreSQL, Docker, RabbitMQ, and other backend fundamentals by adding object storage, media workflows, and CDN concepts.

## Context

Course: Boot.dev — Learn File Servers and CDNs with S3 and CloudFront

Related technical foundations I am building: **Go · HTTP · file storage · S3 · CloudFront · AWS fundamentals · media processing · backend systems**.
