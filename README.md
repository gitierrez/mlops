# mlops
Concepts and applications regarding the efficient development, deployment and maintenance of machine learning models.

Based on personal professional experience and Standford's [CS329S](https://stanford-cs329s.github.io/index.html) (Machine Learning Systems Design) course.

# Introduction

MLOps deals with the process of defining the **interface**, **algorithms**, **data**, **infrastructure** and **hardware** for a machine learning system to be *reliable*, *scalable*, *maintainable* and *adaptable*.

This brings a whole suite of challenges to machine learning practitioners:

* **Data testing**: how to know if a sample is good or bad for your system?
* **Data and model versioning**: how to version datasets and model checkpoints? Example: [DVC](https://github.com/iterative/dvc)
* **Monitoring**: how to know that your data distribution has shifted and you need to retrain your model? Example: [Dessa](https://www.dessa.com/)
* **Labeling**: how to quickly label new data or re-label existing data? Example: [Snorkel](https://www.snorkel.org/)
* **CI/CD**: how to run tests to make sure your model still works as expected after every change? Example: [Argo](https://argoproj.github.io/)
* **Deployment**: how to package and deploy a new model or replace an existing model? Example: [OctoML](https://octoml.ai/)
* **Model compression**: how to compress a ML model to fit onto consumer devices?
* **Inference optimization**: how to speed up inference time for your models? Example: [TensorRT](https://developer.nvidia.com/tensorrt)
* **Edge devices**: hardware designed to run ML algorithms fast and cheap. Example: [Coral SOM](https://coral.ai/products/som/)
* **Privacy**: how to use user data to train your models while preserving their privacy? Example: [PySyft](https://github.com/OpenMined/PySyft)
* **Data format**: if your samples have many features and you only want to use a subset of them, using columnar file formats like *parquet* and *ORC* are optimized for it.

# Designing Machine Learning Systems

When designing a ML system, several things need to be considered:

## 1. Inference mode: Batch vs Online

**Batch mode**

* Generates predictions periodically
* Predictions are stored somewhere (S3, SQL, CSV)
* Should be optimized for high throughput

**Online mode**

* Generates predictions as requests arrive
* Predictions are returned as request responses
* Should be optimized for low latency

## 2. Computing location: Cloud vs Edge

**Cloud computing**

* Computation is done in cloud servers
* Requires high network availability and speed
* Enables the utilization of high-capacity models

**Edge computing**

* Computation is done on-device
* Doesn't require a network connection
* Models are constrained by memory, CPU and energy of the device
* Fewer concerns about data privacy
* Saves costs by requiring fewer cloud computing resources

## 3. Learning mode: Offline vs Online

**Offline learning**

* Retrained every month or so
* Trained on large batch sizes
* Samples are repeated across epochs
* Evaluation is done offline

**Online learning**

* Retrained every few minutes
* Trained on small batch sizes
* Samples are seen at most once
* Evaluation is done mostly by A/B testing

## 4. Evaluation metrics

How to evaluate models during training and how to evaluate models during serving, when there's no hand-labeled ground truth?

**Single model, single loss**

* Easy to train and understand
* Can only optimize for a single business metric

**Single model, combined loss**

* Tuning the importance of each loss requires retraining of the model
* Enables optimization for multiple business metrics

**Combining models, each with individual loss**

* Action to take based on a weighted combination of the results of each model
* Tuning the importance of each model doesn't require retraining of the models
* Enables optimization for multiple business metrics
* Easier to maintain, one of the models might evolve quicker than the others - only that one needs to be retrained that often

## 5. Constraints

**Time constraints**

* 20% of the time to get initial system working, allows you to validate the hypoythesis and the pipeline
* 80% of the time on iterative development

**Budget constraints**

* Data availability
* Hardware resources
* Talent availability

**Performance**

* **Baselines**: existing solutions, simple solutions, human experts, competitors solutions
* **Usefulness threshold**: self-driving cars need human-level performance, predictive texting doesn't
* **Precision vs Recall**: COVID screening shouldn't have false negatives, fingerprint unlocking shouldn't have false positives
* **Interpretability**: does the model need to be interpretable? If yes, to whom?
* **Confidence measurement**: does the model need to report its confidence on a prediction? What to do with predictions below that threshold - discard it, assign to a human or ask for more information from users?

**Privacy**

* Requirements for annotation, storage, third-party solutions, cloud services and regulations
* Can data be shipped outside organizations for annotation?
* Can the system be connected to the internet?
* How long can you keep users data?