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

## Storage / delivery model

```text
User / application request
        ↓
Go application
        ↓
metadata / control state
        ↘
       SQLite
        ↓
object upload / lookup
        ↓
Amazon S3
        ↓
CloudFront / delivery layer
        ↓
Client fetches media / asset
```

A useful architectural distinction is between **application metadata**, **file bytes**, **derived media**, and **delivery state**. The UI may present these as one feature, but the system has to keep them consistent across multiple layers.

That separation creates product and system questions around authorization, object naming, cache behavior, invalidation, upload failure, media processing, cost, stale content, and cleanup.

## Why object storage, metadata, processing, and CDN are separate concerns

- **Application metadata** answers: what does the product know about the asset — owner, status, references, processing state, permissions, etc.?
- **Object storage** answers: where should durable file bytes live, and how should the application address them?
- **Media processing** answers: how do source files become thumbnails, previews, transcoded files, or other derived assets?
- **CDN / delivery** answers: how should those files be delivered efficiently to users, potentially from caches closer to them?

For Product / TPM work, the useful question is not simply “do we use S3?” but **what lifecycle, permission, latency, reliability, freshness, and cost behavior does the product require?**

## Asset lifecycle model

One product-level way to reason about a media asset is:

```text
Upload requested
   ↓
Metadata record created
   ↓
Object stored
   ↓
Processing / inspection
   ↓
Derived assets ready
   ↓
Delivery available
   ↓
Update / replace / delete
   ↓
Metadata + source + derived objects + caches reconciled
```

The important lesson is that a successful API response at one step does not guarantee the whole asset lifecycle is healthy. A robust product design needs explicit states and recovery paths for partial failure.

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

## Why it matters to my product / technical work

This project helps me reason about the path from a backend application to **durable file storage and content delivery infrastructure**. It complements my work on HTTP APIs, PostgreSQL, Docker, RabbitMQ, and other backend fundamentals by adding object storage, media workflows, and CDN concepts.

It is particularly useful for discussing:

- whether file bytes belong in the database, local disk, or object storage;
- how upload / processing / delivery can become separate product states;
- how permissions and signed-access patterns affect product behavior;
- what happens when metadata succeeds but a file upload fails, or vice versa;
- how source and derived assets should be linked and cleaned up;
- how caching can improve delivery while creating staleness / invalidation questions;
- how media processing adds asynchronous failure and observability requirements;
- how storage and bandwidth choices affect both cost and user experience.

## Reviewer guide

A useful discussion of this repository should be able to answer:

1. Why use object storage instead of storing large files directly in a relational database?
2. What product state should live in the application database versus S3?
3. What can go wrong between upload, metadata persistence, processing, and delivery?
4. How should source and derived assets behave when a user replaces or deletes a file?
5. When can CDN caching produce stale or unauthorized behavior?
6. How would private files differ from public assets in a production design?
7. What monitoring, retry, access-control, and lifecycle policies would be needed before this became production-ready?

## Evidence boundary

This repository supports claims about foundational **Go file handling, S3 object-storage concepts, CloudFront/CDN concepts, media-processing workflows, asset lifecycle reasoning, and storage/delivery architecture reasoning**. It does **not** establish production cloud-architecture ownership, high-scale CDN operations, advanced AWS security expertise, production SRE experience, or cost optimization at scale.

The intended signal is **storage / delivery architecture literacy and implementation exposure**, not cloud-architect expertise.

## Context

Course: Boot.dev — Learn File Servers and CDNs with S3 and CloudFront

Related technical foundations I am building: **Go · HTTP · file storage · S3 · CloudFront · AWS fundamentals · media processing · backend systems**.

For stronger product ownership evidence, see the Haven Product Portfolio:
https://somber-tamarillo-df3.notion.site/Haven-AI-native-Product-Portfolio-3d8ad9856018811b96ffe8e33a7d48ef
