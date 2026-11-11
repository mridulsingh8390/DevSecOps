# DevSecOps: Complete Guide & CI/CD Pipeline Security

## Table of Contents
1. [What is DevSecOps?](#what-is-devsecops)
2. [Why Do We Need DevSecOps?](#why-do-we-need-devsecops)
3. [DevSecOps Tools to Learn](#devsecops-tools-to-learn)
4. [CI/CD Pipeline Security Recommendations](#cicd-pipeline-security-recommendations)
5. [Sample CI/CD Pipeline Configuration](#sample-cicd-pipeline-configuration)
6. [Best Practices](#best-practices)
7. [Learning Roadmap](#learning-roadmap)

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
- Supply chain attacks (SolarWinds, Log4j)
- Cloud misconfigurations causing data breaches
- Exposed credentials in code repositories
- Container vulnerabilities

### 3. **Cost of Fixing Vulnerabilities**

| Stage | Cost Multiplier |
|-------|-----------------|
| Development | 1x |
| Testing | 5x |
| Production | 30x+ |
| Post-Breach | 100x+ |

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

### 1. **Source Code & Secret Management**

| Tool | Purpose | Type |
|------|---------|------|
| **Git** | Version control | Essential |
| **GitHub/GitLab/Bitbucket** | Code hosting & collaboration | Essential |
| **TruffleHog** | Secret detection in code & git history | Open Source |
| **Gitleaks** | Fast secret scanning | Open Source |
| **Git-secrets** | Prevent committing secrets | Open Source |

### 2. **Static Application Security Testing (SAST)**

| Tool | Purpose | Type |
|------|---------|------|
| **Semgrep** | Fast, customizable SAST | Open Source |
| **SonarQube** | Code quality + security | Open Source |
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
| **Grype** | Vulnerability scanner | Open Source |

### 5. **Infrastructure as Code (IaC) Security**

| Tool | Purpose | Type |
|------|---------|------|
| **Checkov** | IaC security scanning | Open Source |
| **Terrascan** | IaC misconfiguration detection | Open Source |
| **tfsec** | Terraform security scanner | Open Source |
| **KICS** | Infrastructure as Code scanning | Open Source |

### 6. **Container & Kubernetes Security**

| Tool | Purpose | Type |
|------|---------|------|
| **Trivy** | Container image scanning | Open Source |
| **Clair** | Container vulnerability analysis | Open Source |
| **Kube-bench** | CIS Kubernetes benchmark | Open Source |
| **Kube-hunter** | Kubernetes penetration testing | Open Source |
| **Falco** | Runtime security monitoring | Open Source |

### 7. **Policy as Code**

| Tool | Purpose | Type |
|------|---------|------|
| **Open Policy Agent (OPA)** | Policy engine | Open Source |
| **Sentinel** | HashiCorp policy framework | Commercial |
| **Conftest** | Configuration testing | Open Source |

### 8. **Supply Chain Security**

| Tool | Purpose | Type |
|------|---------|------|
| **Cosign** | Container signing | Open Source |
| **Syft** | SBOM generation | Open Source |
| **Sigstore** | Software signing & transparency | Open Source |
| **in-toto** | Software supply chain integrity | Open Source |

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

---

## CI/CD Pipeline Security Recommendations

### Pipeline Security Stages

```
┌─────────────────────────────────────────────────────────────────┐
│                    SECURE CI/CD PIPELINE                        │
├─────────────────────────────────────────────────────────────────┤
│                                                                 │
│  ┌──────────┐   ┌──────────┐   ┌──────────┐   ┌──────────┐    │
│  │  Commit  │──→│   Build  │──→│   Test   │──→│  Deploy  │    │
│  │          │   │          │   │          │   │          │    │
│  │ • Secrets│   │ • SAST   │   │ • DAST   │   │ • Sign   │    │
│  │ • Lint   │   │ • SCA    │   │ • IaC    │   │ • Verify │    │
│  │ • Pre-commit│ │ • Build  │   │ • Container│ │ • Monitor│    │
│  └──────────┘   └──────────┘   └──────────┘   └──────────┘    │
│       │              │              │              │           │
│       ▼              ▼              ▼              ▼           │
│  ┌─────────────────────────────────────────────────────────┐   │
│  │              CONTINUOUS MONITORING                      │   │
│  │  • Runtime security  • Compliance  • Threat detection   │   │
│  └─────────────────────────────────────────────────────────┘   │
│                                                                 │
└─────────────────────────────────────────────────────────────────┘
```

### Stage-by-Stage Tool Recommendations

#### Stage 1: Pre-Commit (Developer Workstation)

| Tool | Purpose | Configuration |
|------|---------|---------------|
| **pre-commit** | Framework for git hooks | Install hooks for all developers |
| **Gitleaks** | Secret detection | Run on every commit |
| **Semgrep** | Fast SAST | IDE integration + pre-commit |

**Why:** Catch issues before code leaves developer's machine.

#### Stage 2: Commit/Pull Request

| Tool | Purpose | Configuration |
|------|---------|---------------|
| **TruffleHog** | Deep secret scan | Scan full git history |
| **Semgrep** | SAST with custom rules | Block on critical findings |
| **Checkov** | IaC scanning | Scan Terraform/K8s changes |
| **Dependency Review** | Dependency changes | GitHub-native action |

**Why:** Prevent vulnerable code from merging to main branch.

#### Stage 3: Build

| Tool | Purpose | Configuration |
|------|---------|---------------|
| **Trivy** | Container image scan | Scan during image build |
| **Syft** | SBOM generation | Generate with every build |
| **Cosign** | Image signing | Sign images after build |
| **SonarQube** | Code quality gate | Enforce quality standards |

**Why:** Ensure artifacts are secure and traceable.

#### Stage 4: Test

| Tool | Purpose | Configuration |
|------|---------|---------------|
| **OWASP ZAP** | DAST scanning | Scan staging environment |
| **Nuclei** | Vulnerability scanning | Template-based scanning |
| **Trivy** | Filesystem scan | Scan repository filesystem |
| **Kube-bench** | K8s security check | Run against test clusters |

**Why:** Find runtime vulnerabilities before production.

#### Stage 5: Deploy

| Tool | Purpose | Configuration |
|------|---------|---------------|
| **OPA** | Policy enforcement | Kubernetes admission control |
| **Cosign** | Signature verification | Verify before deployment |
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

```yaml
# .github/workflows/devsecops-pipeline.yml

name: DevSecOps Pipeline

on:
  push:
    branches: [main, develop]
  pull_request:
    branches: [main]

env:
  IMAGE_NAME: myapp
  IMAGE_TAG: ${{ github.sha }}

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
        uses: trufflesecurity/trufflehog@main
        with:
          extra_args: --only-verified --exclude-paths=.trufflehogignore

  # Stage 2: SAST
  sast:
    name: Static Analysis
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4

      - name: Run Semgrep
        uses: returntocorp/semgrep-action@v1
        with:
          config: >-
            p/security-audit
            p/owasp-top-ten
            p/python
            p/javascript

  # Stage 3: SCA
  dependency-scan:
    name: Dependency Scanning
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4

      - name: Run Trivy (Filesystem)
        uses: aquasecurity/trivy-action@master
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
    steps:
      - uses: actions/checkout@v4

      - name: Run Checkov
        uses: bridgecrewio/checkov-action@master
        with:
          directory: .
          framework: terraform,kubernetes,helm
          output_format: sarif
          soft_fail: false

  # Stage 5: Build & Container Scan
  build:
    name: Build & Scan Image
    runs-on: ubuntu-latest
    needs: [secrets-scan, sast, dependency-scan, iac-scan]
    steps:
      - uses: actions/checkout@v4

      - name: Build Docker Image
        run: docker build -t ${{ env.IMAGE_NAME }}:${{ env.IMAGE_TAG }} .

      - name: Generate SBOM
        uses: anchore/sbom-action@v0
        with:
          image: ${{ env.IMAGE_NAME }}:${{ env.IMAGE_TAG }}
          format: spdx-json
          output-file: sbom.spdx.json

      - name: Scan Container Image
        uses: aquasecurity/trivy-action@master
        with:
          image-ref: ${{ env.IMAGE_NAME }}:${{ env.IMAGE_TAG }}
          severity: CRITICAL,HIGH
          exit-code: 1
          ignore-unfixed: true

      - name: Sign Image
        uses: sigstore/cosign-installer@v3

      - name: Sign with Cosign
        run: |
          cosign sign --yes ${{ env.IMAGE_NAME }}:${{ env.IMAGE_TAG }}

  # Stage 6: Deploy to Staging
  deploy-staging:
    name: Deploy to Staging
    runs-on: ubuntu-latest
    needs: [build]
    environment: staging
    steps:
      - uses: actions/checkout@v4

      - name: Verify Image Signature
        run: |
          cosign verify ${{ env.IMAGE_NAME }}:${{ env.IMAGE_TAG }}

      - name: Deploy to Kubernetes
        run: |
          kubectl apply -f k8s/staging/

      - name: Run DAST (OWASP ZAP)
        uses: zaproxy/action-full-scan@v0.9.0
        with:
          target: 'https://staging.myapp.com'

  # Stage 7: Production Deploy
  deploy-production:
    name: Deploy to Production
    runs-on: ubuntu-latest
    needs: [deploy-staging]
    environment: production
    steps:
      - name: Verify Image Signature
        run: |
          cosign verify ${{ env.IMAGE_NAME }}:${{ env.IMAGE_TAG }}

      - name: Deploy with ArgoCD
        run: |
          argocd app sync myapp-production
```

---

## Best Practices

### 1. **Start Small, Scale Gradually**
```
Week 1-2:  Secret scanning (TruffleHog)
Week 3-4:  Add SAST (Semgrep)
Week 5-6:  Add SCA (Trivy)
Week 7-8:  Add IaC scanning (Checkov)
Week 9-10: Add container scanning
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
- Maintain `.semgrepignore`, `.checkovignore`, `.trivyignore`
- Document accepted risks
- Regular rule tuning

### 5. **Security Training**
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
- [ ] Checkov / tfsec

### Phase 4: Advanced Topics (Weeks 13-16)
- [ ] Kubernetes security
- [ ] OPA / Policy as Code
- [ ] Supply chain security (Cosign, Syft)
- [ ] Cloud security (AWS/Azure/GCP)

### Phase 5: Certification Prep
- [ ] Certified DevSecOps Professional (CDP)
- [ ] AWS Security Specialty
- [ ] CKA (Certified Kubernetes Administrator)
- [ ] OSCP (Offensive Security)

---

## Resources

### Documentation
- [OWASP DevSecOps Guideline](https://owasp.org/www-project-devsecops-guideline/)
- [CNCF Security Whitepaper](https://github.com/cncf/tag-security)
- [NIST Secure Software Development Framework](https://www.nist.gov/)

### Practice Platforms
- [DevSecOps Bootcamp](https://devsecops-bootcamp.com/)
- [Katacoda DevSecOps Scenarios](https://www.katacoda.com/)
- [Secure Code Warrior](https://securecodewarrior.com/)

### Communities
- [OWASP Local Chapters](https://owasp.org/chapters/)
- [DevSecOps Community](https://devsecops.community/)
- [Reddit r/devsecops](https://www.reddit.com/r/devsecops/)

---

## Conclusion

DevSecOps is not just about tools—it's about **culture, automation, and shared responsibility**. Start with the basics:

1. **Secrets management** (TruffleHog)
2. **Static analysis** (Semgrep)
3. **Dependency scanning** (Trivy)
4. **IaC security** (Checkov)
5. **Container security** (Trivy + Cosign)

Build security into your pipeline gradually, measure results, and continuously improve.

---

*Last Updated: November 2026*
*Version: 1.0*
