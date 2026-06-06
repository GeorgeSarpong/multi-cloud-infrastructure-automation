# Infrastructure Deployment Procedure

**Document Type:** Deployment Runbook
**Author:** George Amankwaa Sarpong
**Last Updated:** June 2026

---

## Purpose
Standardised procedure for deploying cloud infrastructure changes using Terraform and CI/CD pipelines ensuring consistent, safe, and well-documented deployments.

---

## Pre-Deployment Requirements
- Terraform plan reviewed and approved
- Change request approved in change management system
- Deployment window communicated to stakeholders
- Rollback procedure confirmed and tested
- Monitoring dashboards accessible

---

## Deployment Procedure

### Phase 1 — Preparation
1. Review and merge approved pull request
2. Confirm CI/CD pipeline triggered
3. Review Terraform plan output carefully
4. Verify no unexpected resource deletions
5. Confirm approval gate before apply

### Phase 2 — Execution
1. Approve Terraform apply in CI/CD pipeline
2. Monitor pipeline execution in real time
3. Watch for any errors or warnings
4. Do not interrupt pipeline once started
5. Document start time in change ticket

### Phase 3 — Validation
1. Verify all resources created in AWS/Azure console
2. Run smoke tests against new infrastructure
3. Check monitoring dashboards for anomalies
4. Verify application functionality end to end
5. Confirm security controls in place

### Phase 4 — Closure
1. Update change ticket with completion time
2. Notify stakeholders of successful deployment
3. Monitor for 30 minutes post-deployment
4. Document any issues encountered
5. Close change request

---

## Rollback Procedure
1. Identify the last known good Terraform state
2. Run terraform plan with previous version
3. Review plan for any data loss risks
4. Execute rollback with approval
5. Validate rollback successful
6. Document incident and root cause
