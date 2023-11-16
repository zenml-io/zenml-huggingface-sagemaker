<!-- PROJECT SHIELDS -->
<!--
*** I'm using markdown "reference style" links for readability.
*** Reference links are enclosed in brackets [ ] instead of parentheses ( ).
*** See the bottom of this document for the declaration of the reference variables
*** for contributors-url, forks-url, etc. This is an optional, concise syntax you may use.
*** https://www.markdownguide.org/basic-syntax/#reference-style-links
-->

<div align="center">

  <!-- PROJECT LOGO -->
  <br />
    <a href="https://zenml.io">
      <img alt="ZenML Logo" src="assets/header.png" alt="ZenML Logo">
    </a>
  <br />

  [![PyPi][pypi-shield]][pypi-url]
  [![PyPi][pypiversion-shield]][pypi-url]
  [![PyPi][downloads-shield]][downloads-url]
  [![Contributors][contributors-shield]][contributors-url]
  [![License][license-shield]][license-url]
  <!-- [![Build][build-shield]][build-url] -->
  <!-- [![CodeCov][codecov-shield]][codecov-url] -->

</div>

<!-- MARKDOWN LINKS & IMAGES -->
<!-- https://www.markdownguide.org/basic-syntax/#reference-style-links -->

[pypi-shield]: https://img.shields.io/pypi/pyversions/zenml?color=281158

[pypi-url]: https://pypi.org/project/zenml/

[pypiversion-shield]: https://img.shields.io/pypi/v/zenml?color=361776

[downloads-shield]: https://img.shields.io/pypi/dm/zenml?color=431D93

[downloads-url]: https://pypi.org/project/zenml/

[codecov-shield]: https://img.shields.io/codecov/c/gh/zenml-io/zenml?color=7A3EF4

[codecov-url]: https://codecov.io/gh/zenml-io/zenml

[contributors-shield]: https://img.shields.io/github/contributors/zenml-io/zenml?color=7A3EF4

[contributors-url]: https://github.com/othneildrew/Best-README-Template/graphs/contributors

[license-shield]: https://img.shields.io/github/license/zenml-io/zenml?color=9565F6

[license-url]: https://github.com/zenml-io/zenml/blob/main/LICENSE

[linkedin-shield]: https://img.shields.io/badge/-LinkedIn-black.svg?style=for-the-badge&logo=linkedin&colorB=555

[linkedin-url]: https://www.linkedin.com/company/zenml/

[twitter-shield]: https://img.shields.io/twitter/follow/zenml_io?style=for-the-badge

[twitter-url]: https://twitter.com/zenml_io

[slack-shield]: https://img.shields.io/badge/-Slack-black.svg?style=for-the-badge&logo=linkedin&colorB=555

[slack-url]: https://zenml.io/slack-invite

[build-shield]: https://img.shields.io/github/workflow/status/zenml-io/zenml/Build,%20Lint,%20Unit%20&%20Integration%20Test/develop?logo=github&style=for-the-badge

[build-url]: https://github.com/zenml-io/zenml/actions/workflows/ci.yml

<div align="center">
  <h3 align="center">Build portable, production-ready MLOps pipelines.</h3>
  <p align="center">
    <div align="center">
      Join our <a href="https://zenml.io/slack-invite" target="_blank">
      <img width="18" src="https://cdn3.iconfinder.com/data/icons/logos-and-brands-adobe/512/306_Slack-512.png" alt="Slack"/>
    <b>Slack Community</b> </a> and be part of the ZenML family.
    </div>
    <br />
    <a href="https://zenml.io/features">Features</a>
    ·
    <a href="https://zenml.io/roadmap">Roadmap</a>
    ·
    <a href="https://github.com/zenml-io/zenml/issues">Report Bug</a>
    ·
    <a href="https://zenml.io/discussion">Vote New Features</a>
    ·
    <a href="https://blog.zenml.io/">Read Blog</a>
    ·
    <a href="https://www.zenml.io/company#team">Meet the Team</a>
    <br />
  </p>
</div>

---

<!-- TABLE OF CONTENTS -->
<details>
  <summary>🏁 Table of Contents</summary>
  <ol>
    <li><a href="#-huggingface-model-to-sagemaker-endpoint-mlops-with-zenml">Introduction</a></li>
    <li><a href="#-quickstart">Quickstart</a></li>
    <li>
      <a href="#-create-your-own-mlops-platform">Create your own MLOps Platform</a>
      <ul>
        <li><a href="##-1-deploy-zenml">Deploy ZenML</a></li>
        <li><a href="#-2-deploy-stack-components">Deploy Stack Components</a></li>
        <li><a href="#-3-create-a-pipeline">Create a Pipeline</a></li>
        <li><a href="#-4-start-the-dashboard">Start the Dashboard</a></li>
      </ul>
    </li>
    <li><a href="#-roadmap">Roadmap</a></li>
    <li><a href="#-contributing-and-community">Contributing and Community</a></li>
    <li><a href="#-getting-help">Getting Help</a></li>
    <li><a href="#-license">License</a></li>
  </ol>
</details>

<br />

# 🤖 Huggingface Model to Sagemaker Endpoint: MLOps with ZenML

While almost every Huggingface model can be easily deployed to AWS Sagemaker endpoints with a few lines of code, it is often desirous to automate this flow, and have this process track the entire lineage of the model as it goes from training to production.

This project showcases one way of using [ZenML](https://zenml.io) pipelines to achieve this:

- Create and version a dataset in a `feature_engineering_pipeline`.
- Train/Finetune a BERT-based Sentiment Analysis NLP model and push to Huggingface Hub in a `training_pipeline`.
- Promote this model to `Production` by comparing to previous models in a `promotion_pipeline`.
- Deploy the model at the `Production` Stage to a [AWS Sagemaker](https://aws.amazon.com/pm/sagemaker/) endpoint with a `deployment_pipeline`.

Here is an overview of the entire process:

<img src="assets/pipelines_overview.png" alt="Pipelines Overview" width="800">

The above flow is achieved in a repeatable, fully tracked pipeline that is observable across the organization. Let's
see how this works.

## 👋 Step 0: Get started

What to do first? You can start by giving the the project a quick run. The
project is ready to be used and can run as-is without any further code
changes! You can try it right away by installing ZenML, the needed
ZenML integration and then calling the CLI included in the project.

### Install requirements

```bash
# Clone this repo
git clone git@github.com:zenml-io/zenml-huggingface-sagemaker.git
cd zenml-huggingface-sagemaker

# Set up a Python virtual environment, if you haven't already
python3 -m venv .venv
source .venv/bin/activate
# Install requirements & integrations
# Alternatively see the Makefile for commands to use
make setup
```

### Connect to a deployed ZenML and register secrets

After this, you should have ZenML and all of the requirements of the project installed locally.
Next thing to do is to connect to a [deployed ZenML instance](https://docs.zenml.io/deploying-zenml/). You can
create a free trial using [ZenML Cloud](https://cloud.zenml.io) to get setup quickly.

Once you have your deployed ZenML ready, you can connect to it using:

```shell
zenml connect --url YOUR_ZENML_SERVER_URL
```

This will open up the browser for your to connect to a deployed ZenML!

We now need to register your Huggingface API token to run this demo. This can be found in your [settings](https://huggingface.co/settings/tokens) page. Register this as a ZenML secret with:

```shell
zenml secret create huggingface_creds --username=HUGGINGFACE_USERNAME --token=HUGGINGFACE_TOKEN
```

### Set up your local stack

To run this project, you need to create a [ZenML Stack](https://docs.zenml.io/user-guide/starter-guide/understand-stacks) with the required components to run the pipelines.

```shell
make install-stack

zenml stack hf-sagekamer-local
```

### Set up AWS access

To deploy to AWS SageMaker, your local AWS client needs the necessary permissions. Ensure that you have been granted SageMaker access on your AWS account. For more information about configuring AWS for programmatic access, refer to the [AWS documentation on setting up the AWS CLI](https://docs.aws.amazon.com/cli/latest/userguide/cli-configure-quickstart.html).

Please set the appropriate environment variables for your session with the following export commands:

```shell
export AWS_ACCESS_KEY_ID=your_access_key_id
export AWS_SECRET_ACCESS_KEY=your_secret_access_key
export AWS_SESSION_TOKEN=your_session_token # if you are using temporary credentials
```

Replace `your_access_key_id`, `your_secret_access_key`, and `your_session_token` with your actual AWS credentials. These credentials will allow your local AWS client to interact securely with SageMaker and other AWS services.

## 🧑‍💻 How To Run This Project

There are two paths you can take this with the project. You can 

## 📓 Use a Jupyter notebook

```shell
# Install jupyter
pip install notebook

# Go to run.ipynb
jupyter notebook
```

## ✍️ Run it locally

If you're note the notebook type, you can use this README to run the pipelines one by one.

<details>

### 👶 Step 1: Start with feature engineering

### 💪 Step 2: Train the model

Next, you should look at the CLI help to see what you can do with the project:
  
```shell
python run.py --help

python run.py --training-pipeline --num-epochs 1 --train-batch-size 128 --eval-batch-size 12
```

This will train a model from Huggingface and register a new ZenML model on the Model Control Plane:

<img src="assets/mcp_1.png" alt="ZenML Model Control Plane" width="600">

Please note the above screens are a cloud-only feature in [ZenML Cloud](https://zenml.io/cloud), and
the CLI `zenml models list` should be used instead for OSS users.

At the end of the pipeline, the model will also be pushed the Huggingface, and a link estabilished between the ZenML Control Plane and the Huggingface model repository.

<img src="assets/hf_repo_commit.png" alt="Huggingface Repo" width="600">

<img src="assets/training_pipeline_with_hf.png" alt="Training Pipeline with HF" width="600">

Notice the linkage of the revision made on Huggingface to the metadata tracked on the ZenML pipeline. This estabilishes lineage.

### 🫅 Step 3: Promote the model 

You can run the training pipeline a few times to produce many versions of the model. Feel free to edit the parameters accordingly.
When the time is right, you now run the promotion pipeline:

```shell
python run.py --help

python run.py --promoting-pipeline
```

This pipeline finds the best model from the last pipelines that were run, and promotes it to production. That simply means its marked as production in the Model Control Plane:

<img src="assets/mcp_2.png" alt="Model versions" width="600">

### 💯 Step 4: Deploy the model

Finally, when the time is right, its time to deploy the latest `Production` model!

```shell
python run.py --deploying-pipeline
```

This uses the latest Huggingface revision, and deploys it on Sagemaker:

```shell
Creating model with name: huggingface-pytorch-inference-2023-11-08-10-33-02-272
Creating endpoint-config with name huggingface-pytorch-inference-2023-11-08-10-33-03-291
Creating endpoint with name huggingface-pytorch-inference-2023-11-08-10-33-03-291```
```

Verify that the endpoint is up:

```shell
aws sagemaker list-endpoints
```

You should see a deployed endpoint to sagemaker.

### 🏃 Step 5: Run the demo app

```shell
cd gradio
python app.py
```

<img src="assets/nlp_zenml_demo.png" alt="Demo Sentiment Analysis" width="600">

The demo has two modes: `sagemaker` and `local`. If sagemaker is selected, the client pings ZenML, finds the latest Production model, and uses the associated sagemaker endpoint to make the prediction. Otherwise, it just downloads the model and runs it locally. Naturally, the Sagemaker endpoint will usually be faster!

And there you go, you have successfully trained and pushed a model to Huggingface, and deplyoed it to AWS Sagemaker, in a ZenML pipeline. Read more on the [ZenML docs](https://docs.zenml.io)

</summary>