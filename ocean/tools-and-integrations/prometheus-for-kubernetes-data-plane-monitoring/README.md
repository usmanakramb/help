# Prometheus for Kubernetes Data-Plane Monitoring

Ocean functions as an autopilot for the Kubernetes data plane, as it delivers premium [container-driven autoscaling](https://spot.io/blog/ocean-explained-container-driven-autoscaling-with-kubernetes/) to continuously optimize your cloud infrastructure for the cluster. Ocean manages the scaling of the data plane and the data regarding it could be valuable to monitor your containerized environment. Using well-defined Prometheus metrics for monitoring Ocean can accelerate your understanding of the cluster scaling and debugging of issues that may come up. In addition, you could build alerts based on the metrics to address issues in real time and track important trends on a dashboard of different Ocean metrics.

Ocean maintains an official set of metrics, natively scrapable by Prometheus. This set of metrics helps to build a 360 degree view of the actions Ocean takes, while providing an application-driven infrastructure.

## Ocean Metrics

By monitoring a few key metrics you can gain an understanding of how Ocean scales. The metrics are described below with examples.

### ocean_managed_nodes

This metric tracks the total number of nodes managed by Ocean in the cluster. The metric could be useful in responding to unnatural anomalies. In addition, the metric provides an easy breakdown to monitor specific virtual node groups in the cluster as well as other infrastructure characteristics such as instance lifecycle and type.

Prometheus metric type: gauge

Dimensions:
`vng_id`, `lifecycle="Spot/ OD/ OD(RI)"`, `az`, `vm_type`, `ocean_name`, `vng_name`, `ocean_id`

Example:

```
ocean_managed_nodes {vng_id="default", lifecycle="Spot", az="us-west-2a", vm_type="c5.2xlarge", ocean_name=”Prod”, vng_name = “default”, ocean_id= “o-XXXXX”} 13
ocean_managed_nodes {vng_id="ols-xxxxxxxx", lifecycle="OD", az="us-west-2b", type="c5.2xlarge", ocean_name=”Prod” , vng_name = “Vng1”,ocean_id= “o-XXXXX”} 2
ocean_managed_nodes {vng_id="ols-xxxxxxxx", lifecycle="Spot", az="us-west-2b", vm_type="m5.8xlarge",  vng_name = “Vng2”, ocean_id= “o-XXXXX”} 3
```

### Ocean_Nodes_added_total and Ocean_Nodes_removed_total

These counter type metrics help track the rate of addition and removal of nodes in the cluster and the different reasons behind them, as reflected in a dedicated label called Reason.  Reason includes values such as `pendingPodsOrHeadroom` (as part of scale up), `scaleDownForOptimization`, `odToSpotReplacement`, `autoHealingReplacement`, `riUtilizationReplacement`, `recoveryReplacement`, `revertToLowerCostReplacement`, `shutdownHoursActivity`, `clusterRollReplacement`, `workloadMigration`.

Prometheus metric type: counter

Dimensions: `reason` (, `VNG_Id, lifecycle="Spot/ ON-DEMAND/ OD(RI)"`, `az`, `vm_type`, `Ocean_Name`, `vng_name`, `ocean_id`

Example:
```
nodes_added_toal{reason="revertToLowerCostReplacement", lifecycle="Spot", az="us-west-2a", vm_type="c5.xlarge", vng_name = “Vng2”, ocean_id= “o-XXXXX”} 46
nodes_added_total{reason="scaleUpForPendingPods", lifecycle="Spot", az="us-west-2b", vm_type="r5a.4xlarge", vng_name = “Vng2”, ocean_id= “o-XXXXX”} 13
```

## Tracking Ocean Managed Resources

It is also useful to track Ocean managed resources in order to get a full picture.

### ocean_allocatable_cpu_vcpus, ocean_allocatable_memory_gibs, ocean_allocatable_gpu_units

These metrics track the total resources allocatable by all nodes in the cluster.

Prometheus metric type: gauge

Dimensions: `VNG_Id`, `lifecycle="Spot/ OD/ OD(RI)"`, `az`, `vm_type`, `Ocean_Name`, `vng_name`, `ocean_id`

Example:
```
allocatable_cpu{vng_id="ols-xxxxxxxx", lifecycle="OD/Spot/RI/SP", az="us-west-2a", vm_type="c5.2xlarge", vng_name = “Vng2”, ocean_id= “o-XXXXX”, “ocean_name”: “Prod”} 45
```

### ocean_allocatable_headroom_vcpus, ocean_allocatable_headroom_memory_gibs, ocean_allocatable_headroom_gpu_units

This metric tracks the total resource allocatable as headroom in all nodes in the cluster.

Prometheus metric type: gauge

Dimensions: `VNG_Id`, `lifecycle="Spot/ OD/ OD(RI)"`, `az`, `vm_type`, `Ocean_Name`, `vng_name`, `ocean_id`

Example:
```
allocatable_headroom_gpu{vng_id="ols-xxxxxxxx", lifecycle="OD", az="us-west-2a", vm_type="c5.2xlarge",  vng_name = “Vng2”, ocean_id= “o-XXXXX”, “ocean_name”: “prod”} 12
```

## Tracking Healthiness

The metrics described below provide a good indication of Ocean health.           

### ocean_controller_heartbeat_info

This metric is a measure of the controller health in the cluster. The metric tracks Ocean connectivity health.

Prometheus metric type: gauge (1/0)

Dimensions:  `Ocean_Name`, `ocean_id`

Example:
```
ocean_controller_heartbeat_info{ “ocean_name”: “prod”, “ocean_id”: o-XXXXXX} 1
```

## What’s Next?

Learn how to use [Prometheus to scrape Ocean metrics](ocean/tools-and-integrations/prometheus-for-kubernetes-data-plane-monitoring/scrape-ocean-metrics-using-prometheus).