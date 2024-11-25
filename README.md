# Fine-Tuning and Deploying Custom Models on Amazon Bedrock

## Overview

This repository demonstrates how to fine-tune and deploy a custom language model using Amazon Bedrock, specifically focusing on medical conversation summarization using the Cohere `command-light-text-v14` model.

## Overview

This project showcases the implementation of a custom medical note generation system that:

- Fine-tunes a Cohere model on medical conversations
- Generates concise clinical notes from doctor-patient dialogues
- Demonstrates the complete workflow from data preparation to model deployment

## Prerequisites

- AWS Account with Amazon Bedrock access
- Python 3.x
- Required Python packages:
-- boto3
-- pandas
-- json
-- requests

## Project Structure

```
.
├── README.md                               # Project documentation (this file)
├── dataset/                                # Directory where fetched dataset is stored
│   └── data.csv                            # Dataset: ACI-Bench: Ambient Clinical Intelligence Benchmark
├── bedrock-custom-model-finetuning.ipynb   # Jupyter notebook with the complete workflow
```

## Workflow

1. **Data Preparation**
   - The notebook load dataset and conert it to jsonl format.
   - These abstracts are saved as a JSONL file, where each entry contains a `prompt` (the abstract text) and a placeholder `completion`.

2. **Save Dataset to S3**
   - The converted dataset is uploaded to an S3 bucket that will be used during the fine-tuning process.

3. **Create an IAM Role**
   - An IAM role is created with appropriate permissions, allowing Amazon Bedrock to access the S3 bucket for model fine-tuning.

4. **Fine-Tune the Model**
   - The notebook fine-tunes Cohere's `command-light-text-v14` model using the uploaded dataset. It uses a custom job name and configuration parameters like `batchSize`, `learningRate`, and `epochCount`.

5. **Deploy and Inference**
   - Once the fine-tuning process is complete, the custom model is used for inference. You can pass a patient-doctor conversation, and the model will return a summarized version of the text.

## Usage

### Running the Notebook

1. **Clone the repository**:
   ```bash
   git clone https://github.com/miladrezaei-ai/bedrock-custom-model-finetuning.git
   cd bedrock-custom-model-finetuning
   ```

2. **Open the notebook**:
   You can run the Jupyter notebook by using the following command:
   ```bash
   jupyter notebook bedrock-custom-model-finetuning.ipynb
   ```

3. **Follow the steps** in the notebook to fine-tune the model. Be sure to set your own AWS credentials and region.

### Example Inference

Once the fine-tuned model is deployed, you can test it by passing medical abstracts like the example below:

```python
prompt = """
[doctor] Good morning, Mr. Smith. How have you been feeling since your last visit?  
[patient] Good morning, doctor. I've been okay overall, but I’ve been struggling with persistent fatigue and some dizziness.  
[doctor] I see. Is the dizziness occurring frequently or only under specific circumstances?  
[patient] It’s mostly when I stand up quickly or after I've been walking for a while.  
[doctor] Have you noticed any changes in your heart rate or shortness of breath during these episodes?  
[patient] No shortness of breath, but I do feel my heart racing sometimes.  

[doctor] How about your medications? Are you taking them as prescribed?  
[patient] Yes, but I missed a few doses of my beta-blocker last week due to travel.  
[doctor] That could explain some of the symptoms. I’ll need to check your blood pressure and do an EKG to assess your heart rhythm.  
[patient] Okay, doctor.  

[doctor] How has your diet been? Are you still following the low-sodium plan we discussed?  
[patient] I’ve been trying, but I’ve slipped up a bit during holidays with family meals.  
[doctor] I understand. We’ll reinforce that, as it’s critical for managing your hypertension.  
[patient] Yes, I’ll make sure to get back on track.  

[doctor] Let’s discuss the results from your last bloodwork. Your cholesterol levels were slightly elevated, and your hemoglobin A1c suggests borderline diabetes.  
[patient] I see. What does that mean for me?  
[doctor] It means we need to focus on dietary changes and consider starting a low-dose statin. I’ll also refer you to a nutritionist for better meal planning.  
[patient] That makes sense. Thank you, doctor.  

[doctor] Lastly, you mentioned experiencing more frequent leg swelling recently. Is that still a concern?  
[patient] Yes, especially after long days at work.  
[doctor] That could be a sign of fluid retention. I’ll adjust your diuretic dose and monitor your progress over the next two weeks.  
[patient] Thank you, doctor.  

[doctor] All right, let’s get those tests done and review everything at our next appointment. Do you have any other concerns?  
[patient] No, I think that’s all for now.  
[doctor] Great. See you in two weeks. 
"""
```

### Expected Output:

The model should return a concise summary of the abstract.

## Important Notes

- Remember to set up provisioned throughput before performing inference
- Clean up resources after use to avoid unnecessary costs
- The small dataset size is for demonstration purposes; production use cases typically require larger datasets
 
## Cost Considerations

- Fine-tuning costs vary based on model size and training duration
- Provisioned throughput incurs ongoing costs
- S3 storage costs apply for training data and model artifacts
 
## Cleanup 
To avoid ongoing charges:

- Delete provisioned throughput
- Remove S3 buckets and objects
- Delete IAM roles and policies

## Contributing
Feel free to submit issues, fork the repository, and create pull requests for any improvements.
