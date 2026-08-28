# Lab 4: Model Optimization

## Introduction

In this lab, you enable model routing without changing Seer Construction Intelligence code. Text-only project and supplier questions use the lower-cost model, while image prompts remain on the stronger multimodal model.

Estimated Time: 15 minutes

### Objectives

- Establish a single-model baseline
- Enable routing through application configuration
- Compare text and image workloads
- Optionally evaluate a third model

### Prerequisites

- A running application from Lab 3A or Lab 3B
- Access to both configured models in the workshop region

## Task 1: Record the baseline

1. Ask the supplier recommendation question from Lab 3 and record the model label and rough response time.

2. Attach a nonconfidential construction image and record the same observations.

## Task 2: Enable model routing

### Build path

1. In `sample-app/.env`, set `OCI_GENAI_MODEL_ROUTING_ENABLED=true`.

2. Save the file, stop Streamlit with Ctrl+C, and restart it with `streamlit run app.py`.

### Launch path

1. In Cloud Shell, return to the directory containing `launch-container-instance.sh`.

2. Create a routed replacement and enter the same three OCI Enterprise AI resource identifiers.

    ```bash
    <copy>
    MODEL_ROUTING_ENABLED=true APP_VERSION=v2 ./launch-container-instance.sh
    </copy>
    ```

3. Keep the original instance until the replacement passes the tests. Container runtime configuration is versioned by replacement so the earlier deployment remains available for comparison and rollback.

## Task 3: Test the routing policy

1. Ask:

    ```text
    Which suppliers are recommended for project AUS-BANK-01?
    ```

2. Confirm that a text-only request uses the cheaper model.

3. Attach a construction image and ask for observations separated from assumptions. Confirm that the stronger model remains selected.

4. Compare capability, latency, response depth, and token cost with the baseline.

## Task 4: Optional third-model experiment

1. Choose another text-capable model available in the workshop region.

2. In the Build path, update `OCI_GENAI_CHEAPER_MODEL` and restart the app. In the Launch path, update the generated helper's `CHEAPER_MODEL` value and launch `APP_VERSION=v3`.

3. Run the same governed-data prompt across all three candidates and record latency, evidence quality, and relative cost. Keep image input on a model that explicitly supports images.

You may now proceed to Lab 5.

## Learn More

- [OCI Generative AI models](https://docs.oracle.com/en-us/iaas/Content/generative-ai/pretrained-models.htm)

## Acknowledgements

- **Author** - Oracle LiveLabs
