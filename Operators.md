# Operator overview
---

Kubernetes/Openshift Operators are software extensions
They automate the management of complex stateful applications

They are meant to encapsulate the operational knowledge required to run these applications


## Fundamental components
Operators rely on 3 primary components:

## Custom Resource Definitions (CRDs)
CRDs extend the Kubernetes API by introducing new resource types specific to the app. 

They define the schema and structure for the custom resource, which can be managed with kubectl

## Custom Controllers
A piece of software that monitors and manages the state of custom resources

It implements the control loop, that continuously reconciles the desired state with the state of the system

They can perform a number of tasks such as:
- creating
- updating
- deleting
- managing upgrades
- health checks
- etc

## Operator Lifecycle Manager (OLM) -> Optional
A kubernetes extension that helps manage the lifecycle of Operators.

That may include:
- Installation
- Upgrade
- Uninstallation

The fundamental components are outlined in 3 specific locations in the kubebuilder file structure

```
.
├── api        <-- This directory contains the API definition for your custom resources
│   └── v1alpha1
│       ├── groupversion_info.go
│       ├── testresource_types.go   <-- 
│       └── zz_generated.deepcopy.go
├── bin
│   └── controller-gen
├── config
│   ├── crd    <-- This directory contains the CRD manifests generated from API definition rules
│   │   ├── bases
│   │   │   └── testing.test.com_testresources.yaml  <-- Generated CRD Manifest
│   │   ├── kustomization.yaml
│   │   ├── kustomizeconfig.yaml
│   │   └── patches
│   │       ├── cainjection_in_testresources.yaml
│   │       └── webhook_in_testresources.yaml
│   ├── default
│   │   ├── kustomization.yaml
│   │   ├── manager_auth_proxy_patch.yaml
│   │   └── manager_config_patch.yaml
│   ├── manager
│   │   ├── kustomization.yaml
│   │   └── manager.yaml
│   ├── prometheus
│   │   ├── kustomization.yaml
│   │   └── monitor.yaml
│   ├── rbac
│   │   ├── auth_proxy_client_clusterrole.yaml
│   │   ├── auth_proxy_role_binding.yaml
│   │   ├── auth_proxy_role.yaml
│   │   ├── auth_proxy_service.yaml
│   │   ├── kustomization.yaml
│   │   ├── leader_election_role_binding.yaml
│   │   ├── leader_election_role.yaml
│   │   ├── role_binding.yaml
│   │   ├── role.yaml
│   │   ├── service_account.yaml
│   │   ├── testresource_editor_role.yaml
│   │   └── testresource_viewer_role.yaml
│   └── samples
│       └── testing_v1alpha1_testresource.yaml
├── controllers    <-- This directory contains the controller implementation
│   ├── suite_test.go
│   └── testresource_controller.go  <-- Implements the custom controller + reconcile logic
├── Dockerfile
├── go.mod
├── go.sum
├── hack
│   └── boilerplate.go.txt
├── main.go
├── Makefile
├── PROJECT
└── README.md
```

## Libraries

Go will be using 3rd party in order to operate

### sigs.k8s.io/controller-runtime 
This is the primary library used with kubebuilder.

It provides a higher-level API than client-go, contains tools and abstractions for building custom controllers

#### sigs.k8s.io/controller-runtime/pkg/client
Provides high level client for interacting with the kubernetes API

Including:
- creating resources
- updating resources
- deleting resources

#### sigs.k8s.io/controller-runtime/pkg/controller
Offers tools for building and managing controllers


#### sigs.k8s.io/pkg/reconcile
Contains the reconciler interface, in which the reconcile logic is implemented

#### sigs.k8s.io/controller-runtime/pkg/log
Contains logging utils built on top of the logr interface

### k8s.io/apimachinery
Contains various packages packages for working with the Kubernetes API 

This includes:
- types
- serializers
- utility functions

#### k8s.io/apimachinery/pkg/apis/meta/v1
Contains the API metadata types 

- ObjectMeta
- TypeMeta
- Status

#### k8s.io/apimachinery/pkg/runtime
Provides the runtime infra for working with the Kubernetes API

### k8s.io/api
This library contains the typed Go structs for Kubernetes objects

- Deployments
- Services
- ConfigMaps
- etc

### k8s.io/client-go
Not used too often in favor of the controller runtime client.

Still encountered in some cases.

### go-logr/logr
The logging interface for Go 



