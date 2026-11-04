# Task 05 — Identity and Tenant Management

Implement authentication, user sessions, tenant lifecycle, memberships, roles, permissions, tenant switching, session revocation, audit and recovery flows.

Every request must authenticate actor, resolve membership, authorize action and establish tenant DB context. Include brute-force/rate-limit controls, secure cookie/session policy, CSRF where applicable, password hashing if passwords are used, and MFA-ready design.

Do not implement frontend-only authorization. Provide comprehensive tenant-crossing and privilege-escalation tests.
