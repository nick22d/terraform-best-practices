blue-green
canary
A/B testing
rolling


Promote through environments: Validate automation in non-production before promoting to production, catching issues early and reducing production incidents. Start with a development environment, progress to staging, then deploy to production with confidence.

Implement progressive deployments: Deploy changes gradually with zero-downtime strategies like blue-green or canary deployments, minimizing risk and enabling quick rollbacks when issues occur.
https://developer.hashicorp.com/well-architected-framework/define-and-automate-processes/automate/deployments

Atomic deployments deploy infrastructure as small, isolated units of change rather than deploying all infrastructure components together. Large, monolithic deployments that modify multiple systems simultaneously create risk, make troubleshooting difficult, and slow down rollback operations when issues occur. Deploy infrastructure atomically to reduce blast radius, enable independent team workflows, and accelerate your deployment velocity.
https://developer.hashicorp.com/well-architected-framework/define-and-automate-processes/deploy/atomic-deployments