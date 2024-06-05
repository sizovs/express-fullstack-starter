# What is this?

This is my starter for full-stack web app development on Node.

JS and TS ecosystem suffers from extreme churn, making migrations between major framework releases extremely challenging, often forcing 'a big rewrite' on app developers every 3-5 years. If  you're playing long-term game, it's better and safer to build on top of stable and boring foundation. Moreover, the SPA ecosystem, and frameworks like Next and SvelteKit, are complex beasts with too much hidden "magic" under the hood. That magic works until it doesn't. For the problem of sending data over HTTP to and from the database, I find that complexity hard to justify. By making certain architectural trade-offs, such as embracing [hypermedia systems](https://hypermedia.systems/) and ditching unneccessary abstractions, it's possible to eliminate all that accidental complexity.

I don't think it's reasonable to split apps prematurely across all axes — 1) vertically into microservices, 2) horizontally into BE and FE, and 3) across 'tiers' with DB running on a separate machine. Instead, I prefer building self-contained, monolith systems that run on a single server. Such systems can handle [10,000s of requests on a beefy VPS](https://blog.wesleyac.com/posts/consider-sqlite) (which is enough for most apps) and can be split into multiple self-contained systems for scalability, if necessary.

Loosely coupled, distributed architectures are challenging to operate, which is why they are better suited for running on the cloud. This is one of the reasons why cloud providers advocate for such architectures. On the other hand, monolithic, self-contained architectures diminish the benefit of running on PaaS or cloud hyperscalers that are opaque and insanely expensive abstractions over good old servers. Thanks to deliberate architectural simplification, I can easily run my apps on a single Hetzner VPS. I created a number of custom scripts for database migrations, zero-downtime deployments, and infrastructure provisioning (I find Terraform state management a hassle and HCL syntax too restrictive). Don't shy away from coding a bit yourself.

As a guiding principles, I bet on stability, simplicity, and fewer abstractions. As a result, I made the following tech choices:
* JS (you don't need TS for large-scale apps if you write tests; rails is a great example)
* Express.js
* Edge.js for templating
* Htmx for SPA experience
* Alpine.js for sprinkling JS here and there
* CSS for styling w/o build tools
* Playwright for E2E tests
* SQLite with better-sqlite3 for DB acccess w/o ORMs and query builders (nobody switches databases anyway)
* Litestream for streaming DB replication

By looking into the code, you'll discover lesser-known gems such as Vine validation library. Generally, the dependencies are minimal, giving a refreshing feel after dealing with bloated frameworks.

# Before deployment
Create `<project_dir>/.env` with the following content:
```
# Path to a public key used for accessing your VPS. Optional, defaults to ~/.ssh/hetzner.pub
PUBLIC_KEY=<value>

# Your public domain such as sizovs.net. Optional, defaults to <server ip>.nip.io
DOMAIN=<value>
```

# Provisioning infra
```
HETZNER_API_TOKEN=<secret goes here> task infra
```

If you're using a custom app domain, point your DNS records to the IP address of your Hetzner VPS.

# Deploying to production

```
SERVER_IP=<server ip> task deploy
```

🎉 Your app should be publicly available via HTTPS on your custom domain or via `<server ip>.nip.io`.

# Running locally
```
task dev
```

# Running tests
```
task test
```

# Pulling production DB to local dev
```
task db:pull
```

# Troubleshooting on prod
```
DB_LOCATION=<db location> npm run repl
```



