# Probing_Benchmarking_LLM: Automated Data Preparation Pipeline

**Probing_Benchmarking_LLM** is an automated data preparation pipeline designed specifically for fine-tuning large language models, particularly **Llama 3.2**. It includes modules for data annotation, cleaning, quality control, and batch processing.

---

## Project Structure

```
Probing_Benchmarking_LLM/
├── src/
│   ├── annotator.py
│   ├── cleaner.py
│   ├── pipeline.py
│   ├── s3_connector.py
│   ├── annotation_system/
│   │   ├── base.py
│   │   └── processors.py
│   ├── quality_control/
│   │   ├── base.py
│   │   ├── metrics.py
│   │   └── rules.py
│   └── __init__.py
├── tests/
│   ├── test_cleaner.py
│   ├── test_annotation_system.py
│   └── test_quality_control.py
├── config/
│   └── quality_config.json
├── requirements.txt
└── run.py
```

---

## Installation

### 1. Set Up the Virtual Environment

```bash
python -m venv venv
source venv/bin/activate  # Windows: venv\Scripts\activate
```

### 2. Install Dependencies

```bash
pip install -r requirements.txt
```

### 3. Install Required Models

```bash
python -m spacy download en_core_web_sm
```

---

## Configuration

### AWS Credentials

Set up your AWS credentials to enable S3 access:

```ini
# ~/.aws/credentials
[default]
aws_access_key_id = YOUR_ACCESS_KEY
aws_secret_access_key = YOUR_SECRET_KEY
```

### Quality Control Configuration

Create the file `config/quality_config.json`:

```json
{
  "min_completeness": 0.8,
  "min_consistency": 0.7,
  "min_validity": 0.8,
  "min_uniqueness": 0.9,
  "max_data_age_days": 30,
  "min_accuracy": 0.8,
  "text_length_threshold": 10,
  "min_overall_score": 0.75,
  "output_dir": "quality_reports"
}
```

---

## Core Components

### 1. Annotation System

```python
from src.annotation_system import AnnotationSystem, AnnotationConfig

config = AnnotationConfig(
    enable_entities=True,
    enable_sentiment=True,
    enable_topics=True,
    enable_keywords=True,
    enable_language=True
)

system = AnnotationSystem(config)
annotations = system.annotate_text("Your text here")
```

**Features**:

- Named Entity Recognition (NER)
- Sentiment Analysis
- Topic Detection
- Keyword Extraction
- Language Detection

---

### 2. Quality Control

```python
from src.quality_control import QualityController

controller = QualityController()
passed, issues, metrics = controller.validate_data(your_dataframe)
```

**Supported Metrics**:

- Completeness
- Consistency
- Validity
- Uniqueness
- Timeliness
- Integrity
- Accuracy

**Custom Rule Example**:

```python
from src.quality_control import SpecialCharacterRule

rule = SpecialCharacterRule()
result = rule.validate(your_dataframe)
```

---

### 3. Cleaning Pipeline

```python
from src.pipeline import AnnotationPipeline

pipeline = AnnotationPipeline(output_dir="processed_data")
pipeline.process_datasets()
```

**Pipeline Steps**:

- Data Cleaning
- Format Normalization
- Quality Validation
- Batch Processing

---

## Usage Examples

### Full Pipeline Execution

```python
from src.pipeline import AnnotationPipeline

pipeline = AnnotationPipeline(output_dir="processed_data")
pipeline.process_datasets()
```

### Quality Control Only

```python
from src.quality_control import QualityController
import pandas as pd

data = pd.read_csv("your_data.csv")
controller = QualityController()
passed, issues, metrics = controller.validate_data(data)

print(f"Passed: {passed}")
print(f"Issues: {issues}")
print(f"Overall Score: {metrics.overall_score}")
```

### Annotation Example

```python
from src.annotation_system import AnnotationSystem

system = AnnotationSystem()
text = "Apple Inc. is planning to release a new iPhone next year."
result = system.annotate_text(text)

print("Entities:", result['entities'])
print("Sentiment:", result['sentiment'])
print("Topics:", result['topics'])
```

---

## Testing

Run all tests:

```bash
python -m unittest discover tests
```

Run specific test files:

```bash
python -m unittest tests/test_cleaner.py
python -m unittest tests/test_annotation_system.py
python -m unittest tests/test_quality_control.py
```

---

## Output Structure

### Quality Reports

```
quality_reports/
├── quality_report_dataset1_20241110_123456.json
├── quality_report_dataset1_20241110_123456.txt
├── quality_report_dataset2_20241110_123457.json
└── quality_report_dataset2_20241110_123457.txt
```

### Processed Data

```
processed_data/
├── processed_dataset1.csv
├── processed_dataset1_metrics.json
├── processed_dataset2.csv
└── processed_dataset2_metrics.json
```

---

## Development Timeline

### Day 1

- Environment setup
- Core infrastructure
- Basic cleaning pipeline
- Initial tests

### Day 2

- Annotation system
- Quality control metrics
- Validation rules
- Full integration tests

---

## Contributing

To contribute:

1. Fork the repository
2. Create a new branch
3. Make your changes
4. Push to your branch
5. Submit a pull request

---

## Troubleshooting

### Import Errors

```bash
pip install -e .
```

### Model Download Errors

```bash
python -m spacy download en_core_web_sm --force
```

### Quality Control Failures

- Review reports in the `quality_reports/` folder
- Adjust thresholds in `config/quality_config.json`
- Check the logs in `pipeline.log` and `quality_control.log`
