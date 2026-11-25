# Deploy NeonClipping to Render (Docker)

## Important limitations

- This configuration is CPU-only (`NEONCLIPPING_CPU=1`). Render's Free web service has limited RAM/CPU, sleeps when idle, and has ephemeral storage. Large uploads, long videos, Whisper transcription, concurrent users, and live clipping may fail or be very slow.
- The app currently has no built-in public-user authentication or per-user quotas. Do not expose it to unrestricted public traffic: anyone who can reach the URL may be able to submit work and consume the service's resources. For a public launch, add authentication, rate limits, upload limits, job limits, and durable object storage first.
- Files stored in `/var/data/neonclipping` are not durable on Render Free. A paid persistent disk or external object storage is needed to retain clips across restarts/redeploys.
- This is a deployment starter, not a guarantee that every feature works on Render's Free plan. GPU acceleration is not provided by this Dockerfile.

## Deploy

1. Push the complete contents of this repository (including `Dockerfile`, `render.yaml`, and `.dockerignore`) to a GitHub repository.
2. In Render, choose **New + → Blueprint** and connect that repository. Render reads `render.yaml`.
   - Alternatively choose **New + → Web Service**, connect the repository, select **Docker**, and use the repository root as the Docker context.
3. Deploy. The container starts with:

   `uvicorn app:app --host 0.0.0.0 --port ${PORT:-10000}`

4. When deploy completes, open the `https://<service-name>.onrender.com` URL.

## Before making it public

The upstream app is designed as a local single-user application. This configuration does not add authentication. Keep the service private/restricted until authentication, request throttling, upload-size limits, and persistent storage have been implemented and tested.
