# DevSecOps: Complete Guide & CI/CD Pipeline Security

## Table of Contents
1. [What is DevSecOps?](#what-is-devsecops)
2. [Why Do We Need DevSecOps?](#why-do-we-need-devsecops)
3. [DevSecOps Tools to Learn](#devsecops-tools-to-learn)
4. [CI/CD Pipeline Security Recommendations](#cicd-pipeline-security-recommendations)
5. [Sample CI/CD Pipeline Configuration](#sample-cicd-pipeline-configuration)
6. [AWS / EKS Notes](#aws--eks-notes)
7. [Best Practices](#best-practices)
8. [Learning Roadmap](#learning-roadmap)
9. [Resources](#resources)
10. [Conclusion](#conclusion)

---

## What is DevSecOps?

**DevSecOps** (Development, Security, and Operations) is an approach to culture, automation, and platform design that integrates security as a shared responsibility throughout the entire IT lifecycle.

### Key Principles

| Principle | Description |
|-----------|-------------|
| **Shift Left Security** | Move security testing earlier in the development lifecycle |
| **Security as Code** | Define security policies and controls as version-controlled code |
| **Automated Security Testing** | Integrate security scans into CI/CD pipelines |
| **Continuous Compliance** | Monitor and enforce compliance continuously, not just at audit time |
| **Shared Responsibility** | Security is everyone's job, not just the security team |
| **Shift Right (Runtime)** | Shifting left is not enough; keep monitoring and detecting threats in production |

### DevSecOps vs Traditional Security

```
Traditional Approach:          DevSecOps Approach:
┌─────────────┐                ┌─────────────────────────────┐
│  Develop    │                │  Develop → Secure → Test    │
│     ↓       │                │     ↓                       │
│  Test       │                │  Secure → Deploy → Monitor  │
│     ↓       │                │  (Security at every stage)  │
│  Deploy     │                │                             │
│     ↓       │                │  Security is built-in,      │
│  Security   │                │  not bolted-on              │
│  (Audit)    │                │                             │
└─────────────┘                └─────────────────────────────┘
```

---

## Why Do We Need DevSecOps?

### 1. **Speed vs Security Balance**
Modern development teams deploy code multiple times per day. Traditional security reviews (weekly/monthly) create bottlenecks. DevSecOps enables **fast AND secure** deployments.

### 2. **Rising Security Threats**
- Supply chain attacks (SolarWinds, Log4j, XZ Utils)
- Compromised CI/CD actions and build tooling
- Cloud misconfigurations causing data breaches
- Exposed credentials in code repositories
- Container vulnerabilities

### 3. **Cost of Fixing Vulnerabilities**

Fixing issues later costs significantly more. The multipliers below are commonly quoted **illustrative figures**, not exact measurements; treat them as a rule of thumb.

| Stage | Relative Cost (illustrative) |
|-------|------------------------------|
| Development | 1x |
| Testing | ~5x |
| Production | ~30x+ |
| Post-Breach | ~100x+ |

### 4. **Compliance Requirements**
- GDPR, HIPAA, PCI-DSS, SOC 2 require continuous security
- Manual compliance checks don't scale
- DevSecOps automates evidence collection

### 5. **Developer Productivity**
- Security feedback in IDE/PR = faster fixes
- No context switching to security tools
- Clear remediation guidance

---

## DevSecOps Tools to Learn

> Tool maintenance status changes over time. Check each project's repository before adopting it.

### 1. **Source Code & Secret Management**

| Tool | Purpose | Type |
|------|---------|------|
| **Git** | Version control | Essential |
| **GitHub/GitLab/Bitbucket** | Code hosting & collaboration | Essential |
| **TruffleHog** | Secret detection in code & git history | Open Source |
| **Gitleaks** | Fast secret scanning | Open Source |
| **Git-secrets** | Prevent committing secrets | Open Source |
| **HashiCorp Vault / AWS Secrets Manager** | Store and rotate secrets (detection alone is not enough) | Open Source / Commercial |

### 2. **Static Application Security Testing (SAST)**

| Tool | Purpose | Type |
|------|---------|------|
| **Semgrep** | Fast, customizable SAST | Open Source |
| **SonarQube** | Code quality + security | Community / Commercial |
| **Checkmarx** | Enterprise SAST | Commercial |
| **Snyk Code** | Developer-first SAST | Freemium |
| **Bandit** | Python-specific SAST | Open Source |

### 3. **Dynamic Application Security Testing (DAST)**

| Tool | Purpose | Type |
|------|---------|------|
| **OWASP ZAP** | Web app security scanner | Open Source |
| **Nuclei** | Vulnerability scanner | Open Source |
| **Burp Suite** | Web app security testing | Freemium |
| **Nikto** | Web server scanner | Open Source |

### 4. **Software Composition Analysis (SCA)**

| Tool | Purpose | Type |
|------|---------|------|
| **Snyk** | Dependency vulnerability scanning | Freemium |
| **OWASP Dependency-Check** | Dependency CVE detection | Open Source |
| **Trivy** | Container + dependency scanning | Open Source |
| **Grype** | Vulnerability scanner (also scans SBOMs from Syft) | Open Source |
| **Dependabot / Renovate** | Automated dependency update PRs | Free / Open Source |

### 5. **Infrastructure as Code (IaC) Security**

| Tool | Purpose | Type |
|------|---------|------|
| **Checkov** | IaC security scanning | Open Source |
| **Trivy (config scan)** | IaC scanning; the successor to tfsec | Open Source |
| **tfsec** | Terraform scanner (now merged into Trivy; prefer Trivy for new setups) | Open Source |
| **Terrascan** | IaC misconfiguration detection (verify it is still maintained) | Open Source |
| **KICS** | Infrastructure as Code scanning | Open Source |

### 6. **Container & Kubernetes Security**

| Tool | Purpose | Type |
|------|---------|------|
| **Trivy** | Container image scanning | Open Source |
| **Clair** | Container vulnerability analysis | Open Source |
| **Kube-bench** | CIS Kubernetes benchmark | Open Source |
| **Kube-hunter** | Kubernetes penetration testing (verify it is still maintained) | Open Source |
| **Falco** | Runtime security monitoring | Open Source |

### 7. **Policy as Code**

| Tool | Purpose | Type |
|------|---------|------|
| **Open Policy Agent (OPA)** | Policy engine | Open Source |
| **Kyverno** | Kubernetes-native policy engine | Open Source |
| **Sentinel** | HashiCorp policy framework | Commercial |
| **Conftest** | Configuration testing | Open Source |

### 8. **Supply Chain Security**

| Tool | Purpose | Type |
|------|---------|------|
| **Cosign** | Container signing | Open Source |
| **Syft** | SBOM generation | Open Source |
| **Sigstore** | Software signing & transparency | Open Source |
| **in-toto / SLSA** | Supply chain integrity and build provenance | Open Source |
| **OpenSSF Scorecard** | Security health checks for repositories | Open Source |

### 9. **CI/CD Platforms**

| Tool | Purpose | Type |
|------|---------|------|
| **Jenkins** | Automation server | Open Source |
| **GitHub Actions** | CI/CD workflows | Freemium |
| **GitLab CI** | Integrated CI/CD | Freemium |
| **Azure DevOps** | Microsoft CI/CD | Commercial |
| **ArgoCD** | GitOps continuous delivery | Open Source |

### 10. **Cloud Security**

| Tool | Purpose | Type |
|------|---------|------|
| **Prowler** | AWS security assessment | Open Source |
| **ScoutSuite** | Multi-cloud security auditing | Open Source |
| **Cloud Custodian** | Cloud resource management | Open Source |
| **AWS Security Hub / GuardDuty / Inspector** | Native AWS posture, threat and vulnerability services | Commercial |

---

## CI/CD Pipeline Security Recommendations

### Pipeline Security Stages

```
┌──────────────────────────────────────────────────────────────────────────┐
│                         SECURE CI/CD PIPELINE                            │
├──────────────────────────────────────────────────────────────────────────┤
│                                                                          │
│  ┌────────────┐  ┌────────────┐  ┌──────────┐  ┌──────────┐  ┌────────┐  │
│  │ Pre-commit │→ │ PR/Commit  │→ │  Build   │→ │   Test   │→ │ Deploy │  │
│  │            │  │            │  │          │  │          │  │        │  │
│  │ • Secrets  │  │ • Secrets  │  │ • Image  │  │ • DAST   │  │ • Sign │  │
│  │ • Lint     │  │ • SAST     │  │   scan   │  │ • K8s    │  │ • Verify│ │
│  │ • SAST     │  │ • SCA/IaC  │  │ • SBOM   │  │   bench  │  │ • Admit│  │
│  │            │  │            │  │ • Sign   │  │          │  │        │  │
│  └────────────┘  └────────────┘  └──────────┘  └──────────┘  └────────┘  │
│         │               │              │             │            │       │
│         ▼               ▼              ▼             ▼            ▼       │
│  ┌────────────────────────────────────────────────────────────────────┐  │
│  │                      CONTINUOUS MONITORING                         │  │
│  │      • Runtime security  • Compliance  • Threat detection          │  │
│  └────────────────────────────────────────────────────────────────────┘  │
│                                                                          │
└──────────────────────────────────────────────────────────────────────────┘
```

### Stage-by-Stage Tool Recommendations

#### Stage 1: Pre-Commit (Developer Workstation)

| Tool | Purpose | Configuration |
|------|---------|---------------|
| **pre-commit** | Framework for git hooks | Install hooks for all developers |
| **Gitleaks** | Secret detection | Run on every commit |
| **Semgrep** | Fast SAST | IDE integration + pre-commit |

**Why:** Catch issues before code leaves the developer's machine. Hooks can be bypassed locally, so always enforce the same checks again in CI.

#### Stage 2: Commit / Pull Request

| Tool | Purpose | Configuration |
|------|---------|---------------|
| **TruffleHog** | Deep secret scan | Scan full git history |
| **Semgrep** | SAST with custom rules | Block on critical findings |
| **Trivy / Checkov** | SCA and IaC scanning | Scan Terraform/K8s/dependency changes |
| **Dependency Review** | Dependency changes | GitHub-native action |

**Why:** Prevent vulnerable code from merging to the main branch. Pair with branch protection and required reviews.

#### Stage 3: Build

| Tool | Purpose | Configuration |
|------|---------|---------------|
| **Trivy** | Container image scan | Scan the image before pushing |
| **Syft** | SBOM generation | Generate with every build |
| **Cosign** | Image signing | Sign by digest after push |
| **SonarQube** | Code quality gate | Enforce quality standards |

**Why:** Ensure artifacts are secure and traceable.

#### Stage 4: Test

| Tool | Purpose | Configuration |
|------|---------|---------------|
| **OWASP ZAP** | DAST scanning | Scan staging environment |
| **Nuclei** | Vulnerability scanning | Template-based scanning |
| **Kube-bench** | K8s security check | Run against test clusters |

**Why:** Find runtime vulnerabilities before production.

#### Stage 5: Deploy

| Tool | Purpose | Configuration |
|------|---------|---------------|
| **OPA / Kyverno** | Policy enforcement | Kubernetes admission control |
| **Cosign** | Signature verification | Verify before deployment (or enforce via admission policy) |
| **Falco** | Runtime security | Deploy as DaemonSet |
| **ArgoCD** | GitOps deployment | Enforce Git as source of truth |

**Why:** Ensure only verified, compliant workloads deploy.

#### Stage 6: Monitor

| Tool | Purpose | Configuration |
|------|---------|---------------|
| **Falco** | Runtime threat detection | Real-time alerting |
| **Prometheus + Grafana** | Metrics & dashboards | Security metrics |
| **ELK Stack** | Log aggregation | Centralized security logs |
| **Prowler** | Cloud security monitoring | Scheduled scans |

**Why:** Detect and respond to threats in production.

---

## Sample CI/CD Pipeline Configuration

### GitHub Actions Example

> **Before using this:**
> - **Pin every third-party action to a full commit SHA** (or at least an exact release tag). Never use `@main` or `@master`. Replace the `<pin-to-sha>` placeholder below.
> - Replace `kubectl` / `argocd` auth steps with your own cluster login (see [AWS / EKS Notes](#aws--eks-notes)).
> - Replace `myapp` URLs and paths with your own.

```yaml
# .github/workflows/devsecops-pipeline.yml

name: DevSecOps Pipeline

on:
  push:
    branches: [main]
  pull_request:
    branches: [main]

# Least privilege by default; jobs request more only when needed
permissions:
  contents: read

env:
  REGISTRY: ghcr.io
  IMAGE_NAME: ${{ github.repository }}

jobs:
  # Stage 1: Secret Detection
  secrets-scan:
    name: Secret Detection
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
        with:
          fetch-depth: 0  # Full history for TruffleHog

      - name: Run TruffleHog
        uses: trufflesecurity/trufflehog@<pin-to-sha>
        with:
          extra_args: --results=verified,unknown   # also flag unverified findings

  # Stage 2: SAST
  sast:
    name: Static Analysis
    runs-on: ubuntu-latest
    container:
      image: semgrep/semgrep   # pin to a specific tag or digest
    steps:
      - uses: actions/checkout@v4

      - name: Run Semgrep
        run: >
          semgrep scan
          --config p/security-audit
          --config p/owasp-top-ten
          --error

  # Stage 3: SCA
  dependency-scan:
    name: Dependency Scanning
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4

      - name: Run Trivy (Filesystem)
        uses: aquasecurity/trivy-action@0.28.0   # prefer a full SHA
        with:
          scan-type: fs
          scan-ref: .
          severity: CRITICAL,HIGH
          exit-code: 1
          ignore-unfixed: true

  # Stage 4: IaC Scanning
  iac-scan:
    name: IaC Security
    runs-on: ubuntu-latest
    permissions:
      contents: read
      security-events: write   # needed to upload SARIF
    steps:
      - uses: actions/checkout@v4

      - name: Run Checkov
        uses: bridgecrewio/checkov-action@v12   # prefer a full SHA
        with:
          directory: .
          framework: terraform,kubernetes,helm
          output_format: sarif
          output_file_path: results.sarif
          soft_fail: false

      - name: Upload SARIF
        if: always()
        uses: github/codeql-action/upload-sarif@v3
        with:
          sarif_file: results.sarif

  # Stage 5: Build, Scan, Push & Sign
  build:
    name: Build, Scan & Sign Image
    runs-on: ubuntu-latest
    needs: [secrets-scan, sast, dependency-scan, iac-scan]
    permissions:
      contents: read
      packages: write
      id-token: write          # keyless Cosign signing
    outputs:
      digest: ${{ steps.push.outputs.digest }}
    steps:
      - uses: actions/checkout@v4

      - name: Set up Docker Buildx
        uses: docker/setup-buildx-action@v3

      - name: Build image (not pushed yet)
        uses: docker/build-push-action@v6
        with:
          context: .
          load: true
          push: false
          tags: ${{ env.REGISTRY }}/${{ env.IMAGE_NAME }}:${{ github.sha }}

      - name: Scan container image
        uses: aquasecurity/trivy-action@0.28.0   # prefer a full SHA
        with:
          image-ref: ${{ env.REGISTRY }}/${{ env.IMAGE_NAME }}:${{ github.sha }}
          severity: CRITICAL,HIGH
          exit-code: 1
          ignore-unfixed: true

      - name: Generate SBOM
        uses: anchore/sbom-action@v0
        with:
          image: ${{ env.REGISTRY }}/${{ env.IMAGE_NAME }}:${{ github.sha }}
          format: spdx-json
          output-file: sbom.spdx.json

      # Only push (and sign) from main, never from pull requests
      - name: Log in to registry
        if: github.ref == 'refs/heads/main'
        uses: docker/login-action@v3
        with:
          registry: ${{ env.REGISTRY }}
          username: ${{ github.actor }}
          password: ${{ secrets.GITHUB_TOKEN }}

      - name: Push image
        id: push
        if: github.ref == 'refs/heads/main'
        uses: docker/build-push-action@v6
        with:
          context: .
          push: true
          tags: ${{ env.REGISTRY }}/${{ env.IMAGE_NAME }}:${{ github.sha }}

      - name: Install Cosign
        if: github.ref == 'refs/heads/main'
        uses: sigstore/cosign-installer@v3

      - name: Sign image by digest (keyless)
        if: github.ref == 'refs/heads/main'
        run: |
          cosign sign --yes \
            ${{ env.REGISTRY }}/${{ env.IMAGE_NAME }}@${{ steps.push.outputs.digest }}

  # Stage 6: Deploy to Staging (main branch only)
  deploy-staging:
    name: Deploy to Staging
    runs-on: ubuntu-latest
    if: github.ref == 'refs/heads/main'
    needs: [build]
    environment: staging
    steps:
      - uses: actions/checkout@v4

      - name: Install Cosign
        uses: sigstore/cosign-installer@v3

      - name: Verify image signature
        run: |
          cosign verify \
            --certificate-identity-regexp "^https://github.com/${{ github.repository }}/" \
            --certificate-oidc-issuer https://token.actions.githubusercontent.com \
            ${{ env.REGISTRY }}/${{ env.IMAGE_NAME }}@${{ needs.build.outputs.digest }}

      # TODO: authenticate to your cluster here (e.g. AWS OIDC + aws eks update-kubeconfig)
      - name: Deploy to Kubernetes
        run: |
          kubectl apply -f k8s/staging/

      - name: Run DAST (OWASP ZAP)
        uses: zaproxy/action-full-scan@v0.12.0
        with:
          target: 'https://staging.myapp.com'

  # Stage 7: Production Deploy (manual approval via environment protection rules)
  deploy-production:
    name: Deploy to Production
    runs-on: ubuntu-latest
    if: github.ref == 'refs/heads/main'
    needs: [build, deploy-staging]
    environment: production
    steps:
      - name: Install Cosign
        uses: sigstore/cosign-installer@v3

      - name: Verify image signature
        run: |
          cosign verify \
            --certificate-identity-regexp "^https://github.com/${{ github.repository }}/" \
            --certificate-oidc-issuer https://token.actions.githubusercontent.com \
            ${{ env.REGISTRY }}/${{ env.IMAGE_NAME }}@${{ needs.build.outputs.digest }}

      # TODO: log in to ArgoCD first (e.g. argocd login ... using a short-lived token)
      - name: Deploy with ArgoCD
        run: |
          argocd app sync myapp-production
```

**Notes on this pipeline**
- Deploy jobs only run on `main`, never on pull requests.
- The image is pushed to a registry, then signed and verified **by digest**, so the verified artifact is the one that gets deployed.
- Keyless signing needs `id-token: write`; verification must state the expected identity and OIDC issuer.
- Production approval is enforced through GitHub **environment protection rules** (required reviewers).

---

## AWS / EKS Notes

If your pipeline targets AWS and EKS:

- **Use GitHub OIDC instead of long-lived AWS keys.** Create an IAM role with a trust policy limited to your repo and branch, then:

```yaml
permissions:
  id-token: write
  contents: read

steps:
  - uses: aws-actions/configure-aws-credentials@v4
    with:
      role-to-assume: arn:aws:iam::<ACCOUNT_ID>:role/<github-actions-role>
      aws-region: ap-south-1

  - name: Update kubeconfig
    run: aws eks update-kubeconfig --name <cluster-name> --region ap-south-1
```

- **ECR:** enable image scanning on push (or Amazon Inspector) as a second layer next to Trivy.
- **EKS:** use IRSA / Pod Identity for pod-level AWS access, enforce Pod Security Standards, and apply NetworkPolicies.
- **Terraform:** scan with Checkov or Trivy in the PR stage, and keep state in an encrypted, access-controlled backend.
- **Account-level monitoring:** enable GuardDuty and Security Hub, and run Prowler on a schedule.

---

## Best Practices

### 1. **Start Small, Scale Gradually**
```
Week 1-2:   Secret scanning (TruffleHog / Gitleaks)
Week 3-4:   Add SAST (Semgrep)
Week 5-6:   Add SCA (Trivy)
Week 7-8:   Add IaC scanning (Checkov)
Week 9-10:  Add container scanning, SBOM and signing
Week 11-12: Add DAST & monitoring
```

### 2. **Define Security Gates**

| Severity | Action |
|----------|--------|
| CRITICAL | Block pipeline immediately |
| HIGH | Block pipeline, require security review |
| MEDIUM | Warning, create ticket |
| LOW | Log for tracking |

### 3. **Custom Rules & Policies**
```yaml
# Example: Custom Semgrep rule
rules:
  - id: no-hardcoded-passwords
    pattern: password = "..."
    message: "Hardcoded password detected"
    severity: ERROR
    languages: [python, javascript]
```

### 4. **False Positive Management**
- Use each tool's supported suppression mechanism: `.semgrepignore` / `# nosemgrep`, `.trivyignore`, and Checkov inline skip comments, `--skip-check`, or a baseline file
- Document accepted risks with an owner and an expiry date
- Review and tune rules regularly

### 5. **Harden the CI/CD Platform Itself**
- Enable branch protection, required reviews, and signed commits
- Pin third-party actions to commit SHAs and keep them updated (Dependabot/Renovate)
- Use short-lived credentials (OIDC) instead of stored secrets
- Restrict `GITHUB_TOKEN` permissions per workflow and job
- Prefer ephemeral runners; isolate self-hosted runners

### 6. **Threat Modeling & Metrics**
- Run threat modeling (e.g. STRIDE) for new services and major changes
- Track: mean time to remediate (MTTR), findings by severity, percentage of builds blocked, and false-positive rate

### 7. **Security Training**
- Secure coding training for developers
- Tool-specific workshops
- Regular security awareness sessions

---

## Learning Roadmap

### Phase 1: Foundations (Weeks 1-4)
- [ ] Learn Git and GitHub/GitLab
- [ ] Understand CI/CD concepts
- [ ] Set up Jenkins or GitHub Actions
- [ ] Basic Linux and networking

### Phase 2: Security Fundamentals (Weeks 5-8)
- [ ] OWASP Top 10
- [ ] Common vulnerabilities (SQLi, XSS, CSRF)
- [ ] Cryptography basics
- [ ] Security headers and HTTPS

### Phase 3: DevSecOps Tools (Weeks 9-12)
- [ ] TruffleHog / Gitleaks
- [ ] Semgrep / SonarQube
- [ ] Trivy / Grype
- [ ] Checkov / Trivy IaC scanning

### Phase 4: Advanced Topics (Weeks 13-16)
- [ ] Kubernetes security
- [ ] OPA / Kyverno / Policy as Code
- [ ] Supply chain security (Cosign, Syft, SLSA)
- [ ] Secrets management (Vault / AWS Secrets Manager)
- [ ] Cloud security (AWS/Azure/GCP)

### Phase 5: Certification Prep (Optional)
- [ ] Certified DevSecOps Professional (CDP)
- [ ] AWS Certified Security – Specialty
- [ ] CKA (Certified Kubernetes Administrator)
- [ ] CKS (Certified Kubernetes Security Specialist)
- [ ] OSCP (only if moving toward offensive security)

---

## Resources

### Documentation
- [OWASP DevSecOps Guideline](https://owasp.org/www-project-devsecops-guideline/)
- [CNCF Security Whitepaper](https://github.com/cncf/tag-security)
- [NIST Secure Software Development Framework (SP 800-218)](https://csrc.nist.gov/pubs/sp/800/218/final)
- [SLSA: Supply-chain Levels for Software Artifacts](https://slsa.dev)
- [OpenSSF Scorecard](https://scorecard.dev)
- [OWASP SAMM](https://owaspsamm.org)

### Practice Platforms
- [OWASP Juice Shop](https://owasp.org/www-project-juice-shop/)
- [Secure Code Warrior](https://securecodewarrior.com/)

### Communities
- [OWASP Local Chapters](https://owasp.org/chapters/)
- [Reddit r/devsecops](https://www.reddit.com/r/devsecops/)

---

## Conclusion

DevSecOps is not just about tools. It's about **culture, automation, and shared responsibility**. Start with the basics:

1. **Secrets management** (TruffleHog / Gitleaks + a secrets manager)
2. **Static analysis** (Semgrep)
3. **Dependency scanning** (Trivy)
4. **IaC security** (Checkov / Trivy)
5. **Container security** (Trivy + Cosign)
6. **Hardened CI/CD** (pinned actions, least privilege, OIDC)

Build security into your pipeline gradually, measure results, and continuously improve.

---

*Last Updated: November 2026*
*Version: 1.1*
