## Creating Service account

apiVersion: v1
kind: ServiceAccount
metadata:
  name: jenkins
  namespace: webapps

## Create Role

apiVersion: rbac.authorization.k8s.io/v1
kind: Role
metadata:
  namespace: app-role  # Namespace where the Role is created
  name: webapps
rules:
  - apiGroups:
      - ""
      - apps
      - autoscaling
      - batch
      - extensions
      - policy
      - rbac.authorization.k8s.io  
    resources:
      - pods
      - secrets
      - componentstatuses
      - configmaps
      - daemonsets
      - deployments
      - events
      - endpoints
      - horizontalpodautoscalers
      - ingress
      - jobs
      - limitranges
      - namespaces
      - nodes
      - persistentvolumes
      - persistentvolumeclaims
      - resourcequotas
      - replicasets
      - replicationcontrollers
      - serviceaccounts
      - services
    verbs: ["get", "watch", "list", "create", "update", "patch", "delete"]


## Bind the role to service account

apiVersion: rbac.authorization.k8s.io/v1
kind: RoleBinding
metadata:
  namespace: app-rolebinding  # This should be the namespace where the RoleBinding is applied
  name: webapps
roleRef:
  apiGroup: rbac.authorization.k8s.io
  kind: Role
  name: app-role
subjects:
- kind: ServiceAccount
  name: jenkins
  namespace: app-rolebinding  # The namespace where the ServiceAccount "jenkins" exists

## Create additional API tokens
https://kubernetes.io/docs/reference/access-authn-authz/service-accounts-admin/#create-token

apiVersion: v1
kind: Secret
type: kubernetes.io/service-account-token
metadata:
  name: mysecretname
  annotations:
    kubernetes.io/service-account.name: myserviceaccount



