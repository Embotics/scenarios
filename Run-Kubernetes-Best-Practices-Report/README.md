# Running a Kubernetes Best Practices Report with Snow® Commander®

Kubernetes is an open-source system for deploying and managing containerized applications within a hybrid or cloud environment. Using the Snow Commander cloud management platform, you can run a best practices report that compares the current state of a Kubernetes cluster against a set of checks to ensure that the resources (such as pods and containers) deployed on the cluster follow best practices and corporate standards.

This guide shows you how to use Commander 7.0.2 or greater to run a best practices report as a command workflow against a Kubernetes cluster. The best practices report is based on best practices documented by Kubernetes related to [configuration](https://kubernetes.io/docs/concepts/configuration/overview/), [security](https://kubernetes.io/blog/2016/08/security-best-practices-kubernetes-deployment), and [large clusters](https://kubernetes.io/docs/setup/cluster-large/), as well as on our own experience with Kubernetes.

This guide is intended for systems administrators, engineers and IT professionals. Previous experience with Kubernetes is required.

## Prerequisites

Before you begin, you must add a Kubernetes cluster as a cloud account. You can do this in one of two ways:

- Add an existing Kubernetes cluster as a Commander cloud account. See [Adding a Kubernetes Cloud account](https://docs.embotics.com/commander/adding-kubernetes-managed-systems.htm).
- Create a new Kubernetes cluster through Commander and have it automatically added as a Commander cloud account. To learn how, search for "Kubernetes" on our [Knowledge Base](https://support.embotics.com/support/solutions/8000051955) and choose the article for your preferred platform for deploying Kubernetes clusters.

## Install plug-in workflow step packages

This scenario uses the following plug-in workflow steps:

- Kubernetes plug-in workflow step package (`wfplugins-k8s.jar`), which provides a plug-in workflow step to add the deployed Kubernetes cluster to vCommander’s inventory as a cloud account
- Text processing plug-in workflow step package (`wfplugins-text.jar`), which provides a plug-in workflow step to manipulate text input
- Email plug-in workflow step package (`wfplugins-email.jar`), which provides a plug-in workflow step to send an email with an optional attachment

Go to [Embotics GitHub / Plug-in Workflow-Steps](https://github.com/Embotics/Plug-in-Workflow-Steps) and clone or download the repository. Then in your local version of the repo, browse to the `k8s`, `text` and `email` directories, which contain the Kubernetes, text, and email plug-in workflow step packages. 

To learn how to download and install workflow plug-in steps, see [Adding plug-in workflow steps](https://docs.embotics.com/commander/Using-Plug-In-WF-Steps.htm#Adding).

## Import the command workflow

1. Go to [Embotics Git Hub / Scenarios](https://github.com/Embotics/Scenarios) and clone or download the repository.
1. In Commander, go to **Configuration > Service Request Configuration > Completion Workflows** and click **Import**.
1. Go to the Scenarios repo that you cloned or downloaded, then from the `Run-Kubernetes-Best-Practices-Report` directory, select the `k8s-best-practices.yaml` file, and click **Open**.
   Commander automatically validates the workflow and displays the validation results in the Messages area of the Import Workflow dialog.
1. Enter a comment about the workflow in the **Description of Changes** field, and click **Import**.

​        To learn more, see [Exporting and Importing Workflow Definitions](https://docs.embotics.com/commander/exporting-and-importing-workflows.htm).

## Optional: Customize the rules and their thresholds

You can optionally customize the rules and their thresholds used by the K8s Best Practices and E-mail command workflow. These rules are defined in a separate configuration file that will be referenced by the **Kubernetes Best Practices** workflow step.

1. Go to the Scenarios repository that you cloned or downloaded, then from the `Run-Kubernetes-Best-Practices-Reports` directory, copy the `default-best-practices-rules.yaml` configuration file to a known location on the Commander server.
1. Edit the rules and their thresholds as required, then save the file.
1. Save the file to a known location on the Commander server.

## Configure the command workflow

1. In Commander, go to **Configuration > Command Workflows**, select the **K8s Best Practices and E-Mail** workflow, and click **Edit**. 
1. In the Command Workflow Configuration dialog, click **Next** to go to the Steps page.
1. Configure the **Kubernetes Best Practices** workflow step. This step runs the checks against the selected Kubernetes cluster.
    - **Rules File**: If you customized the rules and thresholds in the .yaml configuration file, enter the absolute path to the file on the Commander server, for example, `C:\temp\mybestpractices.workflow.yaml`. If no file is specified, the default set of rules is used.
1. Configure the **XSLT Transform** step. This step processes the output of the previous step. 
    - **Output**: Specify an absolute path for the output file. The specified directory must already exist.
    - **Stylesheet**: Specify the absolute path to a stylesheet file on the Commander server.
    - **Parameters**: Adjust parameter values, such as the report title, as required.
1. Configure the **Email Attachment** step. This step emails the output of the previous step. 
    - **To**: Enter one or more email addresses as required. 
    - **Subject**: Customize as required.
    - **Body**: Customize as required.
1. Click **Next**, then **Finish**. 

## Run the command workflow

1. Select a Kubernetes cluster in the Operational or Deployed view. 
1. Right-click and select **Run Workflow**.
1. In the Select a Workflow dialog, select the workflow and click **Run**.

    The report is generated and emailed to recipients.
