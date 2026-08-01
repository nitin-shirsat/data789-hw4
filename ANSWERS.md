# HW4 — Short Answers

Answer each question in 2–3 sentences.

## 1. Rolling-update safety
Why does the Deployment use `maxUnavailable: 0`, and what would change if it were `maxUnavailable: 1`?

> `maxUnavailable: 0` ensures that Kubernetes keeps all existing ready pods available until replacement pods become ready, which helps prevent dropped requests. With `maxUnavailable: 1` Kubernetes could take one existing pod offline before its replacement is ready, temporarily reducing capacity.


## 2. Health probes
Why do the liveness/readiness probes target `/health` instead of `/predict`?

> The `/health` endpoint is a lightweight operation designed to verify that the service is alive and ready without performing an actual prediction. The `/predict` endpoint requires transaction input and performs inference, so using it as a health check would waste resources and could create misleading probe failures.


## 3. HPA
Your HPA scales at 40% CPU up to 8 pods — if request volume doubled, what would you expect to happen, and what happens once it reaches the maximum?
> If request volume doubles and CPU utilization rises above 40%, the HPA will add pods until average CPU utilization moves toward the target. Once the deployment reaches eight pods, it cannot scale further, so additional traffic may cause higher latency or errors unless the maximum is increased or each pod receives more resources.