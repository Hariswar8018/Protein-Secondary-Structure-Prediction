# TrackIO Implementation Guide

![Business Webinar (1)_page-0001](https://github.com/user-attachments/assets/fc55376a-b353-4e20-a3ed-9ec81c7f346f)

A step-by-step guide to implementing TrackIO for experiment tracking in your machine learning projects.

## Overview

TrackIO is a lightweight experiment tracking tool that integrates seamlessly with Hugging Face Spaces. This guide walks you through the complete setup and usage process.

## Prerequisites

- Kaggle account (for notebook execution)
- Hugging Face account
- Hugging Face token with write access

---

## Step 1: Installation

Install TrackIO package quietly to avoid cluttering output:

```python
!pip install trackio --quiet
```

---

## Step 2: Configure Hugging Face Authentication

Set up your Hugging Face token using Kaggle secrets:

```python
import os
from kaggle_secrets import UserSecretsClient

# Retrieve and set HF token from Kaggle secrets
os.environ["HF_TOKEN"] = UserSecretsClient().get_secret("trackio")
```

---

## Step 3: Verify Authentication

Verify your Hugging Face connection and check user details:

```python
from huggingface_hub import whoami

print("WHOAMI:", whoami())
```

This will display your Hugging Face username and email to confirm successful authentication.

---

## Step 4: Initialize TrackIO

Import TrackIO and initialize it with your project configuration:

```python
import trackio

trackio.init(
    project="my-project",
    space_id="aisakihinaki/trackio-auto-tests",
    config={
        "epochs": epochs,
        "learning_rate": 0.001,
        "batch_size": 64
    }
)
```

**Parameters:**
- `project`: Your project name
- `space_id`: Your Hugging Face Space ID (format: `username/space-name`)
- `config`: Dictionary containing hyperparameters and configuration

---

## Step 5: Log Training Metrics

Inside your training loop, log metrics for each epoch:

```python
for epoch in range(epochs):
    # Your training code here...
    
    # Log metrics
    trackio.log({
        "epoch": epoch,
        "train_loss": train_loss,
        "train_accuracy": train_acc,
        "val_loss": val_loss,
        "val_accuracy": val_acc
    })
```

You can log any metrics relevant to your experiment. Common metrics include:
- Loss values (training and validation)
- Accuracy scores
- Learning rates
- Custom evaluation metrics

---

## Step 6: Finalize Tracking

After your training loop completes, finalize the tracking session:

```python
trackio.finish()
```

This ensures all logs are properly saved and uploaded to your Hugging Face Space.

---

## Step 7: Configure Hugging Face Space

Navigate to your Hugging Face Space and ensure proper setup:

### 7.1: Check requirements.txt

Open your Space's `requirements.txt` file and verify that `trackio` is listed:

```txt
trackio
# ... other dependencies
```

If `trackio` is missing, add it to the file and save.

### 7.2: Wait for Space to Build

After adding dependencies, your Space will automatically rebuild. Wait for the build process to complete.

---

## Step 8: Access Your Results

Once the Space is ready:

1. Navigate to your Hugging Face Space URL: `https://huggingface.co/spaces/[your-username]/[space-name]`
2. View your tracked experiments and metrics
3. Copy the Space link to share or reference in reports

**Example Space URL:**
```
https://huggingface.co/spaces/aisakihinaki/trackio-auto-tests
```

---

## Complete Example

Here's a full example combining all steps:

```python
# Step 1: Install
!pip install trackio --quiet

# Step 2-3: Authentication
import os
from kaggle_secrets import UserSecretsClient
from huggingface_hub import whoami

os.environ["HF_TOKEN"] = UserSecretsClient().get_secret("trackio")
print("WHOAMI:", whoami())

# Step 4: Initialize
import trackio

trackio.init(
    project="my-project",
    space_id="aisakihinaki/trackio-auto-tests",
    config={
        "epochs": 10,
        "learning_rate": 0.001,
        "batch_size": 64
    }
)

# Step 5: Training loop with logging
for epoch in range(10):
    # Your training code...
    train_loss, train_acc = train_one_epoch()
    val_loss, val_acc = validate()
    
    trackio.log({
        "epoch": epoch,
        "train_loss": train_loss,
        "train_accuracy": train_acc,
        "val_loss": val_loss,
        "val_accuracy": val_acc
    })

# Step 6: Finish tracking
trackio.finish()

# Step 7-8: Visit your Space to view results
print("View results at: https://huggingface.co/spaces/aisakihinaki/trackio-auto-tests")
```

---

## Troubleshooting

### Common Issues

**Issue:** `trackio` module not found after installation
- **Solution:** Restart your kernel after installing trackio

**Issue:** Authentication error
- **Solution:** Verify your HF_TOKEN is correctly stored in Kaggle secrets

**Issue:** Space not updating
- **Solution:** Check that `trackio` is in requirements.txt and the Space has finished rebuilding

**Issue:** Metrics not appearing
- **Solution:** Ensure `trackio.finish()` is called after logging all metrics

---

## Best Practices

1. **Use descriptive project names** to easily identify experiments
2. **Log consistently** - include the same metrics across all epochs
3. **Call finish()** - Always finalize your tracking to ensure data is saved
4. **Version your configs** - Include all hyperparameters in the config dictionary
5. **Check your Space** - Verify the Space builds successfully after changes

---

## Additional Resources

- [TrackIO Documentation](https://github.com/trackio/trackio)
- [Hugging Face Spaces Guide](https://huggingface.co/docs/hub/spaces)
- [Kaggle Secrets Documentation](https://www.kaggle.com/docs/api#secrets)

---

## Author Information

- **Created by**: Ayusman Samasi
- **Student ID**: 22f3001994@ds.study.iitm.ac.in
- **Institution**: Indian Institute of Technology Madras (IITM)
- **Course**: Introduction to Deep Learning & Generative AI (NPPE 2)
