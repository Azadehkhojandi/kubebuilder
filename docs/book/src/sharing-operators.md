# OperatorHub.io 
OperatorHub.io is a new home for the Kubernetes community to share Operators. You can add your operator to [OperatorHub.io](https://operatorhub.io/).

## Steps

1. Create an empty folder same name as your Operator 

`CronJob-operator`

2. Copy all crds `from config/crd/bases` to `CronJob-operator` and rename each file. Remove group and domin from name from the begining of the file name and add change extension to `crd.yaml`

for example:

`batch.tutorial.kubebuilder.io_cronjobs.yaml`=> `cronjobs.crd.yaml`

3. Check each crd  has a `version` under spec. If it doesn't have it add it.

```
apiVersion: apiextensions.k8s.io/v1beta1
kind: CustomResourceDefinition
metadata:
  creationTimestamp: null
  name: cronjobs.batch.tutorial.kubebuilder.io
spec:
  group: batch.tutorial.kubebuilder.io
  names:
    kind: CronJob
    plural: cronjobs
  scope: ""
  subresources:
    status: {}
  version: v1
  versions:
  - name: v1
    served: true
    storage: true
status:
  acceptedNames:
    kind: ""
    plural: ""
  conditions: []
  storedVersions: []
```

4. Create CronJob-operator.package.yaml file. 

```
packageName: CronJob-operator
channels:
- name: alpha
  currentCSV: CronJob-operator.v1.0.0
defaultChannel: alpha
```

5. Create CronJob-operator.v1.0.0.clusterserviceversion.yaml file. 


6. Update CronJob-operator.v1.0.0.clusterserviceversion.yaml 


```
apiVersion: operators.coreos.com/v1alpha1
kind: ClusterServiceVersion
metadata:
  name: CronJob-operator.v1.0.0
  namespace: placeholder
  annotations:
    categories: "xxxxx"
    capabilities: Basic Install
    repository: https://github.com/xxxxx
    description: xxxx
    containerImage: xxxx/xxx:xx
    createdAt: 2019-09-16T12:00:00Z
    support: xxx
    alm-examples: |-
      [{
        "apiVersion": "batch.tutorial.kubebuilder.io/v1",
        "kind": "CronJob",
        "metadata": {
            "name": "cronjob-sample"
        },
        "spec": {
            "schedule": "*/1 * * * *",
            "startingDeadlineSeconds": 60,
            "concurrencyPolicy": "Allow",
            "jobTemplate": {
            "spec": {
                "template": {
                "spec": {
                    "containers": [
                    {
                        "name": "hello",
                        "image": "busybox",
                        "args": [
                        "/bin/sh",
                        "-c",
                        "date; echo Hello from the Kubernetes cluster"
                        ]
                    }
                    ],
                    "restartPolicy": "OnFailure"
                    }}}}
            }
        }]
    
spec:
  displayName: xxx
  description: >
    
   Cron Jobs Operator

  keywords: ['xxx']
  version: 1.0.0
  maturity: alpha
  maintainers:
  - name: xxxx
    email: xxx@xxxx.com
  provider:
    name: xxxx
  labels:
    name: CronJob-operator
  selector:
    matchLabels:
      name: CronJob-operator
  links:
  - name: CronJob-operator Source Code
    url: https://github.com/xxxxxx
  icon:
  - base64data: xxxx
    mediatype: image/png
  installModes:
  - type: OwnNamespace
    supported: true
  - type: SingleNamespace
    supported: false
  - type: MultiNamespace
    supported: false
  - type: AllNamespaces
    supported: true
  install:
    strategy: deployment
    spec:
      clusterPermissions:
      - serviceAccountName: CronJob-operator
        rules:
        - apiGroups:
            - ""
            resources:
            - pods
            - services
            - endpoints
            - persistentvolumeclaims
            - events
            - configmaps
            - secrets
            - serviceaccounts
            verbs:
            - "*"
        - apiGroups:
            - batch
            resources:
            - jobs
            verbs:
            - create
            - delete
            - get
            - list
            - patch
            - update
            - watch
            - apiGroups:
            - batch
            resources:
            - jobs/status
            verbs:
            - get
            - apiGroups:
            - batch.tutorial.kubebuilder.io
            resources:
            - cronjobs
            verbs:
            - create
            - delete
            - get
            - list
            - patch
            - update
            - watch
            - apiGroups:
            - batch.tutorial.kubebuilder.io
            resources:
            - cronjobs/status
            verbs:
            - get
            - patch
            - update
      deployments:
      - name: controller-manager
        spec:
            selector:
                matchLabels:
                control-plane: controller-manager
            replicas: 1
            template:
                metadata:
                labels:
                    control-plane: controller-manager
                spec:
                containers:
                - command:
                    - /manager
                    args:
                    - --enable-leader-election
                    image: controller:latest
                    name: manager
                    resources:
                    limits:
                        cpu: 100m
                        memory: 30Mi
                    requests:
                        cpu: 100m
                        memory: 20Mi
      terminationGracePeriodSeconds: 10
  customresourcedefinitions:
    owned:
    - name: cronjobs.batch.tutorial.kubebuilder.io
      version: v1
      kind: CronJob
      displayName: CronJob
      description: CronJob
```

7. Your package folder structure 

```
CronJob-operator/
├── cronjobs.crd.yaml
└── CronJob-operator.package.yaml
└── CronJob-operator.v1.0.0.clusterserviceversion.yaml
```

8. Follow Steps here to [Test your package locally](https://github.com/operator-framework/community-operators/blob/master/docs/testing-operators.md#operator-courier)

9. Fork and Clone (operator-framework)[https://github.com/operator-framework]. Copy your folder under [](https://github.com/operator-framework/community-operators/tree/master/upstream-community-operators) and send pr
