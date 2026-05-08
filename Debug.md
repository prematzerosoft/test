I have a Kubernetes pod in a managed AKS (Azure Kubernetes Service) dev environment that takes ~15 minutes to start.
In a dedicated AKS cluster the same pod starts in ~5 seconds.

I have attached two diagnostic files:
1. describePod   — output of: kubectl describe pod <pod-name> -n <namespace>
2. getevents     — output of: kubectl get events -n <namespace> --sort-by=.lastTimestamp

Please analyze both files and:

1. Identify the exact phase where time is lost (Pending → Scheduled → Pulling → Running → Ready).
2. Check if the delay is caused by:
   - Node provisioning / cluster autoscaler scale-up (no available node)
   - Image pull time (large image, wrong region, missing credentials)
   - Init containers or lifecycle hooks taking too long
   - PVC / volume attach delays
   - Readiness or liveness probe failures holding back Ready state
   - Resource request/limit mismatches causing scheduling delays
   - Network policy or DNS resolution delays
3. For each root cause found, give:
   - The specific event or field in the output that proves it
   - A concrete fix (kubectl command, YAML config change, or Azure portal action)
4. Prioritize fixes by impact — what single change would give the biggest reduction in startup time?
5. Flag any unrelated warnings or errors in the output that could cause future problems.
