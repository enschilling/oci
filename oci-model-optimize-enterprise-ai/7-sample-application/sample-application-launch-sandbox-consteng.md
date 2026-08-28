# Lab 3B: Launch the Sample Application

## Introduction

In this path, you launch the pre-built Seer Construction Intelligence application on OCI Container Instances. You connect the project and both vector stores created in Labs 1 and 2, while OCI supplies short-lived resource-principal credentials to the running container. No API key, private key, or auth token is stored in the image.

Choose either Lab 3A or Lab 3B. Both paths use the same OCI Enterprise AI capabilities and continue to Labs 4 through 6.

Estimated Time: 15 minutes

### Objectives

- Review managed deployment options for OCI Enterprise AI applications
- Launch a pre-built Construction Engineering image from Cloud Shell
- Connect specification search and governed construction-data retrieval
- Confirm resource-principal authentication and application health

### Prerequisites

- The OCI Enterprise AI project OCID from Lab 1
- The completed unstructured vector store ID from Lab 1 (starts with `vs_`)
- The active semantic store OCID from Lab 2
- The **Launch helper PAR** from the Sandbox Resource List

## Task 1: Review the deployment choice

OCI Generative AI hosted applications provide a managed production path for packaging and operating agentic applications close to the service. Hosted applications are reserved for customer deployments in this workshop because the shared training tenancies cannot allocate one hosted application to every attendee at event scale.

Container Instances demonstrates the same OCI-native pattern without requiring a server or Kubernetes cluster. The public OCIR image contains application code only. Runtime OCIDs are injected as environment variables, and a pre-created dynamic group and IAM policy let the container resource principal call OCI Enterprise AI and read the workshop Vault secret.

## Task 2: Confirm readiness

1. Confirm that the construction evidence data sync job is **Succeeded**, the unstructured vector store is **Completed**, and the processed file count is greater than zero.

2. Confirm that `seer-construction-semantic` is **Active** and semantic enrichment completed successfully.

3. Keep the project OCID, unstructured vector store ID, and semantic store OCID available. These are the only values the helper asks you to enter.

## Task 3: Download the generated helper

1. Open Cloud Shell in the workshop region.

2. Copy the **Launch helper PAR** from the Sandbox Resource List and run the following commands. Replace `<launch-helper-par>` with the complete URL.

    ```bash
    <copy>
    curl -fL '<launch-helper-par>' -o launch-container-instance.sh
    chmod 700 launch-container-instance.sh
    </copy>
    ```

3. Inspect the pre-populated configuration.

    ```bash
    <copy>
    sed -n '1,45p' launch-container-instance.sh
    </copy>
    ```

    The compartment, subnet, network security group, Construction Engineering database and Vault secret, region, image URL, and resource sizing come from Terraform output.

## Task 4: Launch Seer Construction Intelligence

1. Run the helper.

    ```bash
    <copy>
    ./launch-container-instance.sh
    </copy>
    ```

2. Paste the project OCID, unstructured vector store ID, and semantic store OCID when prompted.

3. Wait for **ACTIVE**. The helper prints the Container Instance OCID and application URL.

    The instance uses 1 OCPU and 4 GB of memory. `OCI_AUTH_MODE=resource_principal` tells both the OCI SDK and OCI Generative AI authentication helper to use credentials injected and rotated by OCI.

## Task 5: Validate the application

1. Open the application URL. If the page is not ready immediately, wait 30 seconds and refresh. The image checks Streamlit's `/_stcore/health` endpoint.

2. Ask the unstructured retrieval question:

    ```text
    What structural engineering requirements are stated for the Austin project?
    ```

3. Confirm that the answer is grounded in the ingested structural engineering specification.

4. Ask the governed-data question:

    ```text
    Which suppliers are recommended for project AUS-BANK-01, and what evidence supports each recommendation?
    ```

5. Confirm that the answer uses the semantic store and ADB MCP path and identifies evidence rather than inventing approvals.

If either test fails, recheck the three entered resource identifiers and the readiness states from Task 2 before recreating the instance.

You may now proceed to Lab 4. Keep the Container Instance OCID; Lab 4 creates a routed replacement for comparison.

## Learn More

- [Overview of OCI Container Instances](https://docs.oracle.com/en-us/iaas/Content/container-instances/overview-of-container-instances.htm)
- [OCI Generative AI IAM-based authentication](https://docs.oracle.com/en-us/iaas/Content/generative-ai/oci-genai-auth.htm)
- [OCI Generative AI application deployment permissions](https://docs.oracle.com/en-us/iaas/Content/generative-ai/deploy-permissions.htm)

## Acknowledgements

- **Author** - Oracle LiveLabs
