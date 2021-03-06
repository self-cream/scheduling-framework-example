# scheduling-framework-example

This is a kubernetes scheduling-framework example.
It builds in kubernetes v1.20.0.

## A Scheduler Config File based on scheduling-framework likes this:
```yaml
apiVersion: kubescheduler.config.k8s.io/v1beta1
kind: KubeSchedulerConfiguration
clientConnection:
  burst: 100
  qps: 50
leaderElection:
  leaderElect: true
  leaseDuration: 15s
  renewDeadline: 10s
  resourceLock: leases
  resourceName: scheduling-framework-example
  resourceNamespace: kube-system
  retryPeriod: 2s
percentageOfNodesToScore: 0
podInitialBackoffSeconds: 1
podMaxBackoffSeconds: 10
profiles:
  - pluginConfig:
      - args:
          apiVersion: kubescheduler.config.k8s.io/v1beta1
          kind: DefaultPreemptionArgs
          minCandidateNodesAbsolute: 100
          minCandidateNodesPercentage: 10
        name: DefaultPreemption
      - args:
          apiVersion: kubescheduler.config.k8s.io/v1beta1
          hardPodAffinityWeight: 1
          kind: InterPodAffinityArgs
        name: InterPodAffinity
      - args:
          apiVersion: kubescheduler.config.k8s.io/v1beta1
          kind: NodeAffinityArgs
        name: NodeAffinity
      - args:
          apiVersion: kubescheduler.config.k8s.io/v1beta1
          kind: NodeResourcesFitArgs
        name: NodeResourcesFit
      - args:
          apiVersion: kubescheduler.config.k8s.io/v1beta1
          kind: NodeResourcesLeastAllocatedArgs
          resources:
            - name: cpu
              weight: 1
            - name: memory
              weight: 1
        name: NodeResourcesLeastAllocated
      - args:
          apiVersion: kubescheduler.config.k8s.io/v1beta1
          defaultingType: System
          kind: PodTopologySpreadArgs
        name: PodTopologySpread
      - args:
          apiVersion: kubescheduler.config.k8s.io/v1beta1
          bindTimeoutSeconds: 600
          kind: VolumeBindingArgs
        name: VolumeBinding
    plugins:
      filter:
        enabled:
          - name: Scheduling-framework-example
            weight: 0
      score:
        enabled:
          - name: Scheduling-framework-example
            weight: 2
    schedulerName: Scheduling-framework-example
```
