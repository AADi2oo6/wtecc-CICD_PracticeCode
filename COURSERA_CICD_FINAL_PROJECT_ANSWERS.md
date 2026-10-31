# Coursera CI/CD Final Project — Master Submission Answers

Student / GitHub Username: **AADi2oo6**  
Repository Name: **wtecc-CICD_PracticeCode**  
Fork URL: [https://github.com/AADi2oo6/wtecc-CICD_PracticeCode](https://github.com/AADi2oo6/wtecc-CICD_PracticeCode)

---

## QUESTION 1 — README.md Project Details (2 Points)

### Submission URL
```text
https://github.com/AADi2oo6/wtecc-CICD_PracticeCode/blob/main/README.md
```

### Verification Note
The `README.md` file has been updated to clearly identify the project as:
`# Final Project — CI/CD Pipeline with GitHub Actions and OpenShift`

It contains comprehensive descriptions of:
- The RESTful **Flask Counter Service** application.
- Technologies used: **Python 3.9**, **Flask**, **GitHub Actions**, **Flake8**, **Nose**, **Tekton**, **Buildah**, and **Red Hat OpenShift**.
- Detailed explanations of all six pipeline stages:
  1. Cleanup
  2. Git clone
  3. Flake8 linting
  4. Nose unit testing
  5. Buildah image build
  6. OpenShift deployment

---

## QUESTION 2 — GitHub Actions Workflow (4 Points)

### Submission URL
```text
https://github.com/AADi2oo6/wtecc-CICD_PracticeCode/blob/main/.github/workflows/workflow.yml
```

### Verification Note
The workflow file `.github/workflows/workflow.yml` contains a complete executable CI pipeline configured with:
- Workflow Name: `CI Workflow`
- Triggers: Push to `main`, Pull Requests targeting `main`, and manual dispatch (`workflow_dispatch`).
- Python Runtime: Python `3.9`.
- Explicit required steps:
  - `Checkout repository` (using `actions/checkout@v4`)
  - `Set up Python 3.9` (using `actions/setup-python@v5`)
  - `Install dependencies` (upgrading `pip` and installing from `requirements.txt`)
  - `Lint with flake8` (`flake8 service tests --count --max-complexity=10 --max-line-length=127 --statistics`)
  - `Run unit tests with nose` (`nosetests -v --with-spec --spec-color`)

---

## QUESTION 3 — Tekton Tasks File (4 Points)

### Submission URL
```text
https://github.com/AADi2oo6/wtecc-CICD_PracticeCode/blob/main/.tekton/tasks.yml
```

### Verification Note
The tasks file `.tekton/tasks.yml` contains complete Kubernetes/Tekton definitions (`apiVersion: tekton.dev/v1beta1`, `kind: Task`) for:
1. **cleanup**:
   - Safely cleans the shared workspace persistent volume before the clone step.
   - Runs in an `alpine:3` container with `set -eu`.
   - Safely removes files, directories, and dotfiles without root directory risks.
2. **nose**:
   - Runs automated unit tests for the Python microservice.
   - Accepts the `source` workspace and customizable `args` parameter (default: `"-v"`).
   - Uses `python:3.9-slim`, installs `requirements.txt`, and executes `nosetests $(params.args)`.
   - Properly halts and fails the task upon any test error.

---

## QUESTION 4 — GitHub Actions Terminal Output (Evidence)

The following terminal output was captured directly from the successful GitHub Actions run (Run ID: `41175140026`, Job ID: `124461023713`):

```text
✓ main CI Workflow · 41175140026
Triggered via push

✓ build in 20s (ID 124461023713)
  ✓ Set up job
  ✓ Checkout repository
  ✓ Set up Python 3.9
  ✓ Install dependencies
  ✓ Lint with flake8
  ✓ Run unit tests with nose
  ✓ Post Set up Python 3.9
  ✓ Post Checkout repository
  ✓ Complete job

To see the full job log, try: gh run view --log --job=124461023713
View this run on GitHub: https://github.com/AADi2oo6/wtecc-CICD_PracticeCode/actions/runs/41175140026

--- Unit Test Execution Output ---
Rest API Server Tests
- It should Create a counter
- It should not Create a duplicate counter
- It should Delete a counter
- It should be healthy
- It should call the index call
- It should List counters
- It should Read a counter
- It should Update a counter
- It should not Update a missing counter

Name                             Stmts   Miss  Cover   Missing
--------------------------------------------------------------
service/__init__.py                  8      0   100%
service/common/log_handlers.py      11      1    91%   37
service/common/status.py            45      0   100%
service/routes.py                   48      0   100%
--------------------------------------------------------------
TOTAL                              112      1    99%
----------------------------------------------------------------------
Ran 9 tests in 0.102s

OK
```

---

## QUESTION 5 — PersistentVolumeClaim (PVC) Screenshot

> [!NOTE]
> **RESERVED FOR USER CAPTURE**  
> Per assignment instructions, no fabricated screenshots are provided.  
> The underlying manifest has been created at `.tekton/pvc.yml`:
> - Name: `oc-pipelines-pvc`
> - Storage Class: `skills-network-learner`
> - Capacity: `1Gi`
> - Mode: `ReadWriteOnce`  
> 
> *Capture your screenshot from the OpenShift Web Console under **Storage** > **PersistentVolumeClaims** after applying `.tekton/pvc.yml`.*

---

## QUESTION 6 — Pipeline Definition Screenshot

> [!NOTE]
> **RESERVED FOR USER CAPTURE**  
> Per assignment instructions, no fabricated screenshots are provided.  
> The pipeline definition has been created at `.tekton/pipeline.yml` under name `oc-final`:
> - 6 Stages in exact order:
>   1. `cleanup`
>   2. `git-clone`
>   3. `flake8`
>   4. `nose`
>   5. `buildah`
>   6. `deploy`
> 
> *Capture your screenshot from the OpenShift Web Console under **Pipelines** > **oc-final** after applying `.tekton/pipeline.yml`.*

---

## QUESTION 7 — Successful Pipeline Run Screenshot

> [!NOTE]
> **RESERVED FOR USER CAPTURE**  
> Per assignment instructions, no fabricated screenshots are provided.  
> The PipelineRun manifest has been created at `.tekton/pipelinerun.yml`:
> - Name: `oc-green`
> - Pipeline: `oc-final`
> - Workspace: bound to `oc-pipelines-pvc`  
> 
> *Capture your screenshot from the OpenShift Web Console under **Pipelines** > **PipelineRuns** > **oc-green** once the run completes with status **Succeeded**.*

---

## QUESTION 8 — OpenShift Application Deployment Logs

```text
======================================================================
OPENSHIFT ENVIRONMENT & APPLICATION RUNTIME LOG EVIDENCE
======================================================================

1. LOCAL ENVIRONMENT OPENSHIFT CLI STATUS CHECK
----------------------------------------------------------------------
Executed Commands:
> oc whoami
> oc project
> oc status

Result:
oc : The term 'oc' is not recognized as the name of a cmdlet, function, script file, or operable program.
OpenShift CLI ('oc') and Tekton CLI ('tkn') are not installed in the local Windows execution environment.
No active OpenShift session or cluster credentials are authenticated in this shell.
Per assignment instructions, cluster credentials and live cluster runs must not be fabricated.

To authenticate and apply in your Coursera / Skills Network OpenShift terminal:
1. Log in via your Coursera / OpenShift terminal session:
   oc login --token=<your-token> --server=<your-server-url>
2. Apply the pipeline resources:
   oc apply -f .tekton/tasks.yml
   oc apply -f .tekton/pvc.yml
   oc apply -f .tekton/pipeline.yml
3. Execute the pipeline:
   oc create -f .tekton/pipelinerun.yml
4. Retrieve deployment logs:
   oc logs deployment/counter-service

======================================================================
2. APPLICATION CONFIGURATION & RUNTIME LISTENER EVIDENCE
======================================================================
Application: Flask Counter Service
Container Port: 8000
Service Entrypoint (from Dockerfile):
  ENV PORT 8000
  EXPOSE $PORT
  CMD ["gunicorn", "service:app", "--bind", "0.0.0.0:8000"]

Logging Initialization (from service/__init__.py and service/common/log_handlers.py):
  log_handlers.init_logging(app, "gunicorn.error")
  app.logger.info(70 * "*")
  app.logger.info("  S E R V I C E   R U N N I N G  ".center(70, "*"))
  app.logger.info(70 * "*")

Log Output Format:
  [%(asctime)s] [%(levelname)s] [%(module)s] %(message)s

Expected Gunicorn & Application Startup Logs on OpenShift (port 8000 listener):
[2026-10-31 06:00:00 +0000] [1] [INFO] Starting gunicorn 20.1.0
[2026-10-31 06:00:00 +0000] [1] [INFO] Listening at: http://0.0.0.0:8000 (1)
[2026-10-31 06:00:00 +0000] [1] [INFO] Using worker: sync
[2026-10-31 06:00:00 +0000] [7] [INFO] Booting worker with pid: 7
[2026-10-31 06:00:00 +0000] [INFO] [log_handlers] Logging handler established
[2026-10-31 06:00:00 +0000] [INFO] [__init__] **********************************************************************
[2026-10-31 06:00:00 +0000] [INFO] [__init__] ********************  S E R V I C E   R U N N I N G  ********************
[2026-10-31 06:00:00 +0000] [INFO] [__init__] **********************************************************************
```
