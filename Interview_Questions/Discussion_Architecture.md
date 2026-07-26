# DE/DataOps Interview — General Conceptual Questions 

10 non-tool-specific questions

1. **What's the difference between a data engineer and a data analyst, in your own words?**

2. **How do you approach designing a pipeline when the requirements aren't fully clear yet?**

3. **What does "data freshness" mean, and how would you decide what freshness SLA a pipeline needs to meet?**

4. **How would you handle a situation where two different upstream sources disagree on the same value?**

5. **What's the difference between a fact table and a dimension table?**

6. **How do you think about cost when designing a pipeline — what typically drives cost up?**

7. **What does "single source of truth" mean, and why does it matter in a data platform?**

8. **How would you approach a stakeholder who wants "real-time" data but the underlying source only updates once a day?**

9. **What's the risk of a pipeline having no owner or documentation, and how do you avoid that on your own projects?**

10. **How do you decide when a pipeline problem is worth fully automating a fix for, versus just handling it manually when it comes up?**

---

# Discussion/Architecture

## HIGH PROBABILITY — have a real story ready

### Apache Airflow
1. How would you design a DAG so that one failing task doesn't block unrelated downstream tasks from running?
2. What's your process for testing a DAG before deploying it to production?
3. How do you handle a task that depends on a previous day's data being ready (e.g., "don't run today's aggregation until yesterday's ingestion succeeded")?
4. If a DAG needs to run hourly but a single run sometimes takes longer than an hour, how do you prevent overlapping runs?
5. How would you explain your DAG structure to a non-technical stakeholder who wants to know "when will today's data be ready"?

### Docker / Docker Compose
1. If a teammate says "it works on my machine but not in the container," what's your first move to figure out why?
2. How do you handle a service in Docker Compose that needs a config file or secret it shouldn't have baked into the image?
3. What's the difference between `docker-compose up` and `docker-compose up --build`, and when does that distinction bite you?
4. How would you set up Docker Compose so your Python app doesn't crash on startup because Postgres isn't ready yet?
5. If your Docker image is 2GB and a colleague's equivalent image is 300MB, what would you check first?

### GitHub Actions
1. How would you set up a workflow so tests only run on files that actually changed, instead of the whole repo every time?
2. What's the difference between a workflow that runs on `push` versus one that runs on `pull_request`, and why would you choose one over the other?
3. How do you debug a GitHub Actions run that's failing but gives you a vague error message?
4. How would you set up a workflow so a deploy only happens after tests pass, not in parallel with them?
5. What would you check first if a workflow suddenly stopped triggering at all?

### Logging + Sentry
1. How would you decide what log level (info, warning, error) a given message should be?
2. If your logs are full of noise and it's hard to find the actual problem during an incident, what would you change?
3. How do you make sure sensitive data (like customer emails or API keys) never accidentally ends up in a log?
4. What's the difference between logging an error and raising an alert — shouldn't every error be an alert?
5. How would you set up logging so you can trace one specific pipeline run across multiple tasks or services?

### PostgreSQL (design/architecture)
1. How would you decide whether to use a single large table versus splitting data across multiple related tables?
2. What would make you consider partitioning a table, and how would you decide the partition key?
3. How do you think about foreign keys and constraints — do you always enforce them, or are there tradeoffs?
4. If a query that used to be fast suddenly becomes slow as a table grows, what would you investigate first?
5. How would you design a table so that re-running an ingestion job twice doesn't create duplicate rows?

### Git / GitHub
1. How do you decide when to squash commits versus keeping the full commit history in a PR?
2. What's your process when you accidentally commit something sensitive, like an API key?
3. How do you handle working on a feature that takes several days while `main` keeps moving forward?
4. What's the difference between `git merge` and `git rebase`, and when would you use each?
5. How would you structure a repo for a project with multiple related services (e.g., ingestion, transformation, API)?


## MODERATE PROBABILITY — one solid sentence, folds into other answers

### python-dotenv
1. What would you do differently for secrets in a team setting versus working solo?
2. How do you make sure a new teammate knows which environment variables they need to set up?
3. What's a downside of relying entirely on `.env` files for configuration?
4. How do you handle a secret that needs to rotate periodically (like an API key that expires)?
5. Would you ever commit a `.env` file — under what circumstance, if any?

### Makefile
1. How do you keep a Makefile from becoming outdated as a project's setup steps change?
2. What's an example of a Makefile target you'd consider risky to run accidentally (like one that deletes data)?
3. How do you document what each Makefile target actually does?
4. Would you use a Makefile in a project a non-technical teammate needs to interact with? Why or why not?
5. How does a Makefile fit alongside Docker in your workflow — do they overlap?

### YAML
1. How do you handle a YAML config value that needs to differ between environments?
2. What's a readability tradeoff you've noticed with deeply nested YAML?
3. How would you catch a YAML syntax error before it causes a runtime failure?
4. Why might a team choose YAML over a `.env` file or a Python config file for certain settings?
5. Have you used YAML anchors or references, and what are they useful for?

### bash / Linux / shell
1. How do you check whether a service is actually running versus just installed?
2. What's your approach to reading and interpreting a stack trace from the terminal?
3. How would you check what port a specific process is using?
4. What's a bash command you'd use to quickly search for a string across multiple files?
5. How do you check environment variables currently set in your shell session?


## LOW PROBABILITY — surface-level only

### AWS/GCP basics
1. What's the difference between a managed database service (like RDS) and running Postgres yourself on a VM?
2. Have you dealt with cloud storage costs — what drives cost up unexpectedly?
3. What's a basic reason a team might choose multi-region deployment?
4. What's the difference between compute (like EC2) and storage (like S3) in terms of what you're paying for?
5. How would you approach estimating cost for a new cloud-based pipeline before building it?

### Networking & Security Basics
1. What's the purpose of an API gateway, at a basic level?
2. Why might a company use a VPN or private network to connect to a cloud database instead of a public endpoint?
3. What's a basic difference between symmetric and asymmetric encryption?
4. Why is rotating credentials periodically considered good practice?
5. What's the risk of using the same credentials across dev, staging, and production?