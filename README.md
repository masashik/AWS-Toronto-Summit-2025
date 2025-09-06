# AWS Toronto Summit 2025 — Field Report (September 4, 2025)

## TL;DR

- I attended 4 sessions.

- DAT302: Build a cost-effective RAG-based gen Al application with Amazon Aurora
  - In this chalk talk, explore key design considerations for building cost-effective generative Al applications. Learn about model selection and optimization echniques,     focusing on strategies for optimizing the Retrieval Augmented Generation (RAG) architecture. Additionally, learn about implementing efficient data storage and            retrieval methods using Amazon Aurora. Discover prompt engineering best practices and effective monitoring and cost-tracking approaches along the way to help ensure     the overall cost-effectiveness of your generative Al solution.
    
- KUB301: Architecture patterns for MLOps on Amazon EKS
  - Amazon EKS helps organizations tackle machine learning operations (MLOps) challenges by providing a highly secure, reliable, scalable, and flexible Kubernetes            environment in the cloud. In this chalk talk, explore architecture patterns for training, fine tuning, and serving machine learning models using Amazon EKS, Amazon       EC2, and AWS storage services.
    
- AIM103: Al in action: From PoC to business value
  - More global leaders like Bundesliga and Trellix are taking their generative Al applications to production and realizing business impact through increased innovation,     productivity, and cost savings. How are they achieving these benefits and what are the key priorities that are critical for success? Through training, professional       services experts, and comprehensive generative Al offerings, AWS provides trusted guidance and tools for customers to scale with confidence. In this session, learn       how services and models like Amazon Nova, Amazon Bedrock, and Amazon Q are delivering business value across use cases to enhance customer engagement and boost            employee productivity.
    
- SEC301 : Securing your generative Al applications on AWS
  - In this code talk, discover how to secure generative Al applications using AWS services and features. Explore how to deploy a vulnerable sample generative Al             application and then layer security controls to protect, detect, and respond to security issues. Learn how to apply similar controls to the generative Al                 applications in your organization. See this all build live during this interactive session.

---

## 1) Objectives
- Understand what's important to build LLM infrastructure from recent AWS talks.
- Anything I can apply for my current LLM inference API deployment in EKS.
- Fill my gap of understanding LLM infrastructure between my LLM inference API deployment and the latest LLM inference API deployment

---

## 2) Session Notes & Takeaways

### DAT302: Build a cost-effective RAG-based gen Al application with Amazon Aurora — distilled takeaways
- **LLM** is the root cause of the cost.
- **RAG** is necessary if the context is not available in general LLM.
- **Prompt Diet** is the key to have **cost-effective** 
- Key is how to navigate the user prompt to provide ask **good** question.
<img width="572" height="459" alt="Screenshot 2025-09-05 at 5 03 43 PM" src="https://github.com/user-attachments/assets/7299cbac-2f79-413f-ac38-2272210e33ad" />
<img width="697" height="361" alt="Screenshot 2025-09-05 at 5 03 58 PM" src="https://github.com/user-attachments/assets/e77dab2a-a4fe-4c5d-a1a3-887b0e43f708" />
<img width="611" height="449" alt="Screenshot 2025-09-05 at 5 05 19 PM" src="https://github.com/user-attachments/assets/fcea949e-91f1-44ba-880b-a12cf630048a" />

### KUB301: Architecture patterns for MLOps on Amazon EKS — distilled takeaways
- **Sage Maker** is for research and development as PoC.
- **Infrastructure** starts from FSX cluster: storage, security, networking, compute, observability, and adds-on.
<img width="1006" height="527" alt="Screenshot 2025-09-05 at 6 17 23 PM" src="https://github.com/user-attachments/assets/e6188d22-7a1a-49c5-9fcc-36d53df0b604" />
<img width="890" height="748" alt="Screenshot 2025-09-05 at 8 25 10 PM" src="https://github.com/user-attachments/assets/60c2bcd7-dd63-4347-8eb5-874c1c19e6cf" />

### AIM103: Al in action: From PoC to business value
- **Business problem**: What is the problem to bring business values?
- **Data Governance**: Data needs to be **qualitative** before quantity (huge volume data).
- **AWS Bedrock/Nova/Rufus**: AWS available resource.
<img width="584" height="768" alt="Screenshot 2025-09-05 at 8 19 27 PM" src="https://github.com/user-attachments/assets/ad85fedd-659c-4fa8-a4f3-0243d20cb6e5" />

### SEC301 : Securing your generative Al applications on AWS
- **Deterministic security**: Traditional AuthN & AuthZ
- **Non-deterministic security**: AWS BedRock GuardRail.
<img width="526" height="777" alt="Screenshot 2025-09-05 at 8 38 16 PM" src="https://github.com/user-attachments/assets/f6a2f533-a5a1-49c0-aeee-f75b7d8174d6" />

---

## 3) Expo & Demo Insights (general patterns)
- **LLM Optimization for AIOps**: Most service providers are basically offering how to save money with LLMs.

---

## 4) Applying to My LLM API Server

### 4.1 Scaling & Cost
- [ ] **HPA external metrics**: drive autoscaling with request rate and **p95 latency**, not just CPU/memory.
- [ ] **Faster cold starts**: slim images, optimize layers, warm paths for hot models.
- [ ] **Cost controls**: mix Spot where safe; **nightly downsize/stop** lower environments; TTL auto-cleanup for ephemeral test stacks.

### 4.2 Observability
- [ ] **Trace IDs everywhere**: e.g., `prompt_id` in headers/logs so request → inference → response is linkable.
- [ ] **Core metrics**: success rate, token counts, per-model latency, error classes (input, retrieval miss, inference error).
- [ ] **Structured logs**: JSON logs with sampling; mask PII/secrets; align fields for queryability.

### 4.3 RAG Quality & Safety
- [ ] **Reproducible eval**: fixed datasets offline + small-traffic online A/B.
- [ ] **Guardrails & fallbacks**: thresholded confidence → re-search → model switch → templated safe reply.
- [ ] **Learn from failures**: log → tag → issue → fix → verify loop baked into CI/CD.

---

## 5) EKS vs. ECS vs. EC2 — quick guide
- **EKS**: Maximum flexibility and ecosystem integration; best for **large/multi-team** platforms; higher setup and ongoing ops cost.
- **ECS**: AWS-native and simpler; lower operational overhead; strong fit for **small-to-mid sized** inference APIs.
- **EC2**: Highest freedom (custom drivers, bespoke runtimes); good for **narrow/special** cases; heavier instance-level management.

**Decision axes**: operational effort, standardization potential, cost elasticity, and team/platform maturity.

---

## 6) Next 7 Days — Action Plan
- [ ] PoC **HPA with external metric** (tie scale-out to p95 latency).
- [ ] Add **`prompt_id` and eval tags** to logs (success/failure/re-query/fallback).
- [ ] Create a **failure-case template** (root-cause taxonomy + reproduction steps).
- [ ] **Automate cost hygiene** (nightly downsize/stop jobs for non-prod).
- [ ] Publish a short **English summary** in the README and link to this report.

---

## 7) Notes
This report focuses on **technical learnings and implementation actions** and **intentionally excludes personal networking details**.

*(Last updated: September 5, 2025)*
