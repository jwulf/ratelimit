# Rate Limiter

A functional rate limiter to avoid saturating external resources.

## Install

```bash
npm i @sitapati/ratelimiter
```

## Usage

The following example executes ten REST calls with 500ms between each call.

```typescript
import { RateLimiter } from "@sitapati/ratelimiter"

const limit = new RateLimiter(500)

for (let i = 0; i < 10; i++) { 
    limit.runRateLimited({ 
        task: () => RESTcall(req)
    })
    .catch(console.error)
    .then(console.log)
}
```

You can mark a task as preemptible. This is useful when you have some calls that should take a higher priority - for example, tasks that impact user feedback. In this case, you would mark your low-priority (like async service calls for the backend) as `preemptible`:

```typescript
limit.runRateLimited({
    task: () => backgroundTask(req),
    preemptible: true
})

limit.runRateLimited({
    task: () => foregroundTask(req)
})
```
