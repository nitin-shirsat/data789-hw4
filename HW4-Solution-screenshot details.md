# DATA 789 Homework 4
Nitin Shirsat (`nshirsat`)  

## 1. Kubernetes deployment and autoscaling

Verified three ready replicas, running Redis and fraud pods, a LoadBalancer Service, HPA at `2%/40%` with 3-8 pods.

![Kubernetes deployment, Service, HPA, and labels](screenshots/01-kubernetes-ready-hpa-labels.png)

## 2. Deployment configuration

Confirmed `maxUnavailable: 0`, `maxSurge: 1`, CPU/memory limits of `250m/192Mi`, requests of `100m/128Mi`, `/health` probes, and Redis configuration.

![Deployment configuration](screenshots/02-deployment-configuration.png)

## 3. Local health check 

Confirmed that `http://localhost:8080/health` returned `{"status":"ok"}`.

![Local health endpoint](screenshots/03-health-endpoint.png)

## 4. Kubernetes port forwarding 

Forwarded `localhost:8080` to the `trustbank-fraud` Service and confirmed that Kubernetes handled incoming connections.

![Kubernetes port forwarding](screenshots/04-port-forward.png)

## 5. Local prediction test 

Ran the supplied smoke test and received HTTP `200` with a valid fraud prediction from `/predict`.

![Successful local prediction](screenshots/05-local-predict-200.png)

## 6. Supplemental demonstration 

Recorded the Kubernetes demonstration as supplemental evidence: [open recording](screenshots/06-self-healing-recording.mov).

## 7. Intentional pod deletion

Deleted one fraud-service pod to trigger self-healing.

![Intentional pod deletion](screenshots/07-pod-deletion.png)

## 8. Self-healing recovery 

Kubernetes replaced the deleted pod through Pending, Creating, and Running states and restored the Deployment to `3/3`.

![Self-healing recovery](screenshots/08-self-healing-recovery.png)

## 9. Zero-downtime rolling update 

Rolled the service to `ghcr.io/rfsalas/trustbank-fraud:v2` while all 25 test requests succeeded with `dropped: 0`.

![Rolling update with zero dropped requests](screenshots/09-rolling-update-zero-drops.png)

## 10. Continuous rollout traffic 

Repeated Service connections during the rolling-update request loop.

![Continuous request forwarding](screenshots/10-live-request-forwarding.png)

## 11. Blue deployment troubleshooting 

Diagnosed the invalid auto-generated Redis port, set `REDIS_PORT=6379`, and successfully completed the blue Deployment rollout.

![Blue deployment Redis fix](screenshots/11-blue-redis-fix.png)

## 12. Green deployment validation 

Applied the Redis correction to green and verified all 3 blue and 3 green pods were `1/1 Running`.

![Blue and green pods ready](screenshots/12-green-rollout-ready.png)

## 13. Pre-cutover green test 

Privately tested green on port `9000` and received successful `/health` and `/predict` responses with HTTP `200`.

![Successful green prediction](screenshots/13-green-predict-200.png)

## 14. Blue-green cutover and rollback 

Switched the Service selector from blue to green, verified the green endpoints, and rolled traffic back to blue.

![Blue-green cutover and rollback](screenshots/14-blue-green-cutover-rollback.png)

## 15. Azure Container Apps deployment 

After resolving the Azure region/resource-group conflict, deployed the API to EastUS region.

![Azure deployment](screenshots/15-azure-deployment.png)

## 16. Azure endpoint verification 

Verified the public Azure `/health` endpoint and received a valid fraud decision from `/predict`.

![Azure health and prediction](screenshots/16-azure-health-predict.png)

## 17. Azure smoke test 

Ran the given smoke-test script against the Container App and received HTTP `200`.

![Successful Azure smoke test](screenshots/17-azure-smoke-test.png)

## 18. ACR and Container App verification 

Confirmed the `trustbank-fraud:v1` ACR tag and the running `trustbank-fraud` Container App with its public FQDN.

![ACR and Container App verification](screenshots/18-acr-container-app-verification.png)

## 19. Azure cleanup activity 

Verified in the Azure activity log that deletion of the generated Log Analytics workspace succeeded.

![Azure activity log cleanup](screenshots/19-azure-activity-log-cleanup.png)

## 20. Azure teardown confirmation 

Ran the teardown script, waited for resource-group deletion, and confirmed `az group exists` finally returned `false`.

![Azure teardown confirmation](screenshots/20-azure-teardown-confirmation.png)
