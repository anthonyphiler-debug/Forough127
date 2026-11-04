# 13 — Extension System

## 1. Goal

Build a custom extension architecture inspired by the extensibility model of WordPress, tailored to Forough 127. Extensions may add capabilities only through documented extension points.

## 2. Supported extension points

- tenant/developer navigation entries and pages;
- backend API routes under a namespaced route group;
- event subscribers/hooks;
- background job handlers;
- settings schemas/screens;
- optional data tables and versioned migrations;
- UI widgets/components through approved rendering contracts.

## 3. Manifest

Every extension must declare:
- unique extension ID and publisher;
- name, description and version;
- supported platform/API versions;
- required permissions/capabilities;
- tenant/platform scope;
- plan/entitlement availability;
- dependencies;
- migrations;
- entry points;
- integrity/signature metadata;
- support and update metadata.

## 4. Security model

- default deny;
- capability grants are explicit and revocable;
- no direct unrestricted DB access;
- no access to raw secrets unless a narrow capability explicitly allows a mediated operation;
- tenant context is mandatory;
- extension routes are namespaced and authorized;
- event payloads are versioned and minimized;
- quotas and timeouts apply;
- installation/update/disable/remove operations are audited.

## 5. Runtime decision gate

Do not execute third-party arbitrary code in the main process until sandbox, isolation, signing, resource limits, network policy and rollback are approved. Initial milestone may implement registry, manifest validation, permission model and extension points without enabling untrusted runtime execution.

## 6. Lifecycle

- upload/registry;
- signature and compatibility verification;
- permission review;
- dependency resolution;
- migration plan;
- install;
- enable/disable;
- update with backup and rollback;
- uninstall with explicit data-retention choice.
